# Doris (Rust + Ractor) — PRD-Level Spec (Phased)

## 0) Context snapshot (from “Home assistant - doris.md”)
Doris is a **voice-first personal AI assistant** for a household, optimized for memory, proactive context, and integrations. Key characteristics:
- Persistent memory with semantic retrieval + bootstrap context at session start
- Voice pipeline with wake word, STT, TTS, barge-in, and expressive SSML
- MCP-based shared memory across multiple clients
- Tooling for home automation, calendar, email, messaging, and more
- Background “scouts” for proactive, relevance-scored insights
- Bounded autonomy with permission gating
- Resilience patterns: circuit breakers, fallbacks, health checks

This spec translates that into a Rust-first, ractor-based architecture with a **modular, evolvable** system.

---

## 1) Product vision
**Build a household-grade, voice-first AI system that feels context-aware, reliable, and extensible**—with strong architecture and clean contracts so new applications and modules can evolve rapidly.

### Goals
- **Voice-first UX** with natural conversation flow and low perceived latency
- **Persistent household memory** with controllable categories and privacy boundaries
- **Modular architecture** with stable contracts for future apps and capabilities
- **Proactive intelligence** that surfaces relevant insights without being noisy
- **Resilience by default** (graceful degradation, circuit breakers, fallbacks)
- **Fast evolution** with minimal rewrites: clear boundaries and versioned interfaces
- **Quick pilot**: deliver one high-value integration fast to validate end-to-end flow

### Non-goals (for first phases)
- Full offline local LLM reasoning
- Multi-household SaaS product
- Consumer-grade public release

---

## 2) Core principles (engineering)
- **Contracts-first**: Every module exposes traits/interfaces with versioning
- **Actor model**: ractor used for concurrency, isolation, and supervision trees
- **Extensibility**: Plugins/skills with strict tool registration and permissions
- **Observability**: Built-in telemetry, tracing, and structured logs
- **Safety**: Permission-gated actions, explicit user confirmations for risk

---

## 3) Decisions (confirmed)
- **Deployment**: Linux-first runtime; no Mac requirement
- **LLM**: Pluggable provider interface (LiteLLM-style) with in-house Rust crate
- **Memory**: Postgres + pgvector; encrypted-at-rest
- **Voice**: Cloud STT/TTS options (Azure, Groq) for early phases
- **Privacy/Security**: Encrypted-at-rest required; strong baseline for auth/audit
- **Multi-user**: Role-based policies (kids vs adults)
- **Mobile**: Later; backend must support it now
- **Protocols**: gRPC/HTTP primary, optional MCP adapter
- **Autonomy**: Explicit forbidden actions (even with confirmation)
- **Latency/Budget**: Targets defined; not a hard block for early phases
- **Satellites**: Out of scope initially

---

## 4) Personas & key scenarios
- **Primary**: Household owner (admin). Wants reliable voice assistant and integration control.
- **Secondary**: Family members (spouse/kids). Need friendly, safe, context-aware experience.
- **Pilot user**: Mom. Wants a simple, reliable diet tracker that works by voice.

Key scenarios:
1) “What’s Levi doing this weekend?” (memory + calendar)
2) “Order paper towels” (permissioned commerce workflow)
3) Proactive reminder that calendar and weather conflict
4) “Remember this” → knowledge stored and usable later
5) “I ate two rotis and dal for lunch” → calorie estimate logged with daily totals

---

## 5) System architecture (Rust + ractor)
### 4.1 High-level services
- **Voice Service**: wake word, STT, VAD, barge-in, session loop
- **Conversation Orchestrator**: routing requests, bootstrap, tool usage
- **Memory Service**: persistent store + embeddings + retrieval
- **Tooling Service**: integration registry + permission model
- **Scout Service**: scheduled background tasks + relevance scoring
- **Identity/Policy Service**: household members, roles, permissions
- **Observability/Health**: metrics, tracing, circuit breakers, alerts

