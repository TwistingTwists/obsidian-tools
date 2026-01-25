# DORIS Data Patterns: Two Approaches

## Problem Statement

DORIS needs to:
1. Ingest anything (meals, health metrics, moods, activities, notes...)
2. Store numeric and non-numeric types
3. Be extensible without schema migrations for new types
4. Support time-series queries (trends, aggregations)
5. Enable type-specific validations while maintaining uniform access

---

## Solution 1: Delegated Type with Trait Contracts

**Inspired by:** Rails delegated_type, strongly-typed Rust traits

### Schema

```sql
-- Primary "recordings" table (lean, uniform access)
CREATE TABLE entries (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    created_at TIMESTAMPTZ NOT NULL DEFAULT now(),
    updated_at TIMESTAMPTZ NOT NULL DEFAULT now(),

    -- Who/what created this
    user_id UUID NOT NULL REFERENCES users(id),
    source VARCHAR(50) NOT NULL,  -- 'voice', 'app', 'scout', 'api'

    -- Delegated type reference
    entry_type VARCHAR(50) NOT NULL,  -- 'meal', 'health_metric', 'mood', 'note'
    entry_id UUID NOT NULL,

    -- Denormalized for common queries
    date DATE NOT NULL DEFAULT CURRENT_DATE,

    UNIQUE(entry_type, entry_id)
);

CREATE INDEX idx_entries_user_date ON entries(user_id, date DESC);
CREATE INDEX idx_entries_type ON entries(entry_type);

-- Type-specific tables (only relevant fields)
CREATE TABLE meal_entries (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    food_description TEXT NOT NULL,
    calories INTEGER,
    protein_g DECIMAL(6,2),
    carbs_g DECIMAL(6,2),
    fat_g DECIMAL(6,2),
    meal_type VARCHAR(20),  -- 'breakfast', 'lunch', 'dinner', 'snack'
    raw_transcript TEXT,     -- Original voice input
    confidence DECIMAL(3,2)  -- LLM confidence in parsing
);

CREATE TABLE health_metrics (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    metric_type VARCHAR(50) NOT NULL,  -- 'weight', 'blood_pressure', 'steps'
    value DECIMAL(10,2) NOT NULL,
    unit VARCHAR(20) NOT NULL,
    secondary_value DECIMAL(10,2),      -- For BP: diastolic
    notes TEXT
);

CREATE TABLE mood_entries (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    mood_score INTEGER CHECK (mood_score BETWEEN 1 AND 10),
    energy_level INTEGER CHECK (energy_level BETWEEN 1 AND 10),
    tags TEXT[],
    notes TEXT
);

CREATE TABLE notes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    content TEXT NOT NULL,
    category VARCHAR(50),
    tags TEXT[]
);
```

### Rust Trait Contract

```rust
// doris-core/src/traits/recordable.rs

use async_trait::async_trait;
use sqlx::PgPool;
use uuid::Uuid;

/// Marker trait for types that can be stored as entries
#[async_trait]
pub trait Recordable: Send + Sync + Sized {
    /// Type discriminator stored in entries.entry_type
    const ENTRY_TYPE: &'static str;

    /// Insert into type-specific table, return ID
    async fn insert(&self, pool: &PgPool) -> Result<Uuid, RecordableError>;

    /// Load from type-specific table by ID
    async fn load(pool: &PgPool, id: Uuid) -> Result<Self, RecordableError>;

    /// Validate before insertion
    fn validate(&self) -> Result<(), ValidationError>;
}

/// Unified entry with loaded recordable
pub struct Entry<R: Recordable> {
    pub id: Uuid,
    pub created_at: DateTime<Utc>,
    pub user_id: Uuid,
    pub source: Source,
    pub date: NaiveDate,
    pub recordable: R,
}

impl<R: Recordable> Entry<R> {
    pub async fn create(
        pool: &PgPool,
        user_id: Uuid,
        source: Source,
        recordable: R,
    ) -> Result<Self, RecordableError> {
        recordable.validate()?;

        let entry_id = recordable.insert(pool).await?;

        let entry = sqlx::query_as!(
            EntryRow,
            r#"
            INSERT INTO entries (user_id, source, entry_type, entry_id, date)
            VALUES ($1, $2, $3, $4, CURRENT_DATE)
            RETURNING *
            "#,
            user_id,
            source.as_str(),
            R::ENTRY_TYPE,
            entry_id
        )
        .fetch_one(pool)
        .await?;

        Ok(Self {
            id: entry.id,
            created_at: entry.created_at,
            user_id: entry.user_id,
            source,
            date: entry.date,
            recordable,
        })
    }
}
```

