---
tags:
  - master
---
MIT 6.824 course: 

## Lecture 4 - Primary-Backup (P/B) Replication

Questions to answer: 
- what state to replicate?
	- 
- how to do fail-over / cut-over? 
- manage anomalies during fail-over
- how much lag between P/B sync?
- how to bring back a new replica up? -- needs to have a complete state.



### How to cache - python implementation read

[GitHub - rungalileo/gcache: Fine-grained caching framework](https://github.com/rungalileo/gcache)