### 4.2 Actor model layout (ractor)
- **Supervisor** (root)
  - **VoiceSupervisor**
    - WakeWordActor
    - STTActor
    - TTSActor
    - AudioSessionActor
  - **ConversationSupervisor**
    - OrchestratorActor
    - BootstrapActor
    - ToolRouterActor
  - **MemorySupervisor**
    - MemoryWriteActor
    - MemoryReadActor
    - EmbeddingActor
  - **ScoutSupervisor**
    - ScoutSchedulerActor
    - ScoutWorkers
  - **IntegrationSupervisor**
    - CalendarAdapterActor
    - EmailAdapterActor
    - HomeAssistantAdapterActor
    - MessagingAdapterActor
  - **HealthSupervisor**
    - CircuitBreakerActor
    - HealthCheckActor

### 4.3 Interfaces & contracts
All modules expose traits with versioned request/response structs:
- `VoiceInputProvider` (wake, stream, barge-in)
- `SpeechToTextProvider`
- `TextToSpeechProvider`
- `LLMProvider`
- `MemoryStore` (read/write/query/forget)
- `EmbeddingProvider`
- `Tool` (invoke + schema + permissions)
- `Scout` (schedule + execute + classify)
- `IdentityStore` (users, roles, voices)

Contracts are **stable** and unit-tested. Each adapter implements a trait; all business logic depends only on trait objects.

---

## 6) Data model (first pass)
### Memory entity
- `id`, `category`, `subject`, `content`, `tags`, `confidence`, `created_at`
- `source` (explicit/auto/summary), `visibility` (family/private), `owner_id`

### Identity entity
- `id`, `name`, `role`, `voice_fingerprint`, `preferences` (structured)

### Session entity
- `session_id`, `user_id`, `start_time`, `end_time`, `summary`, `actions`

### Tool invocation log
- `tool_id`, `params`, `result`, `status`, `latency_ms`

### Nutrition log entity (pilot)
- `id`, `user_id`, `meal_time`, `items`, `estimated_calories`, `confidence`, `source`, `created_at`

---

## 7) Voice experience spec (MVP)
- **Wake word** (configurable)
- **Immediate acknowledgment** (short audio cue)
- **Barge-in** to interrupt TTS and switch to listening
- **5-second follow-up window** without wake word
- **Speaker identification** (optional/phase 2)
- **SSML rendering** from lightweight tags `[friendly]`, `[whisper]`, etc.

---

## 8) Memory & bootstrap spec
### Bootstrap flow
1) Resolve current user (voice ID or default)
2) Fetch core identity + preferences
3) Fetch recent decisions/context
4) Include time/date and active projects
5) Compose ~700 tokens

### Memory extraction
- Explicit: “remember this”, “log this decision”
- Auto: LLM summarizer extracts facts
- Session summary: stored for context

### Retrieval
- Hybrid: semantic + keyword; merge and rank
- Category filtering per query

---

## 9) Tooling & permission model
### Tool registration
- Each tool declares:
  - input schema
  - output schema
  - permissions required
  - side-effect category

### Permission levels
- **Autonomous**: safe, reversible, user-only
- **Confirm**: affects others or irreversible
- **Denied**: restricted by policy

### Example rules
- Lights/thermostat: autonomous
- Send email: confirm
- Unlock door: confirm
- Delete memory: confirm
- Nutrition logging: autonomous (user-only), with edit/undo

---

## 10) Scouts (proactive system)
- Scheduled or event-triggered background tasks
- Returns {relevance: LOW/MEDIUM/HIGH, escalate: bool}
- Relevance is post-processed by Orchestrator

Examples: calendar scout, weather scout, email scout

---

## 11) Resilience & observability
- Circuit breakers per integration category
- Health checks on critical services
- Fallbacks for STT/TTS
- Structured logs + tracing IDs per session
- Error budget in policy (max retries, timeouts)

