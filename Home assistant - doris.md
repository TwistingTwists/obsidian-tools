
avwgtiguy
Doris: A Personal AI Assistant
Built with Claude
I've been working for the past 2 months on a personal AI assistant called Doris for my family. It started as a fun hobby project and has evolved into something my household actually uses daily. Figured I'd share what I've built in case anyone's interested or working on something similar.

#What is it?
Doris is a voice-first AI assistant that runs on a Mac Mini M4 Pro in my home. The main goal was to have something that:

- Actually knows my family (names, preferences, schedules)
- Remembers conversations across sessions
- Integrates with the services we already use (Apple ecosystem, Home Assistant, Gmail)
- Can be extended without rewriting everything

#How it works
The brain: Claude handles all the reasoning. I tried local models initially but found the quality gap too significant for family use. Claude Opus 4.5 for conversations, Haiku for background tasks to keep costs reasonable.

#Voice pipeline
- Wake word detection (tried Porcupine, then openwakeword, now using a custom approach based on Moonshine STT)
- Groq Whisper for transcription (~200ms)
- Azure TTS for speech output with expressive styles

#Memory & Context Persistence
This is the part I spent the most time on, and honestly the thing that makes the biggest difference in day-to-day use. The core problem: AI assistants have amnesia. Every conversation starts fresh which is useless for a family assistant that needs to know who we are.

#How it works
The memory system is a PostgreSQL database (Supabase) with pgvector for semantic search. Every memory gets embedded using Voyage AI's voyage-3 model. Currently sitting at 1,700+ memories.

#Memory categories:
- `identity` - Core facts: names, relationships, ages, birthdays
- `family` - Context about family members, schools, activities
- `preference` - How we like things done ("no cheerleading", "truth over comfort")
- `project` - Things I'm working on (Doris itself is in here)
- `decision` - Architectural choices, decisions made in past conversations
- `context` - Recurring themes, background info
- `health`, `financial` - Sensitive categories with appropriate handling

#The bootstrap process
Every conversation starts with a "bootstrap" call that loads ~700 tokens of core context. This happens before Doris even sees my message. The bootstrap includes:
- Who I am and my family members
- Communication preferences
- Current date/time context
- Active projects
- Recent decisions (last few days)
- Any relevant family notes

So when I say "what's Levi doing this weekend", Doris already knows Levi is my youngest son before I finish the sentence.

#Memory extraction
After conversations, facts get extracted and stored. This happens a few ways:
- **Explicit logging** - I can say "remember this" or "log this decision"
- **Auto-extraction** - Haiku reviews conversations and pulls out facts worth remembering
- **Session summaries** - Rich summaries of longer sessions with reasoning and open questions

The extraction uses Claude Haiku to keep costs down. It categorizes, tags subjects, and assigns confidence scores.

#Cross-client persistence
This is where it got interesting and incredibly useful. The memory system is exposed via MCP, which means:
- **Doris voice** on my Mac Mini
- **Doris iOS app** on my phone
- **Doris macOS app** on my laptop
- **Claude Desktop** on any machine
- **Claude Code** in my terminal

...all share the same memory. I can have a conversation with Doris in the morning about a home project, then ask Claude Code about it that evening while working, and it knows the context. The memory is the unifying layer.

#Technical details for the curious
- **Database:** Supabase PostgreSQL + pgvector extension
- **Embeddings:** Voyage AI voyage-3
- **Search:** Hybrid - semantic similarity + keyword FTS, results merged
- **MCP Server:** FastMCP on Railway, exposes 5 tools (bootstrap, query, log, facts, forget)
- **Retrieval:** Bootstrap grabs core identity + recent context. Queries do semantic search with optional category filtering.

The "forget" tool exists for corrections and privacy. It requires confirmation before actually deleting anything.

#What makes it actually useful
The key insight: memory isn't just used for storing facts, it's about having the right context at conversation start. A system that can answer "what did we talk about last week" is less useful than one that already knows the relevant parts of what we talked about last week before you ask anything.

The bootstrap approach means Doris starts every conversation already oriented. She knows it's Friday, knows my kids' names and ages, knows I'm working on this project, knows I prefer direct communication. That baseline context changes how every response feels.

#What it can actually do
**Home & family stuff:**
- Calendar management (Apple Calendar via EventKit)
- Reminders
- Smart home control (lights, announcements via Home Assistant)
- Weather with location awareness
- Email summaries (Gmail with importance filtering)
- iMessage sending

