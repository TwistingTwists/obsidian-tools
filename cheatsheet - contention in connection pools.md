basic contention: min_idle + max_connections MUST be respected at all times. 
important: never shoot beyond max_connections. NEVER. 

👉 **Net effect:** fast when idle, stable under bursts, safe under concurrency.




# short summary of Approval system in the connection pools 

## Capacity Reservation & `ApprovalIter` — Recall Sheet

## 1️⃣ The core problem (first principles)

> **I need to do slow, async, fallible work without ever violating a hard capacity limit.**

In this pool:

- Capacity = `num_conns + pending_conns ≤ max_size`
    
- Work = creating connections (async, retrying, may fail)
    
- Many triggers:
    
    - `get()` demand
        
    - min-idle warmup
        
    - broken/expired connections
        
    - reaper
        

**Key difficulty:**  
You must decide _how many_ to start **under a lock**, but _execute_ the work **outside the lock**.

---

## 2️⃣ The core idea: Capacity Reservation Interface

> **Reserve capacity first. Commit or roll back later.**

### Two phases (2PC-like, single process):

1. **Reserve (prepare)**
    
    - Under lock
        
    - Increment `pending_conns`
        
    - Mint a token (`Approval`)
        
2. **Resolve (commit / rollback)**
    
    - Outside lock
        
    - Success → convert pending → real connection
        
    - Failure → release reservation
        

💡 This is _not_ distributed 2PC:

- single authority
    
- no durability
    
- no blocking participants
    
- just invariant protection
    

---

## 3️⃣ What an `Approval` is (precise meaning)

> **An `Approval` = “you are allowed to attempt exactly ONE new connection.”**

- It represents _reserved pool capacity_
    
- It must be:
    
    - **consumed** on success (`put(conn, Some(approval))`)
        
    - **returned** on failure (`connect_failed(approval)`)
        

Think of it as a **connect-job ticket**, not a usage permit.

---

## 4️⃣ Why `ApprovalIter` exists

### What it does

- Hands out **N approvals** decided under the lock
    
- No allocation
    
- Cheap batch admission
    

```rust
let approvals = locked.approvals(config, n);
for approval in approvals {
    spawn_connect_task(approval);
}
```

### Why not `usize`

- Counts are easy to misuse
    
- Tokens force correct success/failure reconciliation
    
- Types encode correctness
    

### Why not `Vec<Approval>`

- Same semantics
    
- Unnecessary allocation
    
- Iterator is strictly better
    

---

## 5️⃣ Why not just a semaphore?

### Semaphores are great when:

- Capacity = the only constraint
    
- You want callers to wait for permits
    
- One permit = one unit of work
    

### This pool needs more:

- `min_idle` replenishment
    
- Separate **pending vs existing** connections
    
- Demand-based scaling (`in_flight > pending`)
    
- Batch decisions under the same lock as idle queue
    

Semaphores cap _usage_.  
Approvals reserve _creation capacity_.

---

## 6️⃣ Two replenishment modes (important!)

### 1. **Bulk (baseline health)**

```rust
wanted() → ApprovalIter
```

- Ensures `min_idle`
    
- Issues many approvals at once
    
- Used on:
    
    - startup
        
    - reaping
        
    - dropped/broken connections
        

### 2. **Trickle (demand-driven)**

```rust
if in_flight > pending_conns {
    approvals = 1;
}
```

- Avoids thundering herds
    
- Reacts to contention
    
- Used in `get()`
    

💡 This split is a _key design insight_.

---

## 7️⃣ Approval lifecycle (memorize this flow)

```
[approved] → pending_conns += 1
     ↓
(connect async, retries)
     ↓
SUCCESS → pending--, num_conns++, push idle
FAILURE → pending--
```

Everything funnels through this path.

---

## 8️⃣ Where this pattern appears in real systems

Use this pattern when:

- Capacity must **never** be exceeded
    
- Work is **slow / async / fallible**
    
- Decisions must be **atomic**, execution cannot be
    

### Common examples

- DB / Redis / HTTP connection pools
    
- Worker schedulers
    
- Bounded producer pipelines
    
- Memory / buffer pools
    
- Rate limiters with rollback
    
- Cluster schedulers (K8s-style reservations)
    

---

## 9️⃣ Mental shortcuts (for recall)

- **Approval ≈ permit for creation**
    
- **ApprovalIter ≈ batch permits**
    
- **pending_conns = prepared**
    
- **put(Some(approval)) = commit**
    
- **connect_failed = rollback**
    
- **Semaphore caps usage**
    
- **Approval caps growth**
    

---

## 🔟 One-line takeaway

> **`ApprovalIter` is a cheap, explicit way to reserve pool capacity upfront, so async connection creation can happen safely without ever violating invariants.**

If you remember _only one thing_, remember that.

---



# Avoiding thundering herd in the connectoin pools 

[[thundering-herd python script]]

## What “`in_flight > pending`” actually means

- **`in_flight`**  
    = number of _active `get()` attempts_ (current demand)
    
- **`pending`**  
    = number of _connections already being created_ (supply in progress)
    

So:

> `in_flight > pending`  
> ⇒ _demand is growing faster than supply_

That is the **signal**.

---

## Why add **only 1** connection

When that signal is true, the pool does this:

```rust
approvals = 1;
```

Not N. Not “fill to max”.

**Reason:** connection creation is slow, expensive, and failure-prone.

Adding only 1 means:

- demand increase → _small_ supply increase
    
- repeat signals → gradual ramp-up
    
- no sudden connection storms
    

---

## How this prevents a thundering herd

### Without trickle (bad case)

- 50 tasks call `get()` simultaneously
    
- All see “no idle connections”
    
- All decide “we should create connections”
    
- 50 connection attempts start at once
    
- DB / service collapses
    

### With trickle

- First `get()` sees `in_flight=1, pending=0` → start 1 connect
    
- Second sees `in_flight=2, pending=1` → start 1 more
    
- Third sees `in_flight=3, pending=2` → start 1 more
    
- …
    
- Growth is **linear and controlled**
    

Each extra connection is justified by _new observed contention_.

---

## Why this still converges fast enough

Because:

- Each `get()` loop iteration re-checks
    
- `in_flight` grows quickly under load
    
- Approvals are cheap to mint
    
- Multiple connect tasks run in parallel (up to `max_size`)
    

So the pool still ramps up — just **smoothly**, not explosively.

---

## One-sentence intuition

> **Trickle scaling turns sudden demand spikes into controlled, step-by-step supply increases, instead of speculative over-creation.**

If you want, I can show a tiny timeline diagram (numbers over time) to make this visually obvious.