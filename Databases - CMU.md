## Chapter 01
- [ ] unique key and fkey constraints are the most popularly used
- [ ] DML - data manipulation languages
- [ ] PJ SIP DU 


## Chapter 02
- [ ] SQL - based on bags (duplicates) 
- [ ] Aggregates - Avg, SUM, MIN MAX COUNT
- [ ] CTE
- [ ] Window Functions
- [ ] 


Revise DB stuff quickly 


 https://github.com/aphyr/distsys-class#an-introduction-to-distributed-systems 
  [https://martinfowler.com/articles/patterns-of-distributed-systems/](https://martinfowler.com/articles/patterns-of-distributed-systems/ "https://martinfowler.com/articles/patterns-of-distributed-systems/") [https://github.com/systemdesignfightclub/SDFC/tree/main](https://github.com/systemdesignfightclub/SDFC/tree/main "https://github.com/systemdesignfightclub/SDFC/tree/main")

Database Query Optimisations

- [ ] https://clickhouse.com/blog/apache-parquet-clickhouse-local-querying-writing



### Unions 

UNION ALL vs LEFT JOIN 



---


### Cassandra + Leaderless

##### Strongly consistent read

R + W > N

=> ONE overlap Node among R/W

→ Quorums


###### Recovery

- anti Entropy (Sync from peers)

- Read repair = Only if reading ~ all keys frequently



##### Scaling R/W

- W + R > N → W = N - 1, R = 1

- read scaled


Sloppy Quorum => Strongly consistent read? (☹) ✗


--- 


### Ecto Repo optimisation for buffered writer 


- so that all write queries happen at the same time. 
	- not reducing number of queries, but *less* postgres pages are written to disk 

- write segregations to same table. 
- traffic shaping - disk optimisation? 