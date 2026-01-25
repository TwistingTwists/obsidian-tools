# DORIS Architecture & Project Structure

## Technology Stack

**Primary Language:** Rust
**Actor Framework:** Ractor (OTP-style supervision trees, fault tolerance)
**Architecture Pattern:** Actor model with supervision hierarchies

## Workspace Structure

```
doris/
├── Cargo.toml                # Workspace root
├── crates/
│   ├── doris-core/          # Core actor traits, message types, supervision
│   │   ├── src/
│   │   │   ├── lib.rs
│   │   │   ├── traits/      # Integration, Scout, Tool contracts
│   │   │   ├── messages/    # Message type definitions
│   │   │   ├── supervisor/  # Supervision tree primitives
│   │   │   └── health/      # Health monitoring, circuit breakers
│   │   └── Cargo.toml
│   │
│   ├── doris-memory/        # Memory system, embeddings, persistence
│   │   ├── src/
│   │   │   ├── lib.rs
│   │   │   ├── bootstrap.rs # Bootstrap context loading
│   │   │   ├── extraction.rs # Memory extraction from conversations
│   │   │   ├── storage.rs   # PostgreSQL + pgvector
│   │   │   ├── embeddings.rs # Voyage AI integration
│   │   │   └── search.rs    # Hybrid semantic + keyword search
│   │   └── Cargo.toml
│   │
│   ├── doris-voice/         # Voice pipeline
│   │   ├── src/
│   │   │   ├── lib.rs
│   │   │   ├── wake_word/   # Moonshine STT, openwakeword, Porcupine
│   │   │   ├── stt/         # Groq Whisper, local fallback
│   │   │   ├── tts/         # Azure TTS with SSML support
│   │   │   ├── speaker_id/  # Resemblyzer voice biometrics
│   │   │   └── pipeline.rs  # Orchestration of voice flow
│   │   └── Cargo.toml
│   │
│   ├── doris-integrations/  # Tool contracts & base implementations
│   │   ├── src/
│   │   │   ├── lib.rs
│   │   │   ├── traits.rs    # Integration trait definition
│   │   │   ├── circuit_breaker.rs
│   │   │   ├── apple/       # Calendar, Reminders, iMessage, EventKit
│   │   │   ├── home_assistant/ # Smart home control
│   │   │   ├── gmail/       # Email summaries, sending
│   │   │   ├── weather/     # Weather data
│   │   │   ├── music/       # Apple Music control
│   │   │   └── github/      # Self-healing PR creation
│   │   └── Cargo.toml
│   │
│   ├── doris-scouts/        # Background monitoring actors
│   │   ├── src/
│   │   │   ├── lib.rs
│   │   │   ├── traits.rs    # Scout trait definition
│   │   │   ├── calendar.rs  # Calendar monitoring
│   │   │   ├── email.rs     # Email monitoring
│   │   │   ├── weather.rs   # Weather changes
│   │   │   ├── time.rs      # Time-based checks
│   │   │   └── relevance.rs # Urgency classification (LOW/MEDIUM/HIGH)
│   │   └── Cargo.toml
│   │
│   ├── doris-runtime/       # Application runtime & supervision root
│   │   ├── src/
│   │   │   ├── lib.rs
│   │   │   ├── supervisor.rs # Root supervisor
│   │   │   ├── health.rs    # Health monitoring
│   │   │   ├── config.rs    # Configuration management
│   │   │   └── telemetry.rs # Logging, metrics
│   │   └── Cargo.toml
│   │
│   └── doris-llm/           # LLM interaction & tiered routing
│       ├── src/
│       │   ├── lib.rs
│       │   ├── claude.rs    # Opus/Sonnet/Haiku clients
│       │   ├── routing.rs   # Local Ollama for routing
│       │   ├── prompts/     # System prompts
│       │   └── streaming.rs # Response streaming
│       └── Cargo.toml
│
├── apps/
│   ├── doris-main/          # Primary household assistant
│   │   ├── src/
│   │   │   ├── main.rs
│   │   │   └── config.toml  # App-specific configuration
│   │   └── Cargo.toml
│   │
│   ├── doris-kids/          # Future: Kids-focused variant
│   │   └── Cargo.toml
│   │
│   └── doris-guest/         # Future: Guest/visitor mode
│       └── Cargo.toml
│
├── docs/
│   └── plans/               # Design documents
│
└── README.md
```

## Core Trait Contracts

