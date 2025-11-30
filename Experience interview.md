---
tags:
  - interview
---
Here’s a trimmed set of 20 questions, each with:
a strong answer pattern (A1) + 2 follow-ups, and
a weaker / alternative answer pattern (A2) + 2 follow-ups

So you can probe depth, boundaries, and real production experience.

---
1. What does it mean for software to be “easy to change”?
Strong answer (A1):“Easy to change means I can add or modify behavior with localized edits, low risk, and fast feedback. That usually comes from clear boundaries, good tests, and designs that depend on stable abstractions rather than concrete details.”
Follow-ups to A1:
2. “Tell me about a specific system where localized change was possible. What made a particular change surprisingly easy?”

3. “What was the last time you broke production despite thinking a change was localized? What did that reveal about your boundaries?”


Weaker/alternative answer (A2):“Easy to change means the code is clean and readable so I can quickly understand it and modify it.”
Follow-ups to A2:
1. “Can you give an example where ‘readable code’ wasn’t enough, and you still struggled to make a change safely?”

2. “How would you quantify ‘easy to change’ on a real service? What metrics or signals would you look at?”



---
2. Describe a time real-world constraints forced you to rethink a design.
Strong answer (A1):“We designed X assuming Y throughput, but production traffic spiked and latency SLOs were violated. We profiled, found the bottleneck in Z, and changed the design (e.g., added a queue, changed data model, or reindexed) to align with actual usage and constraints.”
Follow-ups to A1:
3. “What metrics or dashboards told you it was a design issue rather than an implementation bug?”

4. “If the workload pattern changed again (e.g., more writes than reads), how resilient is your new design to that?”


Weaker/alternative answer (A2):“We had performance problems, so we just upgraded the hardware and it was fine.”
Follow-ups to A2:
1. “If upgrading hardware stops being an option, how would you approach that same situation differently?”

2. “How did you know the issue wasn’t in your architecture or queries? Did you do any measurement or just rely on intuition?”



---
3. How do you distinguish essential complexity from accidental complexity?
Strong answer (A1):“Essential complexity comes from the problem domain and constraints we can’t remove (e.g., consistency requirements, business rules). Accidental complexity comes from our chosen tools, abstractions, or historical decisions. I look for things that don’t map to business concepts or constraints as candidates for removal.”
Follow-ups to A1:
4. “Give an example where you successfully removed accidental complexity. What changed for the team?”

5. “Have you ever misclassified something as ‘accidental’ only to realize it was essential? What happened?”


Weaker/alternative answer (A2):“Essential complexity is necessary code; accidental complexity is unnecessary code.”
Follow-ups to A2:
1. “In a payment system, what would you call essential complexity and what would you call accidental complexity?”

2. “Walk me through a module in your last project and point out one piece of each.”



---
4. If your system suddenly had 10x the traffic, what would you expect to fail first and why?
Strong answer (A1):“I’d look at the tightest resource: usually database connections, write throughput, or a hotspot in a shared cache. In my last system, we knew our DB was the limiting factor, so we had read replicas and backpressure ready; still, some downstream queues backed up first.”
Follow-ups to A1:
5. “How did you verify which component was truly the bottleneck, not just guessing?”

6. “If the 10x spike persisted for 6 months, what longer-term architectural changes would you make?”


Weaker/alternative answer (A2):“Things would probably slow down, but we’d scale horizontally. That’s what cloud is for.”
Follow-ups to A2:
1. “Which part would you scale first, and how do you know that’s the right one?”

2. “Can you describe a case where horizontal scaling didn’t solve the problem?”



---
5. How do you know when an assumption embedded in your system has become a liability?
Strong answer (A1):“Assumptions become liabilities when incidents, feature requests, or data patterns repeatedly clash with them. You see workarounds, conditionals everywhere, and fragile code paths. I watch for high-change hotspots and recurring bugs around the same ‘frozen’ assumptions.”
Follow-ups to A1:
6. “Describe a specific assumption that hurt you in production. How did you root-cause and untangle it?”

