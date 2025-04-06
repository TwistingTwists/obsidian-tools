---
tags:
  - system_design
  - interview
---
Here’s a curated list of 15 questions (with follow-ups) inspired by *Designing Data-Intensive Applications* (DDIA) and distributed systems principles. These test depth, practical application, and synthesis of concepts:

---

### **1. Data Models**  
**Q:** How do relational and document data models differ in handling schema evolution?  
**Follow-up 1:** When would you choose a graph database over a document store for a social network?   
**Follow-up 2:** How does schema flexibility in NoSQL systems impact consistency guarantees?  

---

### **2. Storage Engines**  
**Q:** Compare LSM-trees and B-trees for write-heavy workloads.  
**Follow-up 1:** Why do LSM-trees suffer from write amplification, and how is this mitigated?  
**Follow-up 2:** How does indexing strategy affect storage engine performance in time-series data?  

---

### **3. Distributed Data**  
**Q:** Explain leader-based replication vs. multi-leader replication. What failure modes do they handle differently?   
**Follow-up 1:** How does network partitioning affect leader election in consensus protocols like Raft?  
**Follow-up 2:** What trade-offs arise when using quorums (e.g., *W + R > N*) for reads/writes?   

---

### **4. Consensus Algorithms**  
**Q:** How does Raft simplify Paxos while achieving similar safety properties?  
**Follow-up 1:** Why is leader lease required in Raft to prevent stale reads?  
**Follow-up 2:** How does Byzantine fault tolerance differ from crash fault tolerance in consensus?  

---

### **5. Replication Strategies**  
**Q:** What are the trade-offs between synchronous and asynchronous replication for cross-region databases?  
**Follow-up 1:** How does asynchronous replication lead to stale reads in a multi-leader setup?   
**Follow-up 2:** When would you use logical clocks instead of vector clocks for conflict resolution?  

---

### **6. Partitioning**  
**Q:** Compare dynamic partitioning (e.g., consistent hashing) vs. static partitioning in key-value stores.  
**Follow-up 1:** How does rebalancing work in consistent hashing, and what latency impacts does it introduce?  
**Follow-up 2:** Why are range-based partitions prone to hotspots, and how can this be mitigated?  

---

### **7. Transactions**  
**Q:** What guarantees do ACID and BASE provide, and in which scenarios are they appropriate?  
**Follow-up 1:** How does snapshot isolation differ from serializable isolation in distributed databases?  
**Follow-up 2:** Why are distributed transactions (e.g., 2PC) often avoided in high-throughput systems?   

---

### **8. System Design**  
**Q:** How would you design a distributed lock service for a globally distributed system?  
**Follow-up 1:** What role does fencing play in preventing split-brain scenarios?   
**Follow-up 2:** How does a service mesh simplify failure handling in microservices?  

---

### **9. Consistency & CAP**  
**Q:** Explain the CAP theorem and its limitations in real-world systems.  
**Follow-up 1:** How does eventual consistency differ from causal consistency?   
**Follow-up 2:** Why do modern systems often prioritize availability over strict consistency?   

---

### **10. Conflict-Free Replicated Data Types (CRDTs)**  
**Q:** How do CRDTs achieve convergence without coordination?  
**Follow-up 1:** Provide an example of a CRDT and its limitations.  
**Follow-up 2:** How do operational transforms (OT) in collaborative editors differ from CRDTs?  

---

### **11. Event Sourcing & CQRS**  
**Q:** What are the benefits and challenges of event sourcing for stateful services?  
**Follow-up 1:** How does CQRS address scalability in read-heavy systems?   
**Follow-up 2:** Why is idempotency critical when replaying events?  

---

### **12. Security in Distributed Systems**  
**Q:** How do you secure a distributed system against Byzantine failures?  
**Follow-up 1:** What role does TLS play in securing inter-service communication?   
**Follow-up 2:** How do zero-trust architectures enforce least privilege in microservices?  

---

### **13. Observability**  
**Q:** What metrics are critical for detecting cascading failures in distributed systems?  
**Follow-up 1:** How does distributed tracing help debug latency in microservices?  
**Follow-up 2:** Why are SLOs (Service Level Objectives) foundational to observability?  

---

