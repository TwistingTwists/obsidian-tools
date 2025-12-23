
Is **Scatter-Gather** (querying all partitions) optimal? **For pure read latency? No.** It is terrible. If you have 100 partitions, your search speed is tied to the *slowest* of those 100 servers (Tail Latency).

However, companies like Flipkart, Amazon, and Uber often choose this "sub-optimal" read strategy because the alternative (Global Indexes) is disastrous for **Writes** and **Complexity**.

Here is a deep dive into why this happens, specifically focusing on **Big Billion Day (High Scale)** and **Dynamic Attributes (Schema Flexibility)**.

---

### 1. The "Big Billion Day" Problem: Write Throughput vs. Read Speed

During a sale, two things happen simultaneously:
1.  **Reads:** Millions of users searching for "iPhone 15".
2.  **Writes:** Inventory counts change every millisecond. Prices fluctuate. Sellers add new deals.

#### If we used Global Indexes (Term-Partitioned)
Imagine we have a specific Global Index for `Price`. All products with `Price: 1000` are stored on Node A.
*   **The Bottleneck:** If a seller updates the price of a popular shoe, the database has to perform a distributed transaction: Update the product on Node B *and* update the index on Node A.
*   **The Crash:** During a flash sale, thousands of items change status (In Stock -> Out of Stock) instantly. This would hammer the specific nodes holding the indexes for "Stock" or "Price," creating massive **Write Hotspots**.

#### Why Local Indexes (Document-Partitioned) win here
With Local Indexes, each partition is an independent island.
*   **The Isolation:** When a user buys the last "Red Nike Shoe" on Partition 5, the system only needs to update Partition 5's index. It does not care about Partition 1, 2, or 3.
*   **The Benefit:** This allows **linear scaling of writes**. If you need to handle more orders/updates, you just add more partitions.
*   **The Cost:** You pay the penalty on the *Read* (Search) side by doing Scatter-Gather. But e-commerce companies solve this via **Caching**. The search result for "iPhone" is cached; the inventory update is the hard part that must be accurate.

---

### 2. The Nuance of Dynamic Attributes (The "Bag of Words" Problem)

You mentioned: *"Products do not have uniform product categories (color, size, brand vs RAM, Screen Type)."*

This is a classic **polymorphism** problem in databases.
*   **Shoes** have: Size (6-12), Material (Leather).
*   **Laptops** have: RAM (16GB), Processor (M1).

#### If we used Global Indexes
You would need a separate Global Index for *every single attribute*.
*   Node A holds the index for `Size`.
*   Node B holds the index for `RAM`.
*   Node C holds the index for `Material`.

If a seller adds a new attribute (e.g., "Water Resistance" for a watch), you might have to provision a new index or rebalance the existing Global Index nodes. This is operationally very heavy.

#### Why Local Indexes win here
In a Document-Partitioned (Local Index) system (like Elasticsearch or MongoDB):
*   Partition 1 holds 1,000 mixed items (shoes, laptops, fridges).
*   It builds a local "dictionary" of whatever attributes exist *inside that bucket*.
*   If Partition 1 has no Laptops, it simply doesn't have a `RAM` index locally.
*   **The Intution:** This allows the schema to be **messy**. You can dump any JSON document into a partition, and the partition figures out how to index it locally. You don't need a central authority managing the schema of millions of attributes.

---

### 3. How Real World Systems "Cheat" the Scatter-Gather Penalty

So, if Scatter-Gather is slow, how does Amazon return results so fast? They use specific optimizations mentioned in DDIA to mitigate the "Query All" problem.

#### A. Routing (Partition Pruning)
They rarely query *ALL* partitions if they can avoid it.
*   **Use Case:** Viewing your generic "Past Orders".
*   **Strategy:** If the system knows your `UserID` maps to Partition 7, it sends the query *only* to Partition 7. No scatter-gather needed.
*   **Use Case:** Searching within a specific category.
*   If you select "Books" from the dropdown *before* searching "Harry Potter", the system might know that all Books reside on Partitions 10-20. It only scatters the query to those 10 nodes, ignoring the nodes that hold Electronics.

#### B. The "Good Enough" Approach (Tail Latency Cutting)
*   **Scenario:** You search for "White Sneakers."
*   **The Reality:** The system sends the query to 50 partitions.
*   **The Trick:** 49 partitions reply in 50ms. Partition #50 is having a hiccup and takes 1 second.
*   **The Optimization:** The aggregator **does not wait**. It takes the results from the 49 fastest partitions and shows them to you.
*   **Intuition:** You (the user) would rather see 98% of the available sneakers *instantly* than wait 1 second to see 100% of them. This effectively eliminates the tail latency problem of scatter-gather.

#### C. Replicated Search Indexes
*   **Scenario:** High Read Traffic (Big Billion Day).
*   **Strategy:** They don't just have Partition 1. They have Partition 1-Replica-A, Partition 1-Replica-B, etc.
*   The Scatter-Gather load balancer can pick the least loaded replica for each partition. This ensures that one busy node doesn't slow down the entire search.

### Summary Comparison

| Requirement | Strategy | Verdict for E-Commerce |
| :--- | :--- | :--- |
| **Write Speed** | Global Index | **Fail** (Too much contention on attribute indexes) |
| **Write Speed** | Local Index | **Pass** (Updates are contained to one node) |
| **Schema Flexibility** | Global Index | **Fail** (Hard to manage indexes for 1000s of unique attributes like 'RAM', 'Fabric') |
| **Schema Flexibility** | Local Index | **Pass** (Each partition manages its own messy attributes) |
| **Search Latency** | Global Index | **Pass** (Direct lookup, very fast) |
| **Search Latency** | Local Index | **Fail/Mediocre** (Scatter-gather is slow, but fixed via Caching/Timeouts) |

**Conclusion:** Flipkart and Amazon use **Local Indexes (Scatter-Gather)** because in their world, **Write Scalability** (Inventory/Price updates) and **Schema Evolution** (New products/categories) are harder problems to solve than Search Latency. They sacrifice raw search efficiency for operational stability.