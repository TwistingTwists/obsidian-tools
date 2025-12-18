For your specific use case—booking a ticket by changing a row's status—PostgreSQL Advisory Locks are likely **not** the right tool. Your "state machine" approach (Row-Level Locking) is the standard, most robust way to handle that problem.

booking a seat that is represented by a row in a database—**a single atomic UPDATE statement is the "Gold Standard" pattern.**

You do not need an explicit BEGIN/COMMIT block if you can do it in one statement, and you definitely do not need Advisory Locks.

### 1. The "Single SQL" Approach (Compare-and-Swap)

Your proposed approach is known in the industry as **Conditional Update** or **Compare-and-Set (CAS)**.

```sql
-- Attempt to reserve the seat in one atomic command
UPDATE seats 
SET status = 'HELD', 
    user_id = 101, 
    hold_expires_at = NOW() + INTERVAL '10 minutes'
WHERE id = 5432 
AND status = 'AVAILABLE'; -- This condition is the key!
```


**The Advisory Lock Approach:**
```sql
-- You have to invent an arbitrary ID for "Seat 123"
SELECT pg_advisory_lock(123); 
-- Now do your update...
-- If your code crashes here before unlocking, the lock might persist (session-level).
SELECT pg_advisory_unlock(123);
```
*   **The Downside:** You have decoupled the "lock" from the "data." If you acquire the lock but fail to update the row, or if you update the row but fail to release the lock, your system enters an inconsistent state.

### 2. So, what are Advisory Locks actually for?
Advisory locks are "User-Space" locks. They are used when **you need to lock a concept, not a row.**

Real-world systems (like those at Netflix, Uber, or Amazon) use them for **distributed coordination**, not usually for core transaction processing.

#### Common Use Cases:
1.  **Distributed Cron Jobs (The "Singleton" Problem)**
    *   *Scenario:* You have 20 servers. Every hour, you want to run a "Cleanup Job," but you only want **one** server to run it.
    *   *Problem:* You don't have a "Cleanup Job" row in a database to lock.
    *   *Solution:* `SELECT pg_try_advisory_lock(99999);`
    *   The first server gets `true` and runs the job. The other 19 get `false` and skip it. No heavy database writes required.

2.  **Database Migrations (Preventing Parallel Schema Changes)**
    *   *Real World Example:* **GitLab**, **Prisma**, and **Rails** use advisory locks automatically when you run migrations.
    *   When you run `rails db:migrate` or `prisma migrate`, the tool grabs a specific advisory lock ID. If another developer tries to run a migration at the same time, it blocks them so you don't corrupt the schema.

3.  **"Thundering Herd" Caching**
    *   *Scenario:* A heavy report needs to be calculated. 50 users request it at once.
    *   *Solution:* Use an advisory lock on the "Report ID." The first request gets the lock and calculates the data. The other 49 wait on the lock. When the first releases it, the others read the now-cached result from the DB instead of re-calculating it.

4.  **External Resource Locking**
    *   *Scenario:* You need to upload a file to a specific S3 path or call a non-idempotent API endpoint, and you want to ensure no other thread is doing that exact operation. Since this operation happens *outside* the database, a row lock (which releases on commit) might release too early (before the API call finishes). A session-level advisory lock can be held until you explicitly release it.

### 3. Real World Usage
While big tech companies (Netflix, Amazon, Uber) rely heavily on internal distributed locking services (like Google's **Chubby**, Apache **Zookeeper**, or **Redis** Redlock), they use PostgreSQL Advisory Locks in specific niches:

*   **Subskribe (Quote-to-Revenue Platform):** They publicly documented using PG Advisory Locks for "Distributed Mutual Execution." They use it to ensure invoice generation happens strictly sequentially for a customer, even if multiple events trigger it simultaneously. They chose this over Redis to avoid adding a new infrastructure dependency.
*   **Sidekiq (Ruby Background Jobs):** Some plugins for Sidekiq use advisory locks to ensure unique jobs (preventing the same job from being queued twice).
*   **Terraform (State Locking):** When using the Postgres backend for Terraform state, it uses locking to ensure two developers don't apply infrastructure changes effectively wiping each other's work out.

### Summary
*   **For your Ticket Booking:** Stick to `SELECT ... FOR UPDATE`. It is durable, atomic, and standard.
*   **Use Advisory Locks When:** You need to coordinate processes (like cron jobs, migrations, or cache rebuilds) where there is no actual database row to lock, or you want to avoid the write-overhead of updating a "job status" table.