7. “How do you prevent ‘hidden assumptions’ from creeping in when new engineers join and ship features?”


Weaker/alternative answer (A2):“I guess we find out something is wrong when we start seeing bugs or complaints.”
Follow-ups to A2:
1. “What do you do beyond reacting to bugs? Any proactive checks or reviews to surface assumptions?”

2. “Have you ever proposed a redesign just because an assumption felt too rigid? What happened?”



---
6. How do you design APIs so they can evolve without breaking clients?
Strong answer (A1):“I version carefully, avoid leaking internal models, and design around stable concepts. I prefer additive changes, sensible defaults, and feature flags. Deprecations are announced, monitored, and we keep metrics on who’s still calling old endpoints.”
Follow-ups to A1:
7. “Tell me about a painful API change you managed. How did you coordinate and roll it out?”

8. “How do you balance keeping old versions vs pushing consumers to upgrade?”


Weaker/alternative answer (A2):“I try not to change APIs much. If I do, I just add parameters or create a v2.”
Follow-ups to A2:
1. “What happens when you need to remove or change behavior rather than just add parameters?”

2. “Have you ever had to support more than two versions at once? How did you avoid chaos?”



---
7. When is abstraction harmful?
Strong answer (A1):“It’s harmful when it hides important differences or constraints, or when it’s introduced too early and locks us into a mental model. For example, generic repositories that hide query patterns made performance tuning harder in one project.”
Follow-ups to A1:
8. “Tell me about a specific abstraction you had to rip out. What did that reveal about your design process?”

9. “How do you decide when an abstraction has earned its existence vs being premature?”


Weaker/alternative answer (A2):“It’s harmful when it’s over-engineered or too complicated.”
Follow-ups to A2:
1. “Give a production example where you saw this. What concrete harm did it cause?”

2. “How do you detect this early, before the abstraction spreads across the codebase?”



---
8. What’s the difference between good coupling and bad coupling?
Strong answer (A1):“Good coupling happens along stable, cohesive boundaries—things that really belong together (like domain invariants). Bad coupling is when unrelated concerns depend on each other, or when many modules depend on volatile details of another.”
Follow-ups to A1:
9. “Can you show an example where you intentionally increased coupling and it was a good tradeoff?”

10. “When did unexpected coupling show up during a refactor or outage, and how did you fix it?”


Weaker/alternative answer (A2):“Good coupling is loose coupling; bad coupling is tight coupling.”
Follow-ups to A2:
1. “Can you walk through a module you wrote and identify a dependency that you consider ‘good’ and another that’s ‘bad’?”

2. “How do you ensure that ‘loose coupling’ doesn’t turn into ‘no structure and everything is everywhere’?”



---
9. A PM asks for a “small feature” that introduces cross-module dependencies. What do you do?
Strong answer (A1):“I first clarify what outcome they need. If it cuts across boundaries, I look for a way to achieve it through existing interfaces or by introducing a better integration point. If it truly requires boundary changes, I communicate the technical debt and maybe split it into short-term and long-term steps.”
Follow-ups to A1:
10. “Tell me about a time you said ‘no’ or ‘not like this’. How did you convince stakeholders?”

11. “Have you ever accepted a ‘hack’ in production? Under what conditions, and how did you clean it up?”


Weaker/alternative answer (A2):“If it’s small and urgent, I just implement it quickly to keep the PM happy.”
Follow-ups to A2:
1. “What’s an example where this approach later caused problems? How costly was the cleanup?”

2. “How would you change your approach if that ‘small’ feature needed to be extended three more times?”



---
10. You inherit a tangled monolith. Where do you start if the goal is to make change cheaper?
Strong answer (A1):“I start with understanding critical flows, add monitoring/tests around them, and identify hotspots of frequent change. Then I gradually introduce seams: modules, boundaries, or well-defined APIs, often by refactoring inside the monolith before any split.”
Follow-ups to A1:
11. “Give an example of a ‘seam’ you introduced and how it improved things.”

