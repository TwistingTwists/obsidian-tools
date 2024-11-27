---
tags:
  - postgres
  - database
---
## ChatGPT learning 
- [x] https://www.perplexity.ai/search/You-are-an-.bxSVNeiQ5GcBQlNY5D1Lg?s=u
- [x] https://chat.openai.com/share/ea632812-abea-4599-abf8-3a4ffc57dac9


Instructor-rs : gbnf parsing
https://aistudio.google.com/app/prompts/1zAA0tc5UqeJ6mu4ewJbVuir29ESftCcZ

# Section 01

You are a SQL database expert. use the tools to analyse the query. Then, step by step think through the example. Include a working example to explain the concept




1. **Normalization Concepts:**
    
    - Question: Explain the benefits and drawbacks of database normalization. Provide an example scenario where denormalization might be justified.
2. **Query Optimization:**
    
    - Question: Given a complex query with multiple joins and subqueries, how would you optimize its performance? Discuss various techniques and considerations in optimizing SQL queries.
3. **Transaction Management:**
    
    - Question: Describe the ACID properties in the context of database transactions. How does a database ensure consistency, and what are the potential issues related to transactions in a distributed database environment?
4. **Indexing Strategies:**
    
    - Question: Compare and contrast clustered and non-clustered indexes. In what scenarios would you choose one over the other? Also, discuss the impact of indexing on insert, update, and delete operations.
5. **Database Security:**
    
    - Question: Outline the key principles and best practices for securing a database. Discuss aspects such as authentication, authorization, encryption, and auditing. How would you handle sensitive data in a database system?



## Udemy course on postgres optimisation 

- [ ] Understanding query 
	- [x] EXPLAIN ANALYZE - execution time. 
- [ ] INDEXES 
	- [x] Index : speed up query by upto 17 times
	- [ ] 

### CMU DB GROUP videos on fundamentals of DB
Intro to DB - https://www.youtube.com/playlist?list=PLSE8ODhjZXjaKScG3l0nuOiDTTqpfnWFf


#### postgres performance
| Topic                                | Link                                                                    | Done? |
| ------------------------------------ | ----------------------------------------------------------------------- | ----- |
| Optimising EXPLAIN query in postgres | https://www.youtube.com/watch?v=RNDTO33hVtY&ab_channel=EDB              |       |
| postgres optimisation                | https://www.youtube.com/watch?v=XA3SBgcZwtE&ab_channel=CMUDatabaseGroup |       |
| optimisation                         | https://www.youtube.com/watch?v=5M2FFbVeLSs                             |       |
| tuning tips                          | https://www.youtube.com/watch?v=dl98eDu_Ssg&ab_channel=EDB              |       |
| postgres execution plan              | https://www.youtube.com/watch?v=Ls-uE1V31lE                             |       |
| explain / analyse                    | https://www.youtube.com/watch?v=nTYE5SsYaJg&ab_channel=TobinBradley     |       |
| subqueries                           | Advanced SQL - Overview of Sub Queries                                  |       |


# Section 02
### postgres performance - JOINS

1. num joins for N tables =~ N! + 3^(N-1) : Too many paths for joins
2. ANALYZE  + autovacuum + 
3. ANALYZE = per column = NULL + avg width + distinct values + Median + historgram for distribution of values of median + estimate logical-physical relation of table + for arrays - most common elements and their frequencies
4. Bad row count estimates = especially when `where` is on two columns.
	1. Run a `sample` query and then optimise
5. 

RnD == execute partial path and produce partial results

