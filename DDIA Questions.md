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

Each question ties back to DDIA concepts while incorporating real-world challenges from the knowledge base (e.g., network unreliability , data consistency , service boundaries ). Let me know if you’d like to dive deeper into any!