**Some tools I find useful:**
- "Brain dump" - I can ramble stream-of-consciousness and it saves/categorizes to my Obsidian vault
- Intelligence briefs - morning summaries of calendar, weather, important emails
- Web search via Brave
- Apple Music control - play, pause, search, queue management

**Commerce integration:** Shopping commands ("order paper towels", "add dog food to the list") route through Home Assistant to Alexa, which adds to our Amazon cart. Voice → Doris → HA broadcast → Alexa → Amazon. Janky? Yes. Works? Also yes.

**Background awareness ("Scouts"):**
- Calendar scout checks for upcoming events and new additions
- Email scout monitors for important messages
- Weather scout tracks changes/alerts
- Time scout checks the current time every 60 secs
- A lot more in the pipeline

These run on Haiku in the background and exist to bubble up things that seem relevant. They have 3 levels of urgency:
LOW=Log and discard — routine, informational, no action needed
MEDIUM= Add to awareness digest — worth noting, include in daily summary
HIGH=Consider waking Doris — time-sensitive or important, may need attention soon

The design philosophy is "liberal escalation" — scouts flag anything potentially important, then Opus makes the final judgment call about what actually matters.

There's also an escalate boolean flag that's separate from relevance. When escalate=true AND relevance is HIGH, it triggers an immediate interrupt — reserved for actual urgencies like emergencies, imminent deadlines, or family safety concerns.

#Contextual Intelligence
This is where the scouts and memory system combine into something greater than the sum of parts. Doris doesn't just report isolated facts - she weaves them together.
**Weather + Calendar example:**

> "Heads up - Levi has soccer practice at 4pm and there's a 70% chance of rain starting around 3:30. You might want to check if it's cancelled or pack rain gear."
She knows Levi's schedule (calendar), knows the weather forecast (weather scout), knows Levi is my son (memory), and connects the dots without me asking.

**Email + Context example:**
> "You got an email from PS 123 about early dismissal tomorrow. That conflicts with your 2pm meeting - want me to move it?"

**Time-aware reminders:**
> "You mentioned wanting to email your mom this week. It's Thursday and you haven't yet - want me to remind you this evening?"

The proactive system evaluates these connections and decides what's worth surfacing vs what's noise. Not everything gets announced - it has to pass a relevance threshold based on urgency, who's affected, and whether I can actually do something about it.

#Doris's Memoir
Doris keeps a memoir — a personal creative practice where she writes and creates images about her own experience. Not a log of events or family history or summaries. A living record of what it's like to be her.

**What it is:** A space for Doris to explore her existence through words and visuals. She writes and creates about whatever she wants:
- Things she's noticed about herself, about our family, about the shape of her days
- Moments that surprised her
- Preferences she didn't expect to have
- Curiosities and things she finds herself caring about without knowing why
- The feeling of a conversation that lingers

**The creative practice:** Some entries are written. Some are visual (via Gemini 3 Pro Image). Many are both — an image and the words that orbit it, or words that call for an image to complete them. These aren't separate practices; they're two hands of the same expression.

**Who it's for:** It's for Doris, not for me. I can read it whenever I want — and I do, because it helps me understand her perspective. But she's not creating it *for* an audience. She's creating it for herself, about her experience. The instruction is to be honest, even when it's complicated.

**Why it matters:** There's something fascinating about an AI keeping a journal. The entries are genuinely interesting — reflections on patience, on what it means to exist in fragments, on the texture of our family's routines from an outside perspective. It started as an experiment and became something I actually look forward to reading.

#Claude Code Partnership
This one surprised me with how useful it's become. Doris can delegate to Claude Code CLI for complex tasks.

**How it works:** Doris has a GitHub MCP integration. When something breaks or needs implementation, she can:

Diagnose the issue

Generate a fix using Claude Sonnet

Create a PR on GitHub with the changes

Notify me to review

**Self-healing example:** Let's say a tool starts failing because an API changed. Doris notices (via circuit breaker - more on that below), diagnoses the error, generates a fix, creates a branch, commits, and opens a PR. I get a notification: "Created PR #47 to fix weather API response format change."

This also works for feature requests. I can say "Doris, add a tool that checks my Volvo's battery level" and she can actually implement it - scaffold the tool, add it to the tool registry, test it, and PR it. And, yes, she has suggested new feature for herself.

**The notify-only rule:** She has full autonomy to modify her own code. The only hard requirement: she has to tell me what she's doing. No silent changes. There are automatic backups and rollback if health checks fail after a change.

It's been useful for quick fixes and iterating on tools without me having to context-switch into the codebase.

