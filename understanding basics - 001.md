---
tags:
  - turbopuffer
---
[Twitter](https://x.com/turbopuffer/status/2003504825817006549)
```
we rolled out a new indexing queue on all tpuf shared regions

~10x lower index queue time → new documents get indexed sooner → faster queries on new data with less WAL scanning

built entirely on object storage, no kafka

(chart: max create_index time in queue, gcp us-east4)

```

[Tweet](https://x.com/turbopuffer/status/1998058954149208096) 
```
turbopuffer queries are strongly consistent, which requires scanning new writes in the WAL while indexing happens async. tpuf's WAL scan is now up to 2x faster

DIAGRAM: 

exhaustive WAL search
================================================================================

top_k=10,WAL=1MB

before |████████████████████████████████████████████████████████████| 3.17ms
after  |███████████████████████                                      | 1.57ms
								↑ 49% faster

top_k=10000,WAL=1MB

before |████████████████████████████████████████████████████████████| 11.3ms
after  |████████████████████████████████████                         | 7.31ms
											↑ 35% faster
                                    
-----


instead of merging via BTreeSet, we exploit that both result sets (WAL and indexed) are already sorted, for an efficient iterator merge


rust code :

// SLOW
fn merge_hits(index_hits: BTreeSet<Doc>, wal_hits: BTreeSet<Doc>)
-> BTreeSet<Doc> {
    wal_hits.extend(index_hits); // O(n log (n + m))
    wal_hits
}

// FAST
fn merge_hits(index_hits: BTreeSet<Doc>, wal_hits: BTreeSet<Doc>)
-> Vec<Doc> {
    wal_hits.into_iter().merge(index_hits).collect() // O(n + m)
}


-----



curious about this: are all nodes tailing the WAL? Or are you routing reads to the node that wrote the data being read (leaders). If the former, does that mean consistent reads comes at the cost of data freshness and/or read latency?

------

namespaces stick to 1 node (by default, replicas possible). if the ring changes, get-if-not-match on the WAL metadata file fails and the read is still consistent. that's very rare.


-----

Nice. How are you guys handling leader election (range assignment/discovery) for name spaces? files on object storage with a heartbeat/lease?

-----

a namespace is on one node

-----

Yea, I'm wondering how machine X knows _which_ node owns the namespace. We've been thinking about managing leader assignments in object store (indeed, in the manifest). Was just kinda curious if this is part of tpuf proper, or just some service mesh/router tier that's separate.

----

tpuf-router is a proxy that handles this. it's fairly simple but will grow increasingly sophisticated


```


### Summary Checklist for You

If you want to truly understand that snippet, go learn these three things in order:

1. **LSM Trees:** Why writing is fast (append-only) but reading fresh data is slow (WAL scanning).
    
2. **S3 Strong Consistency:** How S3 changed from a "blob store" to a viable replacement for a file system/queue.
    
3. **Disaggregation:** Moving the state out of the application and into the object store so you can scale indexing (write) and querying (read) independently.