12. “How did you avoid slowing feature delivery while refactoring such a system?”


Weaker/alternative answer (A2):“I’d probably start splitting microservices right away around different features.”
Follow-ups to A2:
1. “How would you avoid just distributing the existing complexity over the network?”

2. “What data or metrics would guide you in deciding which pieces to extract first?”



---
11. How do you reason about adding caching, sharding, or queues to a system?
Strong answer (A1):“I start with the bottleneck: latency, throughput, or contention. Caches add complexity (staleness, eviction), sharding affects data access patterns and rebalancing, and queues add backpressure but also eventual consistency. I only add them when I can clearly articulate the new failure modes.”
Follow-ups to A1:
12. “Tell me about a caching or queuing change that caused a subtle production bug.”

13. “If your shard key assumption turned out wrong, what’s your migration plan?”


Weaker/alternative answer (A2):“I use caches for speed, sharding for scale, and queues for async work. They’re standard patterns.”
Follow-ups to A2:
1. “Describe a situation where adding a cache made things worse. What did you learn?”

2. “What are the specific risks of adding a queue between two services?”



---
12. What does a good test strategy look like for a fast-changing system?
Strong answer (A1):“A thin layer of high-value unit tests around core logic, contract tests between services, and a few critical-path end-to-end tests. Plus observability in production. Tests should describe behavior, not implementation, so refactors are cheap.”
Follow-ups to A1:
13. “Tell me about a refactor where your tests gave you confidence—and one where they got in your way.”

14. “Have you ever deliberately deleted tests? Why?”


Weaker/alternative answer (A2):“We just write unit tests for each function and some integration tests. More coverage is always better.”
Follow-ups to A2:
1. “Have you seen 90%+ coverage still fail to catch important bugs? Why do you think that happened?”

2. “If tests start slowing down development, what would you change?”



---
13. What’s the difference between tests that encourage change and tests that block change?
Strong answer (A1):“Encouraging tests assert behavior at stable boundaries and tolerate internal reshuffling. Blocking tests are tightly coupled to implementation details (e.g., specific private methods, log lines) and break with any refactor.”
Follow-ups to A1:
14. “Give an example of a test you rewrote because it was overfitted to the implementation.”

15. “How do you teach teammates to write behavior-focused tests?”


Weaker/alternative answer (A2):“Tests that pass are good. Tests that fail often are blocking change.”
Follow-ups to A2:
1. “Have you seen flakiness or brittle tests? What patterns caused that?”

2. “If you inherit a suite of brittle tests, what’s your plan—delete, refactor, or rewrite?”



---
14. How do you prevent architecture from becoming a bottleneck for team autonomy?
Strong answer (A1):“I try to create clear, stable contracts and ownership boundaries so teams can move independently inside them. Central architecture provides constraints and guardrails, not detailed blueprints. We also use shared libraries/platforms to solve cross-cutting concerns.”
Follow-ups to A1:
15. “Describe a time when you were the bottleneck. How did you change the setup?”

16. “How do you detect when a boundary is too rigid and is slowing down product delivery?”


Weaker/alternative answer (A2):“I just let teams choose whatever they want so they’re not blocked.”
Follow-ups to A2:
1. “How do you handle the operational burden of multiple stacks and patterns in production?”

2. “Have you ever had to standardize after too much freedom? What broke first?”



---
15. If you had to prioritize one: correctness, simplicity, or flexibility—what and why?
Strong answer (A1):“In most production systems, I’d prioritize correctness first—wrong results can be catastrophic. But I try to achieve correctness through simplicity, and I’m cautious about adding flexibility that isn’t driven by real needs.”
Follow-ups to A1:
16. “Tell me about a time you chose simplicity over flexibility. How did that play out later?”

17. “Have you ever sacrificed short-term correctness for learning speed, like in experiments?”


Weaker/alternative answer (A2):“I’d pick flexibility, because requirements always change.”
Follow-ups to A2:
1. “Can you share a case where flexibility you built was never used, or made things harder?”

2. “How do you prevent flexible systems from becoming too complex to reason about?”