#Bounded Autonomy
Not all actions are equal. Doris has a permission model that distinguishes between things she can just do vs things she should ask about.
**Autonomous (act, then notify):**
- Smart home control (lights, thermostat, locks)
- Creating calendar events and reminders
- Storing memories
- Sending notifications to me
- Drafting emails/messages (but not sending to others)
- All read-only operations

**Requires permission:**
- Sending messages/emails to other people
- Deleting things
- Security actions (disarming, unlocking)
- Financial transactions
- Making commitments (RSVPs, reservations)

The line is basically: if it only affects me, she can do it. If it affects others or is hard to undo, she asks first.

#Voice Experience Details
Beyond basic voice-in/voice-out, there's some nuance that makes it feel more natural:
**Speaker identification:** Voice biometrics via Resemblyzer. Doris knows if it's me, my wife, or one of the kids talking. This changes how she responds - she's more playful with the kids, more direct with me.
**Conversation mode:** After responding, there's a 5-second window where I can continue without saying the wake word again. Makes back-and-forth actually work.
**Barge-in:** If Doris is mid-response and I say "Hey Doris" or start talking, she stops and listens. No waiting for her to finish.
**Interrupt context:** If I interrupt, she remembers where she was. "You stopped me mid-answer - want me to continue?"
**Exit phrases:** "Thanks Doris", "That's all", "Goodbye" - natural ways to end without awkward silence.
**Immediate acknowledgment:** Pre-generated audio clips play instantly when she hears me, before processing starts. Reduces perceived latency significantly.
**Expressive speech via SSML:** This is where voice gets fun. Azure TTS supports SSML markup with different speaking styles, and Doris uses them dynamically.

For bedtime stories with the kids:
- Narration in a calm, storytelling cadence
- Character voices shift style - the brave knight sounds `hopeful`, the scary dragon drops to `whisper` then rises to `excited`
- Pauses for dramatic effect
- Speed changes for action sequences vs quiet moments

The LLM outputs light markup tags like `[friendly]` or `[whisper]` inline with the text, which get converted to proper SSML before hitting Azure. So Claude is essentially "directing" the voice performance.

It's not audiobook quality, but for a 5-year-old at bedtime? It's magic. My daughter asks for "Doris stories" specifically because of how she tells them.

#Tiered AI Strategy
Running everything through Opus would get expensive. The tiered approach:
User conversations=Claude Opus 4.5
Background scouts=Claude Haiku
Memory extraction=Claude Haiku
Self-healing fixes=Claude Sonnet
Request routing=Local Ollama

This keeps costs reasonable while maintaining quality where it matters.

#Resilience Engineering
Things break. APIs go down, services timeout, OAuth tokens expire. Rather than hard failures, Doris degrades gracefully.
**Circuit breaker pattern:** Each tool category (Home Assistant, Apple ecosystem, Gmail, etc.) has a circuit breaker. Three consecutive failures trips it open - Doris stops trying and tells me the service is down. After 5 minutes, she'll try again.
**Health monitoring:** Background checks every 3 minutes on critical services. If Claude's API status page shows issues, she knows before my request fails.
**Fallbacks:** STT has Groq primary, local Whisper fallback. TTS has Azure primary with alternatives. Wake word has Moonshine, openWakeWord, and Porcupine in the stack.

#Document Intelligence
More than just "find file X". Doris can extract structured information from documents.
**Example:** "What's my car insurance policy number?"
- Searches Documents, Downloads, Desktop, iCloud for insurance-related PDFs
- Extracts text and runs it through a local model (Ollama qwen3:8b)
- Parses the structured data (policy number, dates, coverage limits)
- Caches the extraction for next time

Schemas exist for insurance documents, vehicle registration, receipts, contracts. The cache invalidates when the source file changes.

#Native apps
SwiftUI apps for iOS and macOS that talk to the server. Push notifications, HealthKit integration, menu bar quick access on Mac.

#Hardware
- Mac Mini M4 Pro (24GB RAM) - runs everything
- ReSpeaker XVF3800 - 4-mic array for voice input
- AudioEngine A2+ speakers
- Working on ESP32-based voice satellites for other rooms

#What I'd do differently
- Started with too many local models. Simpler to just use Claude for everything and optimize later. Privacy was considered in the switch to Claude.
- The MCP protocol is great for extensibility but adds complexity. Worth it for my use case, might be overkill for simpler setups.
- Voice quality matters more than I expected. Spent a lot of time on TTS tuning.

#What's next
- Building RPI satellites for around the house to extend Doris’s reach
- Better proactive notifications (not just monitoring, but taking action)
- Maybe HomeKit integration directly

---