### **14. Stream Processing**  
**Q:** Compare stateless and stateful stream processing (e.g., Kafka vs. Flink).  
**Follow-up 1:** How do windowing strategies affect event-time processing?  
**Follow-up 2:** Why is exactly-once processing difficult in distributed streams?  

---

### **15. Real-World Trade-offs**  
**Q:** How would you design a globally consistent distributed database with low latency?  
**Follow-up 1:** What compromises would you make for availability during network partitions?   
**Follow-up 2:** How does eventual consistency impact user-facing applications?   

--- 



Here’s a structured list of **15 questions** (with follow-ups) covering key topics from *Designing Data-Intensive Applications* and testing deep understanding of distributed systems:

---

### **1. Data Models**  
**Q:** Compare the relational and document data models. What are their trade-offs for modeling hierarchical data?  
- **Follow-up 1:** How does schema flexibility in document databases impact long-term application maintenance?  
- **Follow-up 2:** Why are many-to-many relationships harder to represent in document models, and how do relational databases handle them better?  

---

### **2. Storage Engines**  
**Q:** Contrast LSM-trees and B-trees in terms of write/read performance and storage efficiency.  
- **Follow-up 1:** Why are LSM-trees better for write-heavy workloads, and what are the downsides of compaction?  
- **Follow-up 2:** How do B-trees handle on-disk fragmentation, and why might they struggle with high write throughput?  

---

### **3. Encoding & Evolution**  
**Q:** How does Avro handle schema evolution compared to Protobuf/Thrift?  
- **Follow-up 1:** Why does Avro require a schema registry, and how does it enable backward/forward compatibility?  
- **Follow-up 2:** What are the trade-offs between dynamic typing (Avro) and static code generation (Protobuf)?  

---

### **4. Replication**  
**Q:** Compare leader-based and multi-leader replication. When would you choose one over the other?  
- **Follow-up 1:** How do conflict resolution strategies differ in multi-leader setups (e.g., last-write-wins vs CRDTs)?  
- **Follow-up 2:** What are the challenges of read-after-write consistency in leaderless replication (e.g., Dynamo-style)?  

---

### **5. Partitioning**  
**Q:** Explain strategies for partitioning data (key-range, hash-based) and rebalancing partitions.  
- **Follow-up 1:** How does consistent hashing minimize data movement during rebalancing?  
- **Follow-up 2:** How would you handle "hot keys" skewing load in a partitioned system?  

---

### **6. Transactions**  
**Q:** What do ACID guarantees provide, and how do isolation levels (e.g., read committed, serializable) differ?  
- **Follow-up 1:** Why is serializability hard to achieve in distributed systems, and what are alternatives (e.g., SSI)?  
- **Follow-up 2:** How do optimistic vs. pessimistic concurrency control trade off performance and correctness?  

---

### **7. Distributed Challenges**  
**Q:** Why are partial failures and unbounded network delays fundamental challenges in distributed systems?  
- **Follow-up 1:** How do systems detect node failures (e.g., heartbeats vs. gossip protocols)?  
- **Follow-up 2:** Why can’t you rely solely on synchronized clocks for ordering events?  

---

### **8. Consistency**  
**Q:** Compare linearizability and eventual consistency. When is each appropriate?  
- **Follow-up 1:** How do quorum reads/writes enforce strong consistency in Dynamo-style systems?  
- **Follow-up 2:** Can you achieve linearizability without a consensus algorithm (e.g., using 2PC)?  

---

### **9. Consensus Algorithms**  
**Q:** How does Raft simplify consensus compared to Paxos?  
- **Follow-up 1:** How does Raft handle leader election and log replication during network partitions?  
- **Follow-up 2:** What are the challenges of dynamic membership changes in consensus protocols?  

---

### **10. Batch Processing**  
**Q:** Explain the MapReduce paradigm. How do modern frameworks (e.g., Spark) improve on it?  
- **Follow-up 1:** How does batch processing achieve fault tolerance through immutable inputs and retries?  
- **Follow-up 2:** Why is incremental processing (e.g., microbatching) a hybrid approach between batch and streaming?  

---

### **11. Stream Processing**  
**Q:** What distinguishes event streams (e.g., Kafka) from traditional messaging systems?  
- **Follow-up 1:** How do you handle event time vs. processing time in stream joins/windowing?  
- **Follow-up 2:** What techniques enable exactly-once processing in streams (e.g., idempotence, transactional commits)?  