---
16. What are signs that a piece of software was built by someone who deeply understood the system?
Strong answer (A1):“You see clear modeling of domain concepts, sensible defaults, sharp edges around dangerous operations, and good observability hooks. There’s a ‘just enough’ feeling—no gratuitous abstractions, but obvious extensibility points.”
Follow-ups to A1:
17. “Point to a real module/service you’ve seen that felt like this. What details impressed you?”

18. “Compare that to something you’ve built. Where do you feel you’re not at that level yet?”


Weaker/alternative answer (A2):“The code is very clean and uses advanced patterns or frameworks well.”
Follow-ups to A2:
1. “Have you seen ‘fancy’ code that was actually a sign of misunderstanding? What happened?”

2. “In production, what matters more than ‘advanced patterns’? How do you spot that?”



---
17. Tell me about a production incident where your system failed under load. What did you learn?
Strong answer (A1):“We had an outage when X increased; Y component saturated, causing cascading failures. We analyzed metrics and traces, implemented backpressure and retries with jitter, and adjusted our capacity planning. We also simplified a hot path that was doing unnecessary work.”
Follow-ups to A1:
18. “What did you change in your design process after that, not just the code?”

19. “If the same pattern reappeared in a different service, how would you catch it earlier?”


Weaker/alternative answer (A2):“We just had downtime because traffic was high. We increased capacity and it went away.”
Follow-ups to A2:
1. “Did you run a postmortem? What were the root causes beyond ‘traffic was high’?”

2. “What alarms or automated safeguards did you add after that, if any?”



---
18. How do you decide if a refactor is “worth it” in a production system?
Strong answer (A1):“I look at change frequency, defect rate, and friction for new features in that area. If many people touch it often and each change is risky or slow, refactoring pays off. I also scope refactors to be incremental and tied to real business work.”
Follow-ups to A1:
19. “Describe a refactor you deliberately did not do. Why not?”

20. “Have you ever started a refactor that you had to stop halfway? What went wrong?”


Weaker/alternative answer (A2):“I refactor when I have time or when I see messy code.”
Follow-ups to A2:
1. “How do you avoid ‘drive-by refactors’ that delay shipping or introduce risk?”

2. “Can you recall a refactor that caused a regression? Why do you think that happened?”



---
19. How do you roll out a risky change in a critical production system?
Strong answer (A1):“I break it into small steps, behind feature flags or config, use canary or phased rollouts, and have clear rollback paths. I define success/failure metrics in advance and monitor closely.”
Follow-ups to A1:
20. “Tell me about a specific rollout where you did roll back. What made you pull the trigger?”

21. “How do you ensure that configuration or feature-flag complexity doesn’t become its own risk?”


Weaker/alternative answer (A2):“I test thoroughly locally and in staging, then deploy in one go once everything passes.”
Follow-ups to A2:
1. “Have you seen issues that only appear at real scale or with real traffic patterns?”

2. “What would you do differently if a rollback is difficult or high-cost?”



---
20. How do you handle a major non-functional requirement change, like ‘cut latency by 50%’?
Strong answer (A1):“I’d start with measurement: where is the latency coming from? Then prioritize changes along the critical path: reduce synchronous calls, optimize queries, add caching where safe, or change protocols. I’d also negotiate what can be traded off—consistency, features, or cost.”
Follow-ups to A1:
21. “Describe a real performance or reliability mandate you handled. What tradeoffs did you actually make?”

22. “What if the requirement conflicts with a previous design principle you set, like strict consistency?”


Weaker/alternative answer (A2):“I’d try to optimize the code and maybe upgrade servers or use a faster framework.”
Follow-ups to A2:
1. “How do you know which optimization matters most instead of micro-optimizing randomly?”

2. “If infra upgrades are not allowed, what’s your plan B and C?”



---
If you’d like, next step we can:
pick 5–8 of these and run a mock interview,
I’ll act as interviewer, you answer, and I’ll follow up with the relevant deep-dive questions from this list and challenge your assumptions.