### Implementation Example

```rust
// doris-integrations/src/diet/meal.rs

pub struct MealEntry {
    pub food_description: String,
    pub calories: Option<i32>,
    pub protein_g: Option<Decimal>,
    pub carbs_g: Option<Decimal>,
    pub fat_g: Option<Decimal>,
    pub meal_type: Option<MealType>,
    pub raw_transcript: Option<String>,
    pub confidence: Option<Decimal>,
}

#[async_trait]
impl Recordable for MealEntry {
    const ENTRY_TYPE: &'static str = "meal";

    async fn insert(&self, pool: &PgPool) -> Result<Uuid, RecordableError> {
        let row = sqlx::query!(
            r#"
            INSERT INTO meal_entries
            (food_description, calories, protein_g, carbs_g, fat_g, meal_type, raw_transcript, confidence)
            VALUES ($1, $2, $3, $4, $5, $6, $7, $8)
            RETURNING id
            "#,
            self.food_description,
            self.calories,
            self.protein_g,
            self.carbs_g,
            self.fat_g,
            self.meal_type.as_ref().map(|m| m.as_str()),
            self.raw_transcript,
            self.confidence
        )
        .fetch_one(pool)
        .await?;

        Ok(row.id)
    }

    async fn load(pool: &PgPool, id: Uuid) -> Result<Self, RecordableError> {
        sqlx::query_as!(Self, "SELECT * FROM meal_entries WHERE id = $1", id)
            .fetch_one(pool)
            .await
            .map_err(Into::into)
    }

    fn validate(&self) -> Result<(), ValidationError> {
        if self.food_description.trim().is_empty() {
            return Err(ValidationError::EmptyField("food_description"));
        }
        if let Some(cal) = self.calories {
            if cal < 0 || cal > 10000 {
                return Err(ValidationError::OutOfRange("calories", 0, 10000));
            }
        }
        Ok(())
    }
}
```

### Query Patterns

```rust
// Get all entries for a user today
let entries = sqlx::query!("SELECT * FROM entries WHERE user_id = $1 AND date = $2", user_id, today)
    .fetch_all(pool).await?;

// Get meals specifically with join
let meals = sqlx::query_as!(
    MealWithEntry,
    r#"
    SELECT e.*, m.*
    FROM entries e
    JOIN meal_entries m ON e.entry_id = m.id
    WHERE e.user_id = $1 AND e.entry_type = 'meal' AND e.date = $2
    ORDER BY e.created_at
    "#,
    user_id, today
).fetch_all(pool).await?;

// Daily calorie sum
let total = sqlx::query_scalar!(
    r#"
    SELECT COALESCE(SUM(m.calories), 0) as total
    FROM entries e
    JOIN meal_entries m ON e.entry_id = m.id
    WHERE e.user_id = $1 AND e.date = $2
    "#,
    user_id, today
).fetch_one(pool).await?;
```

### Pros
- **Type safety**: Each recordable has compile-time guarantees
- **Efficient queries**: Query `entries` for all types, join for specifics
- **Easy extension**: New type = new table + implement `Recordable` trait
- **Rich type-specific schema**: Each table optimized for its domain
- **Migrations are isolated**: Adding meal fields doesn't touch health metrics

### Cons
- **More tables**: Each type needs its own table
- **Join overhead**: Type-specific queries require joins
- **Schema migrations still needed**: For new fields on existing types

---

## Solution 2: Unified Observations (Log/Metrics Inspired)

**Inspired by:** OpenTelemetry, Prometheus, InfluxDB line protocol

### Core Insight

Observability systems handle this problem elegantly:
- **Logs**: timestamp + message + attributes (flexible)
- **Metrics**: timestamp + name + value + labels
- **Traces**: timestamp + span + attributes

What if all DORIS data follows this pattern?

### Schema