---

### **12. Fault Tolerance**  
**Q:** How do replication and redundancy mitigate faults in distributed systems?  
- **Follow-up 1:** What are the trade-offs between synchronous and asynchronous replication?  
- **Follow-up 2:** How do systems handle correlated failures (e.g., zone outages)?  

---

### **13. Derived Data**  
**Q:** Compare ETL pipelines and stream processors for maintaining derived data systems.  
- **Follow-up 1:** How do you ensure eventual consistency between primary and derived stores?  
- **Follow-up 2:** What challenges arise from late-arriving data in stream processing?  

---

### **14. Time & Ordering**  
**Q:** Why are clocks and time critical in distributed systems, and how can they fail?  
- **Follow-up 1:** How do logical clocks (e.g., Lamport clocks) provide ordering without physical time?  
- **Follow-up 2:** What is clock skew, and how does it impact systems like Spanner’s TrueTime?  

---

### **15. CAP Theorem**  
**Q:** Explain the CAP theorem’s trade-offs. Can a system be both CA during normal operation?  
- **Follow-up 1:** In practice, how do systems like Cassandra (AP) and etcd (CP) align with CAP?  
- **Follow-up 2:** Why is the “CA” combination only possible in a failure-free network?  

---

Below is a curated list of 15 challenging questions inspired by the topics in _Designing Data-Intensive Applications_ by Martin Kleppmann, along with two follow-up questions for each. These questions are designed to test and deepen your understanding of distributed systems as covered in the book.

1. **Data Models and Query Languages:**
    
    - What are the trade-offs between using a relational versus a document data model in a distributed system?
        
        - How do schema evolution and query optimization challenges differ between these models?
            
        - In what scenarios might one model be preferred over the other in a high-scale environment?
            
2. **Storage and Retrieval:**
    
    - How do storage engines differ in terms of their architecture and performance for write-heavy versus read-heavy workloads?
        
        - What are the key design considerations when choosing between log-structured merge-trees and B-trees?
            
        - How do these differences impact latency and throughput in distributed environments?
            
3. **Encoding and Evolution:**
    
    - What challenges arise from changing data schemas in a live distributed system?
        
        - How can techniques like backward and forward compatibility help mitigate these challenges?
            
        - What role do data serialization formats (e.g., Avro, Protocol Buffers) play in this process?
            
4. **Replication:**
    
    - What are the main strategies for replication in distributed systems, and how do they affect consistency and availability?
        
        - How does leader-based replication differ from leaderless replication in terms of failure handling?
            
        - What impact do network partitions have on replication strategies?
            
5. **Partitioning (Sharding):**
    
    - How does partitioning improve scalability in distributed systems, and what are its potential pitfalls?
        
        - What are the differences between range-based and hash-based partitioning?
            
        - How can rebalancing or repartitioning be performed with minimal disruption?
            
6. **Consistency Models:**
    
    - How do various consistency models (strong, eventual, causal) influence system design in distributed environments?
        
        - What are the trade-offs between consistency and latency in global-scale systems?
            
        - How can one decide which consistency model is appropriate for a given application?
            
7. **Distributed Transactions:**
    
    - What mechanisms are used to ensure atomicity and consistency in distributed transactions?
        
        - How does the two-phase commit protocol work, and what are its limitations?
            
        - What alternatives exist for managing distributed transactions in modern systems?
            
8. **Fault Tolerance and Resilience:**
    
    - How does designing for fault tolerance influence the architecture of a distributed system?
        
        - What are the key strategies to ensure system resilience in the face of node failures?
            
        - How do concepts like redundancy and graceful degradation fit into this design?
            
9. **Leader Election and Coordination:**
    
    - Why is leader election critical in distributed systems, and what are some common algorithms used for it?
        
        - How does the Raft consensus algorithm simplify the implementation of leader election?
            
        - What are the challenges of maintaining consistency during leadership changes?
            
10. **Consensus Algorithms:**
    
    - How do consensus algorithms (e.g., Paxos, Raft) enable reliability in distributed systems?
        
        - What are the major differences between Paxos and Raft in terms of understandability and implementation complexity?
            
        - In what scenarios might one algorithm be favored over the other?
            