## Good questions on query optimisations
1. https://dba.stackexchange.com/questions/303287/postgresql-query-optimisation-with-left-join
2. [optimise join and distincy query ](https://stackoverflow.com/a/50166413)
3. 
4. [Composite Index](https://dba.stackexchange.com/questions/27481/is-a-composite-index-also-good-for-queries-on-the-first-field)
5. 
#### OLAP vs OLTP [Talk](https://www.youtube.com/watch?v=MhE1skm0y7k)

1. OLAP - min indexes (because ad hoc queries)
2. accelerate database queries
   1. use GPU to parallelise workers - PG Storm
   2. Columnar storage - Citus
3. Trade off in using indexes in database
   1. indexed data can be queries fast
   2. each time data changes, index needs to change
4. so for acceleration, use columnar storage engines (CSE) - they store columns separately - if you only need data from 4 columns out of 200 columns , best to use CSE. Otherwise, each row is read from disk, then filetered for 4 columns. so, if you have a lot of joins - you are doing a 'full table scan'


# Section 03
###  [Cloudflare Postgres Usage ](https://www.youtube.com/watch?v=DvblO-f2bqQ)
1. HAProxy + Stolon + pgbouncer + ... tech 
2. 55 Mn req / sec 
3. 15-20 clusters ; one cluster = 3 servers in one region = 3 * 2 (regions) = 6 postgres servers
4. Manage DB - Questions around 
	1. is this data source of truth? 
	2. traffic pattern?  = read heavy / write heavy 
	3. archival process for the data? 
	4. connection counts
	5. size of data and rate of growth in 3 mo / 1 yr
6. Problems 
	1. Connection Hungry applications
	2. Thundering Herd

### Partition vs indexes
Q: https://stackoverflow.com/questions/43508486/sql-server-partitioning-vs-indexes
A:


Q: Why partition?
A: The primary use for table partitioning is to speed up bulk data loading and archiving.
Partitioning is never done for  _query performance_. With partitioning the performance will always be worse, the best you can hope for is no big regression, but never improvement.


# Section 04


  
```md
you are an expert at SQL and relational databases.

ask questions via concrete SQL use cases to check the understanding of transaction isolation and concurrent transactions. include examples in your question
```



## DDIA Chapter - 02  - DB Storage and Retrieval 


##### Simplistic

- k1, v1
- K2, v2
- K3, v3    
- K1, v11


Status 

- W - O(1) 
- Point query - :( 
- Range query - :( 
- Delete - O(1) -> Tombstone
- Fit in memory - yes

  

##### Introduce hash index

  
Status 

- W - O(1) 
- Point query - O(1) 
- Range query - :( 
- Delete - O(1) -> Tombstone
- Fit in memory - yes

##### Introduce Segmentation

File (on-disk) -> broken in fixed size segments. 

  

Segments = ACTIVE (latest) + PASSIVE (immutable) 

  

Status 

- W - O(1) 
- Point query - O(# segments)
- Range query - :( 
- Fit in memory - No  

###### Each segment has its own hash index.

- Good crash recovery -> Crash DURING W + rebuilding indexes after reboot
- Concurrency control - SIMPLE

###### Hash Index is ONE GIANT global file.

Other problems to consider

  

- Concurrency control - R + W 
- Disk overflow => Merging + Compaction
- Crash recovery 

- DURING write 
- Rebuilding indexes

- Read amplification
- Write amplification 

  

##### Each segment sorted by key on disk

=> Sparse index in memory => Hash index for each segment on disk

  

DATA

- ACTIVE (latest , in-memory, sorted-by-key BST)
- PASSIVE (immutable, on-disk, sorted-by-key)
    

  

INDEX 

- Sparse Index (in-memory)
    
- Hash Index for each PASSIVE segment
    

  

Status 

- W - O(log n) (sort in-memory during W)
    
- Point query - O(# segments) 
    
- Range query - O(# segments)
    
- Fit in memory - No
    

  
  

- Concurrency control - R + W => Simple
    
- Crash recovery 
    

- DURING write - :( 
    
- Rebuilding indexes - :) 
    

- Disk overflow  => Merging + Compaction
    
- Read amplification
    
- Write amplification 
    

  

##### WAL for ACTIVE Segment of data

  

DATA

- ACTIVE (latest , in-memory, sorted-by-key BST)
    

- With LOG on disk (WAL)
    

- PASSIVE (immutable, on-disk, sorted-by-key)
    

  

INDEX 

- Sparse Index (in-memory)
    
- Hash Index for each PASSIVE segment
    

  

Status 

- W - O(log n) (sort in-memory during W)
    
- Point query - O(# segments) 
    
- Range query - O(# segments)
    
- Fit in memory - No
    

  
  

- Concurrency control - R + W => Simple
    
- Crash recovery 
    

- DURING write - :)
    
- Rebuilding indexes - :) 
    

- Disk overflow  => Merging + Compaction
    
- Read amplification
    
- Write amplification 
    

  
  

##### In place updates on disk 

- No need to sort on disk :) 
    

  

DATA

- ACTIVE (latest , in-memory, sorted-by-key BST)
    

- With LOG on disk (WAL)
    

- PASSIVE (immutable, on-disk, sorted-by-key)
    

  

INDEX 

- Sparse Index (in-memory)
    
- Hash Index for each PASSIVE segment
    

  

Status 

- W - O(log n) (sort in-memory during W)
    
- Point query - O(# segments) 
    
- Range query - O(# segments)
    
- Fit in memory - No
    

  
  

- Concurrency control - R + W => Simple
    
- Crash recovery 
    

- DURING write - :)
    
- Rebuilding indexes - :) 
    

- Disk overflow  => Merging + Compaction
    
- Read amplification
    
- Write amplification 
    

  
  
  

### BTree vs LSM Tree 

#### LSM 

Write Amplification

- DATA + WAL
    
-  Compaction - Levels (5-6)
    

  

#### BTree

Write Amplification 

- Page Splits**

### DDIA  - Indexes 

- non-clustered index 
- What are the current indexs on the database? 
	- `select relname, relkind from pg_class where relkind = 'i'; `
		- `'i'` for index 
- Visualising B+Tree index = video 009 UDEMY postgres
	- HUGE B+Tree index are stored in levels. 
- Complex Index (compound index? )
- B-Tree index 
- GiST Index 
	- 
- GIN - Arrays / JSON data 
- GeoHashIndex 




#### Rust Databases - SlateDB learnings

###### Testing using cargo test 
 

| Issue                                         | solution                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Testing Rust                                  | What proved to be super useful for Flink was not having per test timeouts, but instead having a tooling around maven that first does a full thread dump and then kills the suite from outside. Without thread dumps you have very little to no context to start from in case something gets stuck. Not sure how realistic is to get a thread dump for timed out tests in rust though. Maybe something along the lines of [https://docs.rs/rstack-self/0.3.0/rstack_self/](https://docs.rs/rstack-self/0.3.0/rstack_self/ "https://docs.rs/rstack-self/0.3.0/rstack_self/") could help |
| abort a test if it runs longer than x seconds | cargo nextest                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| async rust doesn't have backtraces            | databend uses this crate to wrap stack traces for async [https://github.com/tokio-rs/async-backtrace](https://github.com/tokio-rs/async-backtrace "https://github.com/tokio-rs/async-backtrace") (it's a pain to manually wrap EVERY async function to wrap this stack though..)<br><br><br>one of my colleagues built [https://github.com/petrosagg/tasktrace](https://github.com/petrosagg/tasktrace "https://github.com/petrosagg/tasktrace") to address that                                                                                                                      |