### Integration Trait
```rust
// doris-core/src/traits/integration.rs
use async_trait::async_trait;
use ractor::{Actor, ActorRef};

#[async_trait]
pub trait Integration: Actor {
    type Request: Send + 'static;
    type Response: Send + 'static;

    /// Execute integration request
    async fn execute(&self, req: Self::Request) -> Result<Self::Response, IntegrationError>;

    /// Health check for circuit breaker
    async fn health_check(&self) -> HealthStatus;

    /// Get circuit breaker instance
    fn circuit_breaker(&self) -> &CircuitBreaker;

    /// Graceful degradation behavior when circuit is open
    async fn degraded_mode(&self, req: Self::Request) -> Result<Self::Response, IntegrationError> {
        Err(IntegrationError::CircuitOpen)
    }
}
```

### Scout Trait
```rust
// doris-core/src/traits/scout.rs
#[async_trait]
pub trait Scout: Actor {
    /// Monitor and produce a report
    async fn monitor(&self) -> Result<ScoutReport, ScoutError>;

    /// Classify relevance level (LOW/MEDIUM/HIGH)
    fn classify_relevance(&self, report: &ScoutReport) -> RelevanceLevel;

    /// Should this scout escalate immediately?
    fn should_escalate(&self, report: &ScoutReport) -> bool {
        matches!(report.relevance, RelevanceLevel::High) && report.escalate
    }

    /// Monitoring interval
    fn interval(&self) -> Duration {
        Duration::from_secs(180) // 3 minutes default
    }
}
```

### Tool Trait
```rust
// doris-core/src/traits/tool.rs
#[async_trait]
pub trait Tool: Send + Sync {
    type Input: Send + 'static;
    type Output: Send + 'static;

    /// Tool name for LLM tool calling
    fn name(&self) -> &str;

    /// Tool description for LLM
    fn description(&self) -> &str;

    /// JSON schema for parameters
    fn parameters_schema(&self) -> serde_json::Value;

    /// Execute the tool
    async fn execute(&self, input: Self::Input) -> Result<Self::Output, ToolError>;

    /// Does this tool require user permission?
    fn requires_permission(&self) -> bool {
        false
    }
}
```

## Supervision Hierarchy

```
RootSupervisor
├── MemorySupervisor
│   ├── BootstrapActor
│   ├── ExtractionActor
│   └── SearchActor
├── VoiceSupervisor
│   ├── WakeWordActor
│   ├── STTActor
│   ├── TTSActor
│   └── SpeakerIdentificationActor
├── IntegrationSupervisor
│   ├── AppleEcosystemActor
│   ├── HomeAssistantActor
│   ├── GmailActor
│   ├── WeatherActor
│   └── GitHubActor
├── ScoutSupervisor
│   ├── CalendarScout
│   ├── EmailScout
│   ├── WeatherScout
│   └── TimeScout
└── LLMSupervisor
    ├── OpusActor (conversations)
    ├── SonnetActor (self-healing)
    ├── HaikuActor (background tasks)
    └── RouterActor (local Ollama)
```

## Key Architectural Principles

### 1. Fault Isolation
- Each integration runs in its own actor
- Circuit breakers prevent cascading failures
- Supervision strategies: restart transient failures, escalate persistent ones

### 2. Message Priority (Ractor's 4-tier system)
1. **Signals** - Emergency shutdown (Kill)
2. **Stop** - Graceful shutdown
3. **Supervision Events** - Child lifecycle notifications
4. **Regular Messages** - Normal work (tool calls, scout reports, voice input)

### 3. Hot Code Reloading Simulation
While Rust doesn't have BEAM's hot code loading, we can achieve similar results:
- Versioned tool registry (load new tool implementations dynamically)
- Config-driven behavior changes (reload without restart)
- Blue-green deployment for critical actors

### 4. Modularity & Composition
- Apps compose different crate combinations
- Integration crates can be feature-gated
- Shared core provides consistent supervision & messaging

### 5. Type Safety & Contracts
- Traits enforce consistent behavior across integrations/scouts
- Message types are strongly typed per actor
- Compile-time guarantees for actor communication patterns

## Design Philosophy Alignment

**From original DORIS:**
- "Let it crash" → Ractor supervision trees
- Circuit breakers → Per-integration health monitoring
- Tiered AI → Different actors for Opus/Sonnet/Haiku
- Cross-client persistence → Shared memory crate
- Scouts as background actors → Natural actor model fit
- Self-healing via Claude Code → GitHub integration actor

**Rust-specific advantages:**
- Zero-cost abstractions for voice pipeline performance
- Native FFI for Apple ecosystem integration
- Strong typing prevents configuration errors
- Memory safety without GC pauses (critical for voice latency)