11. **Batch Processing vs. Stream Processing:**
    
    - What are the primary differences between batch processing and stream processing paradigms?
        
        - How does the choice between these paradigms affect data latency and system throughput?
            
        - What design considerations should be taken into account when building a hybrid system?
            
12. **Event Sourcing and CQRS:**
    
    - How do event sourcing and CQRS architectures help manage complex state in distributed systems?
        
        - What are the challenges associated with maintaining an event log over time?
            
        - How does the separation of read and write models impact system performance?
            
13. **System Observability:**
    
    - What strategies are critical for achieving high observability in a distributed system?
        
        - How can metrics, logs, and tracing be integrated to monitor system health effectively?
            
        - What role does observability play in diagnosing performance bottlenecks?
            
14. **Security in Distributed Systems:**
    
    - What are the main security challenges when designing a distributed system, particularly in data-intensive applications?
        
        - How do encryption and authentication mechanisms contribute to securing inter-node communications?
            
        - What best practices should be followed to ensure data integrity across distributed components?
            
15. **Scaling and Performance Optimization:**
    
    - What architectural considerations are crucial when designing a system to scale horizontally?
        
        - How do caching strategies and load balancing contribute to overall system performance?
            
        - What are common pitfalls when scaling distributed systems, and how can they be mitigated?
            
As a former Google Distinguished Fellow and expert in distributed systems, I’ve prepared a list of 15 questions inspired by *Designing Data-Intensive Applications* by Martin Kleppmann. These questions are divided into topic-specific areas from the book and broader questions testing your overall understanding of distributed systems. Each question comes with two follow-up questions to dive deeper into your knowledge. Let’s see how you fare!

---

### Topic-Wise Questions from *Designing Data-Intensive Applications*

#### **1. Reliability in Distributed Systems**
- **Question:** What are the key characteristics that make a system reliable?  
  - *Follow-up 1:* How do you ensure fault tolerance in a distributed system?  
  - *Follow-up 2:* What is the difference between fault tolerance and high availability?

#### **2. Scalability**
- **Question:** How do you define scalability in the context of distributed systems?  
  - *Follow-up 1:* What are some common strategies for scaling a system horizontally?  
  - *Follow-up 2:* How does vertical scaling differ from horizontal scaling?

#### **3. Maintainability**
- **Question:** What practices can be adopted to make a distributed system more maintainable?  
  - *Follow-up 1:* How does modularity contribute to maintainability?  
  - *Follow-up 2:* What role does documentation play in this context?

#### **4. Data Models**
- **Question:** What are the differences between relational and non-relational databases?  
  - *Follow-up 1:* When would you choose a document-oriented database over a relational one?  
  - *Follow-up 2:* What are the implications for query performance?

#### **5. Query Languages**
- **Question:** How does SQL differ from other query languages like MongoDB's query language?  
  - *Follow-up 1:* What are the advantages of using a declarative query language?  
  - *Follow-up 2:* How do imperative query languages compare in terms of performance?

#### **6. Replication**
- **Question:** What are the different replication strategies in distributed systems?  
  - *Follow-up 1:* How does leader-based replication work?  
  - *Follow-up 2:* What are the trade-offs between synchronous and asynchronous replication?

#### **7. Partitioning**
- **Question:** What is data partitioning, and why is it necessary in distributed systems?  
  - *Follow-up 1:* How do you handle hot spots in a partitioned database?  
  - *Follow-up 2:* What is the impact of partitioning on query performance?

#### **8. Transactions**
- **Question:** What is a distributed transaction, and what challenges does it present?  
  - *Follow-up 1:* How does the two-phase commit protocol work?  
  - *Follow-up 2:* What are the alternatives to two-phase commit?

#### **9. Consistency Models**
- **Question:** What is eventual consistency, and how does it differ from strong consistency?  
  - *Follow-up 1:* In what scenarios would you prefer eventual consistency over strong consistency?  
  - *Follow-up 2:* How do you handle conflicts in an eventually consistent system?

#### **10. Batch and Stream Processing**
- **Question:** What is the difference between batch processing and stream processing?  
  - *Follow-up 1:* How does Apache Kafka facilitate stream processing?  
  - *Follow-up 2:* What are the challenges in ensuring exactly-once semantics in stream processing?

---

### Questions Testing Overall Understanding of Distributed Systems