---

## 12) Security & privacy
- Memory categories for sensitive domains (health/finance)
- Optional per-category retention rules
- Access control for family members
- Audit trail for all tool actions

---

## 13) Directory / project structure (Rust-first, evolvable)
```
/doris
  /crates
    doris-core          # traits, contracts, shared types
    doris-actors        # ractor-based orchestration logic
    doris-voice         # audio pipeline interfaces + adapters
    doris-memory        # memory store + retrieval logic
    doris-tools         # tool registry + adapters
    doris-scouts        # background tasks + scheduling
    doris-policy        # permissions + governance
    doris-obs           # metrics/tracing/logging
    doris-config        # config loader + env
  /apps
    doris-server        # primary runtime service
    doris-cli           # admin + debug interface
    doris-sim           # simulated voice sessions
  /adapters
    stt-groq
    tts-azure
    llm-anthropic
    memory-postgres
    homeassistant
    calendar-apple
  /docs
    SPEC.md
    ADRs
  /tools
    scripts
```

---

## 14) Phased delivery plan (high level)
### Phase 0 — Foundations (Linux-first core)
- Repo + crate layout with stable contracts
- ractor supervision tree scaffolding
- Config + secrets management (Linux-friendly)
- Observability baseline (tracing, metrics, logs)
- Security baseline design (auth, audit, encrypted-at-rest)
- Provider interfaces (LLM, STT, TTS, memory, tools)

**Exit criteria**: Service boots on Linux and exposes health + logs with config loaded.

### Phase 1 — Core runtime + Memory + One Integration (Diet Planner)
- Conversation orchestration loop (text-only first)
- Postgres + pgvector memory store with encryption at rest
- Bootstrap context pipeline + hybrid retrieval
- Pluggable LLM interface with one provider implementation
- Diet planner integration: parse meals, estimate calories, store in Postgres
- Admin CLI for debugging and seed data

**Exit criteria**: Text request → LLM → response; meal entries logged and queryable by day.

### Phase 2 — Voice MVP + Permissions
- Cloud STT/TTS adapters (Azure, Groq options)
- Wake word + barge-in + response window
- Tool registry + permission gating (autonomous/confirm/denied)
- Multi-user identity + policy enforcement (kids vs adults)
- Explicit forbidden actions enforced

**Exit criteria**: Voice loop works end-to-end; diet logging works by voice.

### Phase 3 — Tooling + Household Integrations
- Calendar + reminders (read/write with gating)
- Home Assistant control
- Email summaries and smart filtering
- gRPC/HTTP APIs ready for future clients

**Exit criteria**: Additional household workflows supported with policy enforcement.

### Phase 4 — Scouts + Proactive Intelligence
- Scout scheduler + relevance scoring + escalation logic
- Digest builder + high-urgency interrupt path
- Cross-signal reasoning (calendar + weather + email)

**Exit criteria**: Daily digests + urgent alerts operate reliably with minimal noise.

### Phase 5 — Resilience + Quality
- Circuit breakers, retries, fallbacks
- Health checks + status dashboard
- Latency/budget tracking + alerts

**Exit criteria**: Degradation and recovery paths tested; performance targets visible.

### Phase 6 — Expansion (later)
- Mobile clients support (backend already compatible)
- Optional MCP adapter
- Voice satellites
- Speaker ID and advanced personalization

---

## 15) Remaining open questions (to finalize later)
1) **Latency/budget targets**: Define concrete SLAs and monthly caps.
2) **Auth model**: Local-only auth vs external IdP?
3) **Key management**: How to manage encryption keys on Linux hosts?
4) **Retention rules**: Any category-specific retention or purge policy?
5) **Forbidden actions**: Final list and enforcement semantics.

---

## 16) Next step
Validate the phased plan and confirm remaining open questions. Then translate Phase 0/1 into an implementation backlog (epics + milestones).
