---
tags:
  - postgres
  - database
---
## ChatGPT learning 
- [x] https://www.perplexity.ai/search/You-are-an-.bxSVNeiQ5GcBQlNY5D1Lg?s=u
- [x] https://chat.openai.com/share/ea632812-abea-4599-abf8-3a4ffc57dac9




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

### pgvector
https://github.com/pgvector/pgvector


### postgres TL query language + dbdev for postgres by supabase


### Partition vs indexes
Q: https://stackoverflow.com/questions/43508486/sql-server-partitioning-vs-indexes
A:


Q: Why partition?
A: The primary use for table partitioning is to speed up bulk data loading and archiving.
Partitioning is never done for  _query performance_. With partitioning the performance will always be worse, the best you can hope for is no big regression, but never improvement.


### validations

library -
1. https://hexdocs.pm/norm/Norm.html
2. goal
3. dolo
4. parameters