#### **11. Handling Failures**
- **Question:** How do you design a distributed system to handle failures gracefully?  
  - *Follow-up 1:* What is the CAP theorem, and how does it influence system design?  
  - *Follow-up 2:* Can you give an example of a system that prioritizes availability over consistency?

#### **12. Consensus Algorithms**
- **Question:** What are the key considerations when choosing a consensus algorithm for a distributed system?  
  - *Follow-up 1:* How does Raft compare to Paxos in terms of understandability and implementation?  
  - *Follow-up 2:* What are the use cases for each?

#### **13. Data Integrity**
- **Question:** How do you ensure data integrity in a distributed system?  
  - *Follow-up 1:* What is the role of checksums and hashes in maintaining data integrity?  
  - *Follow-up 2:* How do you handle data corruption in a replicated system?

#### **14. Security in Distributed Systems**
- **Question:** What are the security challenges in distributed systems?  
  - *Follow-up 1:* How do you implement authentication and authorization in a distributed environment?  
  - *Follow-up 2:* What measures can be taken to prevent denial-of-service attacks?

#### **15. Monitoring and Debugging**
- **Question:** How do you monitor and debug a distributed system?  
  - *Follow-up 1:* What tools and techniques are commonly used for distributed tracing?  
  - *Follow-up 2:* How do you handle performance bottlenecks in a distributed system?

---

These 15 questions span the core concepts from *Designing Data-Intensive Applications*—reliability, scalability, data models, replication, partitioning, transactions, consistency, and processing paradigms—while also challenging your broader grasp of distributed systems design and operation. The follow-ups will test your ability to apply these concepts practically. Ready to tackle them? Let me know how you’d like to proceed!



----- 
You're demonstrating a good understanding of the ACID properties.

You've correctly pointed out that Atomicity refers to the "all-or-nothing" property, and that it's unrelated to concurrency. I've seen some people confuse it with concurrency control, so it's great that you're clear on this.

You've also mentioned that Consistency can be interpreted in different ways, and you've listed some possible meanings, including strong consistency, linearizability, and serializability. This is a great point, as the term "consistency" can be overloaded and context-dependent. In the context of ACID, Consistency typically refers to the idea that a transaction must preserve the integrity constraints of the database, such as foreign key constraints or check constraints.

You've also correctly described Durability as the guarantee that once a transaction has committed, its effects are persisted even in the face of failures, such as disk crashes or network partitions.

Now, let's dive a bit deeper into Isolation. Can you explain what you understand by Isolation in the context of ACID, and how it relates to concurrency control? How would you implement Isolation in a database, and what are some common techniques used to achieve it?

Also, I'd like to explore a specific aspect of transactions: what are some common concurrency control mechanisms that databases use to implement Isolation, and how do they trade off between consistency, availability, and performance?


You're on the right track with Isolation!

You've correctly pointed out that Isolation is about how concurrent transactions interact with each other, and that it's related to concurrency control. You're also right that when two transactions try to access the same data, the effects of each transaction should be isolated from the other.

You've also mentioned two common approaches to concurrency control: Optimistic Concurrency Control (OCC) and Pessimistic Concurrency Control (PCC). OCC is a technique where transactions are allowed to proceed without acquiring locks, and instead, a compare-and-swap (CAS) or similar mechanism is used to detect conflicts. If a conflict is detected, the transaction is aborted and retried.

That's a good start! To build upon that, can you elaborate on how Pessimistic Concurrency Control works? How does it differ from OCC, and what are some common techniques used to implement PCC, such as locking?

Also, when discussing OCC, you mentioned "compare and swap fails." Can you explain what you mean by this, and how it's typically implemented in a database? Are there any specific challenges or trade-offs associated with using OCC versus PCC?

Let's also consider a specific scenario: suppose we have two transactions, T1 and T2, that are trying to update the same row in a database table. T1 reads the row, performs some computation, and then tries to update the row. Meanwhile, T2 also reads the row, performs some computation, and tries to update the row. How would you implement Isolation in this scenario using either OCC or PCC?


For pessimistic concurrency control or lock taken on the row which is then used to let other transactions wait. Yes other trade off between optimusic con currency control (OCC) and PCC are 
1. Optimistic con currency control  ideal for low conflict high latency scenarios. Where tries are okay 
2. For high throughput low latency situations it is preferred to do PCC 