```sql
-- Single unified table for all observations
CREATE TABLE observations (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    ts TIMESTAMPTZ NOT NULL DEFAULT now(),

    -- Classification
    user_id UUID NOT NULL REFERENCES users(id),
    source VARCHAR(50) NOT NULL,
    observation_type VARCHAR(50) NOT NULL,  -- 'meal', 'metric', 'mood', 'event'
    name VARCHAR(100) NOT NULL,              -- 'calories', 'weight', 'mood_score', 'ate_breakfast'

    -- Flexible value storage (use one based on type)
    numeric_value DECIMAL(15,4),
    text_value TEXT,
    bool_value BOOLEAN,

    -- Structured attributes (flexible key-value)
    attributes JSONB NOT NULL DEFAULT '{}',

    -- Context
    date DATE NOT NULL DEFAULT CURRENT_DATE,
    tags TEXT[] DEFAULT '{}'
);

-- Time-series optimized indexes
CREATE INDEX idx_obs_user_ts ON observations(user_id, ts DESC);
CREATE INDEX idx_obs_user_date_type ON observations(user_id, date, observation_type);
CREATE INDEX idx_obs_name ON observations(name);
CREATE INDEX idx_obs_attributes ON observations USING gin(attributes);

-- Partitioning for scale (optional)
-- CREATE TABLE observations (...) PARTITION BY RANGE (ts);
```

### Rust Trait Contract

```rust
// doris-core/src/traits/observable.rs

/// Anything that can be observed and stored
pub trait Observable: Send + Sync {
    fn observation_type(&self) -> &'static str;
    fn name(&self) -> &str;
    fn to_observation(&self) -> Observation;
}

/// The unified observation record
#[derive(Debug, Clone)]
pub struct Observation {
    pub observation_type: String,
    pub name: String,
    pub numeric_value: Option<Decimal>,
    pub text_value: Option<String>,
    pub bool_value: Option<bool>,
    pub attributes: serde_json::Value,
    pub tags: Vec<String>,
}

/// Builder pattern for observations
pub struct ObservationBuilder {
    obs: Observation,
}

impl ObservationBuilder {
    pub fn metric(name: &str, value: impl Into<Decimal>) -> Self {
        Self {
            obs: Observation {
                observation_type: "metric".into(),
                name: name.into(),
                numeric_value: Some(value.into()),
                text_value: None,
                bool_value: None,
                attributes: json!({}),
                tags: vec![],
            },
        }
    }

    pub fn event(name: &str) -> Self {
        Self {
            obs: Observation {
                observation_type: "event".into(),
                name: name.into(),
                numeric_value: None,
                text_value: None,
                bool_value: None,
                attributes: json!({}),
                tags: vec![],
            },
        }
    }

    pub fn log(name: &str, message: &str) -> Self {
        Self {
            obs: Observation {
                observation_type: "log".into(),
                name: name.into(),
                numeric_value: None,
                text_value: Some(message.into()),
                bool_value: None,
                attributes: json!({}),
                tags: vec![],
            },
        }
    }

    pub fn attr(mut self, key: &str, value: impl Serialize) -> Self {
        self.obs.attributes[key] = serde_json::to_value(value).unwrap();
        self
    }

    pub fn tag(mut self, tag: &str) -> Self {
        self.obs.tags.push(tag.into());
        self
    }

    pub fn build(self) -> Observation {
        self.obs
    }
}
```

### Implementation Example (Diet Tracking)

```rust
// Recording a meal becomes multiple observations

pub async fn record_meal(
    pool: &PgPool,
    user_id: Uuid,
    food_description: &str,
    calories: i32,
    protein: Decimal,
    carbs: Decimal,
    fat: Decimal,
    meal_type: &str,
    raw_transcript: &str,
) -> Result<(), Error> {
    let now = Utc::now();
    let base_attrs = json!({
        "food_description": food_description,
        "meal_type": meal_type,
        "raw_transcript": raw_transcript,
    });

    // Record as multiple observations
    let observations = vec![
        ObservationBuilder::metric("calories", calories)
            .attr("food", food_description)
            .attr("meal_type", meal_type)
            .tag("nutrition")
            .build(),
        ObservationBuilder::metric("protein_g", protein)
            .attr("meal_type", meal_type)
            .tag("nutrition")
            .tag("macro")
            .build(),
        ObservationBuilder::metric("carbs_g", carbs)
            .attr("meal_type", meal_type)
            .tag("nutrition")
            .tag("macro")
            .build(),
        ObservationBuilder::metric("fat_g", fat)
            .attr("meal_type", meal_type)
            .tag("nutrition")
            .tag("macro")
            .build(),
        ObservationBuilder::event("meal_logged")
            .attr("food_description", food_description)
            .attr("meal_type", meal_type)
            .attr("raw_transcript", raw_transcript)
            .attr("total_calories", calories)
            .tag("meal")
            .build(),
    ];

    // Batch insert
    for obs in observations {
        sqlx::query!(
            r#"
            INSERT INTO observations
            (user_id, source, observation_type, name, numeric_value, text_value, attributes, tags)
            VALUES ($1, 'voice', $2, $3, $4, $5, $6, $7)
            "#,
            user_id,
            obs.observation_type,
            obs.name,
            obs.numeric_value,
            obs.text_value,
            obs.attributes,
            &obs.tags
        ).execute(pool).await?;
    }

    Ok(())
}
```

