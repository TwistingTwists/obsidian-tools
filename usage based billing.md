Short version: Fly.io’s biggest usage-based billing headaches are **complex metering, confusing invoices (especially bandwidth), free-tier expectations, and migrating pricing models without breaking trust**. I’ll lay those out, then turn them into concrete projects you can do.

---

## 1. What problems is a platform like Fly.io actually facing?

**Context:**  
Fly.io runs apps close to users across many regions. They charge based on usage of compute, storage, bandwidth, IPs and support tiers – mostly pure pay-as-you-go with some legacy plans still around. ([fly.io](https://fly.io/docs/about/pricing/?utm_source=chatgpt.com "Fly.io Resource Pricing · Fly Docs"))

That sounds clean… until you look at what it means in billing.

---

### A. Multi-dimensional metering → invoices that are hard to read

From their Metronome case study:

- Every customer is an **organization**.
    
- Each org can have **many apps**.
    
- Each app can run in **multiple regions** and use:
    
    - different **machine types / CPU counts**
        
    - **bandwidth / data transfer**
        
    - **volumes / storage**
        
    - other features & discounts. ([metronome.com](https://metronome.com/blog/how-fly-io-solves-usage-based-billing-challenges-with-metronome?utm_source=chatgpt.com "How Fly.io Solves Usage-Based Billing Challenges With Metronome"))
        

This means one invoice can have **thousands of line items**, and users struggle to answer:

> “Where did my money go, exactly?” ([metronome.com](https://metronome.com/blog/how-fly-io-solves-usage-based-billing-challenges-with-metronome?utm_source=chatgpt.com "How Fly.io Solves Usage-Based Billing Challenges With Metronome"))

Internally, Fly.io first tried to:

- store usage events in TimescaleDB
    
- bill via Stripe by aggregating that usage.
    

They hit two big problems:

1. **Usage vs pricing data were decoupled**, so breaking down total costs into clear per-app/region numbers was painful.
    
2. Stripe couldn’t express all the dimensions they were metering. ([metronome.com](https://metronome.com/blog/how-fly-io-solves-usage-based-billing-challenges-with-metronome?utm_source=chatgpt.com "How Fly.io Solves Usage-Based Billing Challenges With Metronome"))
    

This is _core_ usage-based billing pain: **collecting tons of granular signals, then turning them into a legible, trustworthy invoice**.

---

### B. Pricing changes & migrations are scary

Fly.io keeps evolving pricing:

- region-based pricing for machines and discounts by region or contract. ([metronome.com](https://metronome.com/blog/how-fly-io-solves-usage-based-billing-challenges-with-metronome?utm_source=chatgpt.com "How Fly.io Solves Usage-Based Billing Challenges With Metronome"))
    
- machine reservation blocks with long-term discounts. ([metronome.com](https://metronome.com/blog/how-fly-io-solves-usage-based-billing-challenges-with-metronome?utm_source=chatgpt.com "How Fly.io Solves Usage-Based Billing Challenges With Metronome"))
    
- a shift from old mixed “Hobby / Launch / Scale + free allowances” to pay-as-you-go plus support tiers. ([srvrlss.io](https://www.srvrlss.io/blog/fly-io-pay-as-you-go/?utm_source=chatgpt.com "Fly.io's New 2024 Pricing Model: Pay As You Go - srvrlss.io"))
    
- newer granular bandwidth billing with different prices for private vs public transfer and per-hop charges. ([Fly.io](https://community.fly.io/t/opt-in-to-granular-bandwidth-billing/21099?utm_source=chatgpt.com "Opt-in to Granular Bandwidth Billing - Fresh Produce - Fly.io"))
    

If you change pricing wrong, you either:

- accidentally overcharge → **lose trust**, or
    
- undercharge → **burn money**.
    

Their own engineer described **pricing migrations as “the worst version of data migration”**, and they moved to Metronome rate cards so they can change one central contract instead of a mess of custom deals. ([metronome.com](https://metronome.com/blog/how-fly-io-solves-usage-based-billing-challenges-with-metronome?utm_source=chatgpt.com "How Fly.io Solves Usage-Based Billing Challenges With Metronome"))

So: **changing the model safely** is a huge problem.

---

### C. Bandwidth is the trouble child

Multiple community threads show that **bandwidth billing is consistently confusing**:

- Historical confusion: is egress billed at the edge or where the VM runs? ([Fly.io](https://community.fly.io/t/bandwidth-pricing-is-confusing/12021?utm_source=chatgpt.com "Bandwidth pricing is confusing! - Fly.io"))
    
- New “granular bandwidth billing” introduced per-hop billing between regions, which is more accurate but harder to reason about. ([Fly.io](https://community.fly.io/t/opt-in-to-granular-bandwidth-billing/21099?utm_source=chatgpt.com "Opt-in to Granular Bandwidth Billing - Fresh Produce - Fly.io"))
    
- Private vs public network transfer now have different pricing; private can be free in-region, cheaper cross-region, but legacy orgs and edge cases complicate it. ([Fly.io](https://community.fly.io/t/cheaper-private-outbound-data-transfer/20842?utm_source=chatgpt.com "Cheaper private outbound data transfer - Fresh Produce - Fly.io"))
    

Users regularly report:

- **“Surprisingly high outbound bandwidth charges”** for apps they think are low-traffic. ([Fly.io](https://community.fly.io/t/suprisingly-high-outbound-bandwidth-charges-since-8-jan/17799?utm_source=chatgpt.com "Suprisingly high outbound bandwidth charges since 8 Jan - Fly.io"))
    
- Worry about **“surprise bills”** driven mainly by outbound data, which they can’t fully control. Fly’s team explicitly says bandwidth is the only charge you don’t directly provision a hard upper bound on. ([Fly.io](https://community.fly.io/t/clarification-on-billing/21208?utm_source=chatgpt.com "Clarification on Billing - Questions / Help - Fly.io"))
    

So for Fly.io, bandwidth is:

> hard to meter, hard to explain, and easy to spike accidentally.

---

### D. Free tier & “surprise bill” perception

Because of usage-based + free allowances, you get:

- People staying below _what they believe_ are free limits but still seeing charges or bill previews. ([Fly.io](https://community.fly.io/t/unexplained-charges-despite-free-tier-compliance-bandwidth-region-or-ghost-machines/24779?utm_source=chatgpt.com "Unexplained Charges Despite Free Tier Compliance: Bandwidth, Region, or ..."))
    
- Emails like “Good news, we don’t collect bills under $5, your $0.08 bill is waived” that nonetheless make users go: “Wait, why did I incur anything at all?” ([Fly.io](https://community.fly.io/t/unexpected-billing/14544?utm_source=chatgpt.com "Unexpected billing - Fly.io"))
    

Combined with not-perfectly-real-time dashboards (billing updates multiple times per day) and cross-region traffic, this produces **stress and distrust** for small projects. ([Fly.io](https://community.fly.io/t/unexplained-charges-despite-free-tier-compliance-bandwidth-region-or-ghost-machines/24779?utm_source=chatgpt.com "Unexplained Charges Despite Free Tier Compliance: Bandwidth, Region, or ..."))

So problem:

> Design usage-based pricing that is sustainable **and** feels safe to hobby users who are terrified of surprise bills.

---

### E. Pricing UX & plan semantics

There’s lingering confusion in the UX around:

- legacy **Hobby / Launch / Scale** plans vs newer pay-as-you-go.
    
- wording like “downgrade” vs “convert” when changing plans. ([Fly.io](https://community.fly.io/t/pricing-plans/26130?utm_source=chatgpt.com "Pricing plans - Questions / Help - Fly.io"))
    
- unclear value of upgrading from hobby to higher tiers, according to users who like the platform but can’t justify explaining the upgrade internally. ([Fly.io](https://community.fly.io/t/whats-preventing-you-from-upgrading-your-plan/19292?utm_source=chatgpt.com "What's preventing you from upgrading your plan? - Fly.io"))
    

They’ve added:

- a pricing calculator on the site. ([fly.io](https://fly.io/pricing/?utm_source=chatgpt.com "Pricing · Fly"))
    
- external tools like **flycost.io** exist just to estimate Fly bills from `fly.toml`. ([flycost.io](https://www.flycost.io/?utm_source=chatgpt.com "flycost.io - Cost Estimation for Fly.io"))
    

That means a big issue is **communicating pricing clearly enough that third-party calculators aren’t required**.

---

### F. Operational overhead of billing itself

Fly.io is still a relatively lean company. Maintaining:

- the old “Founder-Built-Billing-System™” for some customers,
    
- plus a newer Metronome-based system,
    
- plus handling bugs in upstream billing partners (noted in their infra logs for granular bandwidth rollout), ([fly.io](https://fly.io/infra-log/2024-08-03/?utm_source=chatgpt.com "2024-08-03 · Infra Log - fly.io"))
    

…is a lot of engineering and ops work **that doesn’t directly ship product features**, but is absolutely mission critical.

So another problem: **how to keep billing infra flexible and correct without it swallowing the entire engineering team**.

---

### Mental model for you

For your learning, you can bucket Fly.io’s usage-based billing challenges into 3 layers:

1. **Metering** – capturing accurate, granular usage signals.
    
2. **Pricing engine** – turning that usage into dollars (rate cards, discounts, migrations).
    
3. **Experience & trust** – invoices, dashboards, calculators, free tier messaging.
    

Most of Fly.io’s real-world issues are some combination of these three.

---

## 2. Turning this into concrete projects (using your framework)

Your framework:

1. **Iteratively take on concrete projects** (learn on demand, depth-first).
    
2. **Teach/summarize in your own words.**
    
3. **Only compare to younger you.**
    

Let’s turn Fly.io’s problems into a project ladder.

---

### Project 1: Build a tiny “Fly.io” billing engine for one org

**Goal:** implement a bare-bones pricing engine that turns “usage events” into an invoice.

**What you’ll build**

- A script or service that:
    
    - Takes JSON/CSV of usage like:
        
        - `org_id, app_id, region, vm_hours, gb_egress, gb_storage`
            
    - Applies a simple rate card, e.g.
        
        - `$0.02 / vCPU hour`
            
        - `$0.02 / GB egress`
            
        - `$0.10 / GB-month storage`
            
    - Produces:
        
        - Total bill
            
        - Breakdown **per app** and **per resource type**.
            

**What you’ll learn (depth):**

- How metering dimensions (org, app, region, resource) blow up invoice complexity.
    
- Why Fly.io needed something more powerful than Stripe’s basic metered billing. ([metronome.com](https://metronome.com/blog/how-fly-io-solves-usage-based-billing-challenges-with-metronome?utm_source=chatgpt.com "How Fly.io Solves Usage-Based Billing Challenges With Metronome"))
    

**Teach / Summarize step:**  
Write a 1–2 page doc in your own words:

- “How a simple usage billing engine works”
    
- “Exactly what I’m charging for and why.”
    

If you do this again in 2 weeks, compare the two docs → this hits your “compare only to younger me” rule.

---

### Project 2: Reverse-engineer & re-explain Fly.io pricing

**Goal:** turn Fly.io’s real pricing into a clear, developer-friendly explanation.

**What you’ll do**

- Read Fly.io’s pricing docs and calculator. ([fly.io](https://fly.io/docs/about/pricing/?utm_source=chatgpt.com "Fly.io Resource Pricing · Fly Docs"))
    
- Read one or two community threads where people are confused about bandwidth and free tier. ([Fly.io](https://community.fly.io/t/the-price-of-bandwidth-is-still-confusing/23399?utm_source=chatgpt.com "The price of bandwidth is still confusing - Questions / Help - Fly.io"))
    
- Then create:
    
    - A **single-page explainer**: “If you do X, Y, Z on Fly.io, this is roughly what you’ll pay, and here’s where you might get surprised.”
        
    - A tiny spreadsheet or script that approximates their calculator logic for a couple of scenarios (e.g. simple app in one region, multi-region app with lots of bandwidth).
        

**What you’ll learn:**

- How many edge cases hide inside “simple” pricing pages.
    
- How confusing bandwidth, proxies, and free tiers feel from the user side.
    

**Teach / Summarize step:**

- Record a 5–10 minute Loom/video (even just for yourself) walking through your explainer as if you’re teaching a junior dev who’s afraid of surprise cloud bills.
    
- Save that video; redo it after later projects and notice how your language changes.
    

---

### Project 3: Cost dashboard for a single customer

**Goal:** make the invoice **legible** – solve Fly.io’s “thousands of line items” problem in miniature.

**What you’ll build**

- Take your Project 1 engine.
    
- Add a small UI (or notebook) that shows:
    
    - Spend by **app**
        
    - Spend by **region**
        
    - Spend by **resource type** (compute, storage, bandwidth)
        
    - A simple daily time-series of “estimated month-to-date spend”.
        

Even a Jupyter notebook with plots is fine; you’re not being graded on frontend aesthetics here.

**What you’ll learn:**

- Why Fly.io cared so much about “easy-to-read itemization of charges per app, region, instance type, bandwidth, etc.” when they moved to Metronome. ([metronome.com](https://metronome.com/blog/how-fly-io-solves-usage-based-billing-challenges-with-metronome?utm_source=chatgpt.com "How Fly.io Solves Usage-Based Billing Challenges With Metronome"))
    
- What makes a breakdown “trustable” vs “just numbers”.
    

**Teach / Summarize step:**

- Write a short README titled:  
    **“How to design a billing dashboard that prevents surprise bills.”**
    

Focus on _principles_ you discovered: e.g. “Always show worst-case cost if everything runs 24/7” or “Bandwidth deserves its own prominent card”.

---

### Project 4: Bandwidth modeling & guardrails

**Goal:** understand why bandwidth is so hard, and design product features to make it less scary.

**What you’ll build**

- Extend your model with:
    
    - regions with different egress prices (e.g., NA $0.02/GB, SA $0.04/GB). ([Fly.io](https://community.fly.io/t/the-price-of-bandwidth-is-still-confusing/23399?utm_source=chatgpt.com "The price of bandwidth is still confusing - Questions / Help - Fly.io"))
        
    - per-hop costs between regions (like Fly.io’s per-hop granular bandwidth). ([Fly.io](https://community.fly.io/t/opt-in-to-granular-bandwidth-billing/21099?utm_source=chatgpt.com "Opt-in to Granular Bandwidth Billing - Fresh Produce - Fly.io"))
        
- Write a simulator that:
    
    - generates traffic patterns (normal, spike from viral link, bug causing infinite retries…)
        
    - calculates resulting bandwidth bills.
        

Then design **guardrails**, like:

- user-configurable soft cap and alerts (e.g. “if projected bandwidth > $20, send alert”)
    
- “what if” calculator (move app from one region to another; see new egress cost).
    

**What you’ll learn:**

- How small architectural decisions (where your app runs; using private networks; cross-region hops) change costs a lot.
    
- Why users in Fly’s forum report “shockingly high” egress with no code changes. ([Fly.io](https://community.fly.io/t/suprisingly-high-outbound-bandwidth-charges-since-8-jan/17799?utm_source=chatgpt.com "Suprisingly high outbound bandwidth charges since 8 Jan - Fly.io"))
    

**Teach / Summarize step:**

- Write a blog post:  
    **“Why bandwidth is the hardest part of usage-based billing (and how to design guardrails).”**
    

Even if you never publish it, writing it for a hypothetical audience forces clarity.

---

### Project 5: Pricing migration & legacy plans

**Goal:** simulate the “worst version of data migration” problem: moving customers from an old plan to a new usage-based one.

**What you’ll do**

- Define:
    
    - a simple **old plan**: “$10/mo includes X compute, Y GB bandwidth, Z GB storage”
        
    - a **new pure usage plan**: separate per-unit prices.
        
- Generate fake customers with random usage patterns.
    
- Write a script that:
    
    - calculates their bill under old vs new plan
        
    - flags:
        
        - users who’d be overcharged by >N% if migrated
            
        - users who would be drastically undercharged.
            

Then design a **migration strategy**:

- grandfathering rules
    
- discounts for a transition period
    
- “opt-in to new billing” flow (like Fly.io did for granular bandwidth and new private-data pricing). ([Fly.io](https://community.fly.io/t/opt-in-to-granular-bandwidth-billing/21099?utm_source=chatgpt.com "Opt-in to Granular Bandwidth Billing - Fresh Produce - Fly.io"))
    

**What you’ll learn:**

- Why Fly.io uses centralized rate cards and templates in Metronome – to reduce the risk of mistakes when migrating. ([metronome.com](https://metronome.com/blog/how-fly-io-solves-usage-based-billing-challenges-with-metronome?utm_source=chatgpt.com "How Fly.io Solves Usage-Based Billing Challenges With Metronome"))
    

**Teach / Summarize step:**

- Write an internal-style memo:  
    **“Our plan for migrating customers from Plan A to Plan B without breaking trust.”**
    

Even as a solo learner, pretend you’re presenting this to a product + finance team.

---

If you like, next step I can help you:

- pick **one** of these projects as “Project #1 – today”,
    
- design a very tiny v0 scope and schema,
    
- and give you a checklist for how to implement and then summarize it in your own words.