For the given scenario , PCC will lock the row while reading which means either transaction 2 or T1 will update row  sequentiallly. 

--- 

You're demonstrating a good understanding of both Optimistic Concurrency Control (OCC) and Pessimistic Concurrency Control (PCC).

You've correctly pointed out that PCC involves acquiring locks on the data, which allows other transactions to wait until the lock is released. This is a classic approach to ensuring Isolation in databases.

For the given scenario, you've correctly applied PCC by suggesting that the row would be locked while reading, ensuring that either T1 or T2 updates the row sequentially. This is a classic example of using PCC to ensure Isolation.

To further explore this, let's consider a few more aspects:

1. What type of locks would be used in PCC? Would it be shared locks (S-locks) or exclusive locks (X-locks)? How would the locking mechanism handle the scenario where T1 and T2 are trying to update the same row?
2. How would OCC handle the same scenario? Would it use a version number or a timestamp to detect conflicts? How would it handle the retry mechanism?
3. Are there any other concurrency control mechanisms beyond OCC and PCC? For example, what's the role of snapshot isolation or multiversion concurrency control (MVCC) in ensuring Isolation?

Let's dive deeper into these questions and explore the nuances of concurrency control in databases.

---
1. Shared locks will do
2. Occ - detect conflicts Via compare and swap   then t2 retries. It may cause  live lock t2 potentially never finishes 
3. Snapshot isolation keeps a separate copy of data for each transaction and updates the data after the transaction proceeds successfully 
4. Mvcc keeps a copy of data for each transaction but it is more granular than snapshot isolation. 
--- 
Let's examine the issues that may arise when using Optimistic Concurrency Control (OCC) for the scenario where T1 and T2 are trying to update the same row.

Here are some potential issues with OCC in this scenario:

1. **Conflict detection**: When T1 and T2 read the same row, they may not detect a conflict until they try to commit their updates. At this point, one of the transactions (say T2) will detect that the row has been modified by T1 since it was read, and T2 will be aborted.
2. **Abort and retry**: T2 will be aborted and may be retried. However, if T2 is retried, it may again conflict with another transaction that has updated the row, leading to another abort. This can lead to a **livelock** situation, where T2 is repeatedly aborted and retried.
3. **Starvation**: If T1 and other transactions continue to update the row, T2 may be repeatedly aborted and retried, potentially leading to **starvation**, where T2 is unable to make progress.
4. **Increased latency**: The abort and retry mechanism in OCC can lead to increased latency for T2, as it needs to be retried multiple times before it can successfully commit.

These issues highlight some of the challenges associated with using OCC in scenarios where there is high contention between transactions. Now, are you ready to discuss how PCC or other concurrency control mechanisms can address these issues?

---
You're continuing to demonstrate a good understanding of concurrency control mechanisms.

1. You're correct that shared locks (S-locks) could be used, but in the case of updating a row, an exclusive lock (X-lock) would typically be used to prevent other transactions from modifying the same row simultaneously.
2. You've correctly described how OCC would handle the scenario. The compare-and-swap (CAS) mechanism would detect conflicts, and T2 would retry. You're also right that this could potentially lead to a livelock situation, where T2 never finishes due to repeated conflicts and retries.
3. You've got a good understanding of snapshot isolation. It creates a separate snapshot of the data for each transaction, allowing them to operate independently. The updates are then applied to the main data store once the transaction commits successfully.
4. You've also correctly described multiversion concurrency control (MVCC). MVCC is similar to snapshot isolation in that it keeps multiple versions of the data, but it's more granular and can provide better performance and concurrency. MVCC typically uses a combination of version numbers or timestamps to manage the different versions of the data.

To further explore these concepts, let's consider a few more questions:

1. How does MVCC handle garbage collection? As multiple versions of data are created, how does the system clean up old versions that are no longer needed?
2. Can you think of any scenarios where snapshot isolation or MVCC might not provide the desired level of Isolation? For example, what if there are dependencies between transactions that need to be preserved?
3. How do these concurrency control mechanisms interact with other database features, such as indexing or partitioning? Are there any specific considerations or optimizations that need to be made when using these features in conjunction with concurrency control?

Let's continue to dive deeper into these topics and explore the intricacies of concurrency control in databases.