### Query Patterns

```rust
// Daily calories (simple aggregation)
let total = sqlx::query_scalar!(
    "SELECT COALESCE(SUM(numeric_value), 0) FROM observations
     WHERE user_id = $1 AND date = $2 AND name = 'calories'",
    user_id, today
).fetch_one(pool).await?;

// All meals today (query events)
let meals = sqlx::query!(
    "SELECT attributes FROM observations
     WHERE user_id = $1 AND date = $2 AND name = 'meal_logged'
     ORDER BY ts",
    user_id, today
).fetch_all(pool).await?;

// Trend: calories over last 7 days
let trend = sqlx::query!(
    r#"
    SELECT date, SUM(numeric_value) as total
    FROM observations
    WHERE user_id = $1 AND name = 'calories' AND date > CURRENT_DATE - INTERVAL '7 days'
    GROUP BY date
    ORDER BY date
    "#,
    user_id
).fetch_all(pool).await?;

// Flexible attribute query
let high_calorie_meals = sqlx::query!(
    r#"
    SELECT * FROM observations
    WHERE user_id = $1
      AND name = 'meal_logged'
      AND (attributes->>'total_calories')::int > 500
    "#,
    user_id
).fetch_all(pool).await?;
```

### Pros
- **Single table**: No joins, simpler migrations
- **Time-series native**: Built for trends, aggregations, windows
- **Infinite flexibility**: New "types" are just new name/attribute combos
- **Zero schema changes**: Add new observation types instantly
- **Familiar pattern**: Engineers know logs/metrics, low learning curve
- **Easy analytics**: SQL aggregations work naturally

### Cons
- **Less type safety**: JSONB attributes aren't compile-time checked
- **Denormalization**: Meal = multiple rows (calories, protein, carbs, fat, event)
- **Query complexity**: Reconstructing a "meal" requires grouping observations
- **JSONB performance**: Deep attribute queries slower than indexed columns

---

## Comparison Matrix

| Aspect | Solution 1: Delegated Type | Solution 2: Observations |
|--------|---------------------------|-------------------------|
| **Type Safety** | Strong (Rust traits) | Weak (JSONB + conventions) |
| **Schema Flexibility** | New type = new table | New type = new name string |
| **Query Simplicity** | Joins required | Single table |
| **Time-Series** | Possible but not native | Native pattern |
| **Adding Fields** | Migration per type | Just add to JSONB |
| **Validation** | Compile-time + runtime | Runtime only |
| **Storage Efficiency** | Optimized per type | Some redundancy |
| **Analytics** | Type-specific queries | Unified aggregations |
| **Learning Curve** | Rails/DDD familiar | Observability familiar |
| **Debugging** | Clear table structure | Need to understand conventions |

---

## Hybrid Recommendation

**Use both patterns for different purposes:**

```
observations (Solution 2)
├── All time-series metrics
├── Events and logs
├── Analytics and trends
└── Flexible ingestion

type-specific tables (Solution 1)
├── Complex entities needing rich queries
├── Relational data (users, households, devices)
└── Data requiring strict validation
```

### Example: Diet Tracking Hybrid

```sql
-- Solution 2: Time-series metrics
observations: calories, protein, carbs, fat as metrics

-- Solution 1: Rich meal records
meal_entries: full meal details when you need to query by food type, recipe, etc.

-- Link them
observations.attributes->>'meal_entry_id' → meal_entries.id
```

This gives you:
- Fast time-series queries on observations
- Rich domain queries on meal_entries
- Both patterns available based on use case

---

## Questions for Decision

1. **Primary query pattern?**
   - "What did I eat today?" → Delegated type (rich reconstruction)
   - "How many calories this week?" → Observations (time-series)

2. **How important is type safety?**
   - Critical for health data → Delegated type
   - Flexibility prioritized → Observations

3. **Future analytics needs?**
   - Custom dashboards, ML → Observations (uniform format)
   - Domain-specific reports → Delegated type (rich schema)

4. **Team familiarity?**
   - Rails/Django background → Delegated type
   - SRE/Observability background → Observations
