 # Query Cache Mechanisms in Major Log Observability Systems: A Comprehensive Technical Analysis

This technical analysis delves into the architecture, implementation strategies, and performance characteristics of query caching mechanisms in four major observability systems: Elasticsearch, GreptimeDB, InfluxDB, and Quickwit. Query caching serves as a critical performance optimization technique in these systems, dramatically reducing latency for repeated queries and minimizing resource consumption. Each system implements distinct approaches to caching that reflect their architectural foundations, target workloads, and performance priorities. This report examines the technical underpinnings of these caching mechanisms, their effectiveness for observability workloads, and the trade-offs inherent in their design decisions.

## Elasticsearch Query Cache Architecture

Elasticsearch implements a sophisticated multi-layered caching architecture that includes the node query cache, shard request cache, and field data cache - each serving distinct purposes in the query execution pipeline. The node query cache, previously known as the filter cache, specifically targets queries used in filter contexts, optimizing boolean predicates that don't contribute to document scoring but narrow the result set[^1]. This cache is shared across all shards on a node, operating as a global resource that maximizes cache utilization across diverse workloads. As evident in the IndicesQueryCache.java implementation, Elasticsearch employs a Least Recently Used (LRU) eviction policy managed through an ElasticsearchLRUQueryCache class[^2].

The underlying architecture utilizes concurrent hash maps that track both queries and their usage patterns to inform intelligent caching decisions. Cache entries store the results of filter operations, allowing subsequent queries to bypass expensive computations when the same filters are applied. Importantly, Elasticsearch doesn't cache indiscriminately - it implements selective caching based on query frequency and segment size. According to the IndicesQueryCache implementation, Elasticsearch maintains a query history to determine cache eligibility, ensuring that only queries with demonstrated repeated use are considered for caching[^2]. This selective approach prevents one-off queries from polluting the cache while optimizing performance for frequently executed filter operations.

Cache invalidation in Elasticsearch occurs at the segment level, meaning when segments merge during background optimization, cached queries associated with those segments are automatically invalidated[^1]. This granular approach ensures cache coherence without requiring complete cache-wide invalidations that would reduce overall efficiency. The node query cache is constrained by both size and entry count limits, with defaults allowing a maximum of 10,000 queries occupying up to 10% of the available heap space[^1]. This dual-constraint approach prevents either numerous small queries or a few memory-intensive queries from monopolizing cache resources.

### Memory Management and Distributed Coordination

Memory management in Elasticsearch's query cache demonstrates sophisticated resource allocation across shards and nodes. The implementation tracks both overall memory usage and individual shard contributions, with methods like `getShareOfAdditionalRamBytesUsed` calculating how to distribute shared overhead proportionally based on each shard's cache footprint[^2]. This detailed accounting ensures fair resource allocation in multi-tenant environments where different workloads compete for the same cache resources. The system balances memory efficiency against performance by tracking both entry counts and byte usage, implementing protections through configurable thresholds.

In distributed deployments, Elasticsearch's query cache operates independently on each node without direct cross-node coordination, prioritizing scalability and resilience over global cache consistency. This node-autonomy approach avoids the coordination overhead that would come with maintaining coherence across potentially hundreds of nodes. Instead, Elasticsearch relies on its consistent sharding mechanism to route similar queries to the same nodes, naturally leading to effective cache utilization without explicit coordination protocols[^16]. The system doesn't implement specific cache warming after node failures, instead relying on normal query patterns to gradually repopulate caches as queries resume execution against newly allocated shards.

### Query Execution Integration and Observability Optimizations

The query cache interacts with Elasticsearch's query planning primarily during filter execution, potentially bypassing expensive computations when cached results are available. This integration is particularly valuable for complex boolean predicates that would otherwise consume significant CPU resources. However, as noted in GitHub issue \#16259, the default `UsageTrackingQueryCachingPolicy` which uses `CacheOnLargeSegments` may avoid caching expensive queries on small segments, sometimes leading to performance degradation when these queries are repeatedly executed[^3].

For observability-specific concerns, Elasticsearch's caching demonstrates both strengths and limitations. The system doesn't feature specialized structures for high-cardinality tag data, instead relying on its general-purpose caching mechanisms. Time-series specific query patterns, common in observability workloads, face particular challenges as queries containing relative time references (like "now-1d") are typically not cached because they depend on the current time[^17]. This limitation significantly impacts dashboards that frequently refresh with time-relative queries, requiring careful query construction using absolute time references to optimize cache utilization.

### Configuration Parameters and Design Trade-offs

Elasticsearch provides several configurable parameters for fine-tuning cache behavior, primarily through `indices.queries.cache.size` (controlling heap allocation percentage) and `indices.queries.cache.count` (limiting maximum cached query count)[^1]. Administrators can enable or disable caching at the index level using `index.queries.cache.enabled`, allowing for targeted optimization strategies for different data collections[^1]. These configuration options balance memory utilization against query performance, though proper tuning requires understanding workload characteristics.

The design trade-offs in Elasticsearch's cache implementation reflect its position as a general-purpose search engine rather than a specialized observability solution. By caching only filter context queries and selectively based on frequency and segment size, Elasticsearch prioritizes efficient resource utilization over maximizing cache hits for all possible queries[^3]. This approach optimizes for common search patterns while potentially underperforming for specialized workloads, particularly those with expensive computations on small segments. The lack of an API to directly influence caching decisions for specific queries represents another trade-off that prioritizes automatic management over fine-grained control[^3].

## GreptimeDB Query Cache Architecture

GreptimeDB implements a multi-tiered caching architecture specifically designed for time-series data workloads, with distinct caches serving different aspects of query execution. The system employs at least five different cache types: a page cache for SST (Sorted String Table) row groups, a selector result cache for time series operations like `last_value()`, a write cache for recently written data, an SST metadata cache, and a vector cache for in-memory representations[^12]. This specialized architecture reflects GreptimeDB's focus on time-series workloads, where different access patterns require tailored optimization strategies.

The core algorithms leverage temporal locality inherent in time-series data, where recent data receives more frequent access than historical information. While specific implementation details aren't fully documented in the search results, the configuration options reveal an architecture designed to bridge the latency gap between local and remote storage systems[^12]. This is particularly evident in the write cache implementation, which is "recommended to enable when using object storage for better performance"[^12]. The write cache serves as a buffer between application writes and object storage, allowing for faster access to recently written data without requiring immediate remote storage operations.

Cache invalidation in GreptimeDB appears to use time-based mechanisms, particularly for the write cache which has a configurable TTL parameter[^12]. This time-based approach aligns well with time-series data characteristics, where data relevance typically diminishes with age. However, GitHub issue \#2516 indicates challenges with "mismatches between local and remote, resulting in duplicated inserts" and "`__last_checkpoint`, resulting in local dirty cache"[^10]. These issues point to coherence challenges between the cache and underlying storage, particularly in distributed deployments.

### Memory Management and Distributed Coordination

Memory management in GreptimeDB uses fractional allocation, with each cache type allocated a percentage of system memory. The page cache defaults to 1/8 of OS memory, the selector result cache to 1/16 (maximum 512MB), and the vector cache to 1/16 (maximum 512MB)[^12]. This proportional allocation automatically scales cache sizes with available resources, while still providing maximum limits to prevent excessive consumption. Administrators can disable specific caches entirely by setting their size to zero, providing flexibility for different deployment scenarios[^12].

The distributed caching approach appears to be evolving, with GitHub issue \#2516 proposing improvements to "combine write-back, synchronously updating the cache in the local storage while writing recent data to remote object storage"[^10]. This suggests a move toward a coordinated approach that balances consistency between local caches and remote storage while maintaining performance. The specific coordination protocols aren't detailed in the search results, but the proposal indicates recognition of distributed caching challenges in cloud-native environments.

### Query Execution Integration and Observability Optimizations

GreptimeDB's selector result cache provides insight into how the system optimizes for time-series specific operations. This cache targets "time series selector (e.g. `last_value()`)" operations[^12], which are fundamental in observability workloads where retrieving the most recent value for metrics is a common requirement. By caching these selector results, GreptimeDB can efficiently respond to queries that would otherwise require scanning multiple time partitions to find the most recent values.

For high-cardinality data handling, GreptimeDB's approach appears to focus on caching results for frequently accessed series rather than attempting to cache all dimension combinations. The selector result cache, with its 512MB default maximum size, would prioritize "hot" series over less frequently accessed ones[^12]. This approach allows the system to deliver good performance for commonly accessed metrics without requiring prohibitive amounts of memory to cache all possible combinations.

### Configuration Parameters and Design Trade-offs

GreptimeDB offers comprehensive configuration parameters for tuning cache behavior, including `page_cache_size`, `selector_result_cache_size`, `enable_write_cache`, `write_cache_path`, `write_cache_size`, and `write_cache_ttl`[^12]. These settings allow administrators to optimize for different workload characteristics and hardware configurations, controlling which caches are enabled, their size limits, and data retention policies. The default values suggest a balance between memory utilization and cache effectiveness suitable for many deployments.

The design trade-offs reflect GreptimeDB's specialization for time-series data and observability workloads. The allocation of a dedicated cache for selector results prioritizes the common pattern of accessing recent values, potentially at the expense of other query types. Similarly, the recommendation to enable write cache for object storage deployments acknowledges cloud architecture challenges, prioritizing query performance over potential consistency issues between local and object storage[^10]. These decisions position GreptimeDB as a specialized time-series database optimized for observability workloads.

## InfluxDB Query Cache Architecture

InfluxDB 3, spanning both Core and Enterprise editions, implements a sophisticated caching architecture with specialized components targeting different data access patterns common in observability workloads. The system features at least three primary cache types: the Parquet memory cache for accelerating file access, the Last Value Cache (LVC) for optimizing recent value queries, and the Distinct Value Cache for efficiently handling unique value retrieval[^8]. This multi-tiered approach reflects InfluxDB's evolution into a columnar, Parquet-based storage engine while maintaining optimizations for common time-series query patterns.

The Last Value Cache (LVC) represents a particularly innovative approach to addressing a fundamental observability query pattern: retrieving the most recent values for specific metrics. As described in the documentation, the LVC is "an in-memory cache that stores the last N number of values for specific fields of series in a table," allowing users to specify which fields to cache, which tags identify each series, and how many values to cache per series[^9]. This targeted design demonstrates deep understanding of observability workloads where current state queries (e.g., "what is the current CPU utilization across all hosts?") are extremely common.

Cache invalidation in InfluxDB primarily uses time-based eviction, with configurable intervals controlling how frequently expired entries are removed from different cache types. The `last-cache-eviction-interval` and `distinct-cache-eviction-interval` parameters (both defaulting to 10 seconds) determine how often InfluxDB checks for and removes expired entries[^8]. This temporal approach aligns naturally with time-series data, where cache entries progressively lose relevance as newer data arrives. The Parquet memory cache uses a more complex strategy, with `parquet-mem-cache-prune-interval` (default: 1 second) controlling pruning frequency and `parquet-mem-cache-prune-percentage` (default: 0.1) determining what fraction of entries are removed during pruning operations[^8].

### Memory Management and Distributed Coordination

Memory management in InfluxDB is handled through configurable size limits, with `parquet-mem-cache-size-mb` (default: 1000MB) controlling the maximum memory allocation for the Parquet cache[^8]. The system provides the option to completely disable the Parquet memory cache through the `disable-parquet-mem-cache` parameter, offering flexibility for memory-constrained environments[^8]. The `preemptive-cache-age` parameter (default: 3 days) adds another dimension by controlling prefetching scope during compaction operations, balancing cache completeness against memory usage[^8].

In distributed deployments, InfluxDB Enterprise allows cache configurations to target specific nodes through the `--node-spec` parameter when creating a Last Value Cache[^9]. This node-specific configuration implies a distributed architecture where each node maintains its own cache instances, with administrative control over which nodes participate in caching. The time-based eviction intervals suggest that InfluxDB relies on temporal invalidation rather than explicit cross-node coordination to maintain cache freshness in distributed environments.

### Query Execution Integration and Observability Optimizations

InfluxDB's cache architecture is directly exposed to users through SQL functions, with `last_cache()` retrieving data from the Last Value Cache and `distinct_cache()` accessing the Distinct Value Cache[^5][^15]. This explicit cache access model gives users and applications direct control over when to use specialized caches versus standard query paths, allowing for efficient handling of known query patterns. The SQL integration means caches are directly accessible through the same query language used for data analysis, simplifying implementation for application developers.

For high-cardinality data challenges, InfluxDB's Last Value Cache offers a flexible solution by allowing users to specify exactly which tags identify series in the cache. When creating an LVC, users can precisely control "what fields to cache, what tags to use to identify each series, and the number of values to cache for each unique series"[^9]. This targeted approach allows balancing cache efficiency against query flexibility by including only necessary tags in the cache key, effectively reducing cardinality to manageable levels. The ability to cache multiple recent values per series, not just the single most recent value, provides flexibility for queries examining recent trends or comparing current values to historical references.

### Configuration Parameters and Design Trade-offs

InfluxDB offers extensive configuration parameters for cache tuning, allowing optimization for different workload characteristics. For the Parquet memory cache, key parameters include `parquet-mem-cache-size-mb`, `parquet-mem-cache-prune-percentage`, `parquet-mem-cache-prune-interval`, and `disable-parquet-mem-cache`[^8]. For the Last Value Cache, creation parameters include key columns (tags), value columns (fields), count (values per series), and TTL (time-to-live)[^9]. This granular control allows administrators to precisely balance memory utilization against query performance.

The design trade-offs in InfluxDB's cache implementation reflect its position as a purpose-built time-series database for observability workloads. The specialized Last Value Cache and Distinct Value Cache prioritize common observability query patterns over general-purpose approaches[^9]. The explicit cache access model trades transparency for control, requiring users to understand when specialized caches are appropriate. Time-based eviction prioritizes freshness over maximizing hit rates, acknowledging that in observability scenarios, slightly stale data can be more problematic than cache misses. These decisions optimize InfluxDB for time-series analytics and observability use cases with high-cardinality challenges.

## Quickwit Query Cache Architecture

Quickwit implements a multi-layered caching architecture designed to optimize search performance across different aspects of its query pipeline, with both in-memory and on-disk components serving distinct purposes. According to the documentation, Quickwit's in-memory caching includes three primary mechanisms: Hotcache for split file metadata, Fast field caching for frequently accessed fields, and Partial request caching for similar queries with varying parameters[^6]. This layered approach optimizes different phases of query execution, from file access to field retrieval to result reuse.

The on-disk Split cache represents a distinctive aspect of Quickwit's architecture, storing entire splits (logical data partitions) on disk to reduce object store costs and latency[^6]. This approach is particularly valuable for cloud deployments where object storage access incurs both performance penalties and monetary costs. As described in the documentation, "Searchers populate this cache when splits are created or queried and evict them with a simple LRU strategy"[^6]. The Fast field caching mechanism targets fields that "tend to be accessed very frequently by users especially for stream requests," demonstrating understanding of common search patterns in observability workloads[^6].

Cache invalidation in Quickwit uses both time-based and size-based strategies. For the on-disk Split cache, "splits are evicted if they are older than two days," providing a time-based mechanism that aligns with temporal access patterns[^11]. Additionally, "if cache limits are reached, oldest splits are evicted," implementing an age-based eviction policy when size constraints require removing entries[^11]. This dual approach ensures both that stale data is eventually purged and that cache size remains within configurable limits, preventing unlimited growth that could exhaust storage resources.

### Memory Management and Distributed Coordination

Memory management in Quickwit uses configurable capacity limits for each cache type, with parameters like `split_footer_cache_capacity`, `fast_field_cache_capacity`, and `partial_request_cache_capacity` controlling the maximum size of in-memory caches[^6]. For the on-disk Split cache, administrators can configure "the number of splits the cache can hold with `split_store_max_num_splits` and limit the overall size in bytes of splits with `split_store_max_num_bytes`"[^11]. This flexible configuration allows balancing cache size against other resource needs based on specific workload characteristics.

The documentation indicates that the "default cache limits of 1000 splits and 100GB are good enough to avoid downloading splits from the storage for the first two merges," suggesting calibration for common use cases while allowing customization[^11]. When using partitioning, which creates more splits, administrators may need to increase cache limits as "Quickwit will create one split per partition"[^11]. This guidance shows awareness of how different deployment patterns affect optimal cache configurations.

### Query Execution Integration and Observability Optimizations

Quickwit's caching architecture demonstrates clear focus on common observability query patterns. The Fast field cache specifically targets fields "accessed very frequently by users especially for stream requests," a common pattern where certain fields (like timestamps or host identifiers) appear in almost every query[^6]. By keeping these fields in memory, Quickwit accelerates queries without requiring prohibitive memory to cache all fields.

The Partial request caching system optimizes for scenarios where "some very similar requests might be issued, with only timestamp bounds changing," a pattern frequently observed in dashboard and monitoring use cases[^6]. This optimization is particularly valuable for observability workloads where the same query structure repeats over different time windows, such as dashboard panels that refresh regularly with updated time ranges.

### Configuration Parameters and Design Trade-offs

Quickwit provides several configuration parameters for tuning cache behavior, allowing optimization for different workload characteristics. Parameters include cache capacity settings for each cache type and specific controls for the Split cache like `split_store_max_num_splits` and `split_store_max_num_bytes`[^11]. The documentation offers guidance on "setting the right splits cache limits" based on factors like merge policy and partition count, acknowledging that optimal sizing depends on workload patterns[^11].

The design trade-offs in Quickwit's caching reflect its position as a specialized search engine for log data. The allocation of separate caches for different pipeline aspects prioritizes optimizing common access patterns over a one-size-fits-all approach. The on-disk Split cache trades disk space for reduced object storage access, acknowledging performance and cost implications of cloud storage[^6]. The time-based eviction strategy prioritizes recent data over historical information, aligning with observability access patterns where query frequency typically decreases with data age. These decisions position Quickwit as a specialized solution for log and event data search.

## Comparative Analysis of Query Cache Implementations

### High-Cardinality Data Approaches

The four systems employ distinctly different strategies for high-cardinality data in their cache structures, reflecting varying design philosophies and target workloads. Elasticsearch relies primarily on its node query cache for filter operations without specialized mechanisms for high-cardinality dimensions[^1]. This approach works for moderate cardinality but may struggle with billions of unique dimension combinations common in observability workloads. GreptimeDB adopts a more specialized approach with its selector result cache optimized for time series selectors like `last_value()`, effectively prioritizing frequently accessed series[^12].

InfluxDB takes specialization further with its Last Value Cache, allowing users to explicitly specify which tags identify series in the cache, providing fine-grained control over the cardinality-memory trade-off[^9]. This approach enables precise targeting of which dimensions participate in cache keys, effectively managing the explosion of combinations. Quickwit focuses on optimizing underlying data access through its Split cache and Fast field cache rather than attempting to cache results for all dimension combinations[^6]. For environments with extremely high cardinality, InfluxDB's explicit tag selection and Quickwit's split-based approach likely offer the most scalable solutions.

### Performance Characteristics and Workload Suitability

Performance characteristics vary significantly under different query patterns, reflecting architectural differences and optimization priorities. Elasticsearch's node query cache excels at accelerating repeated filter operations but, as noted in issue \#16259, its selective caching based on segment size can lead to suboptimal performance for "queries that are quite heavy to construct and compute, even on small segments"[^3]. GreptimeDB's specialized caches deliver excellent performance for recent data queries in time-series workloads, particularly with write cache enabled for object storage deployments[^12].

InfluxDB's Last Value Cache offers potentially the best performance for current state queries, while its Parquet memory cache accelerates access to historical data[^9]. Quickwit's multi-layered approach, with separate caches for split metadata, fast fields, and partial results, delivers good performance across various query types, particularly for log search workloads[^6]. For time-series specific patterns, the specialized systems (GreptimeDB, InfluxDB, and Quickwit) generally outperform Elasticsearch due to purpose-built optimizations. InfluxDB's explicit `last_cache()` function provides near-instantaneous responses for current state queries without requiring data scanning[^5][^15].

### Cache Coherence and Distributed Design

Approaches to cache coherence and distributed coordination vary significantly, reflecting different trade-offs between consistency, availability, and partition tolerance. Elasticsearch adopts node-local caching without direct cross-node coordination, prioritizing performance and scalability over global consistency[^16]. This works well with consistent sharding but can cause inefficiencies in small clusters where similar queries might execute on different nodes with cold caches. GreptimeDB appears to be evolving toward more coordination, with issue \#2516 proposing a write-back strategy that would "synchronously update the cache in local storage while writing recent data to remote object storage"[^10].

InfluxDB's node-specific cache configuration indicates a controlled approach to cache distribution, potentially allowing specialized caching nodes[^9]. Quickwit's split-based approach with its on-disk cache suggests a node-local strategy optimized for cloud deployments where object storage access is expensive[^11]. For cache warming after failures, none of the systems implement explicit mechanisms based on the search results, likely relying on normal query patterns to gradually repopulate caches. These design decisions reflect the distributed systems principle that perfect consistency, availability, and partition tolerance cannot be simultaneously achieved.

### Memory Overhead and Scaling Characteristics

Memory overhead characteristics reflect different architectural approaches and configuration philosophies. Elasticsearch's node query cache, defaulting to 10% of heap space with maximum 10,000 queries, represents a bounded but potentially significant memory commitment in large clusters[^1]. The proportional accounting mechanism visible in IndicesQueryCache.java demonstrates attention to fair resource allocation in multi-tenant environments[^2]. GreptimeDB adopts a fractional allocation approach, with page cache at 1/8 of OS memory and other caches at proportional sizes[^12]. This adaptive scaling works well in heterogeneous clusters but requires monitoring to prevent excessive total allocation.

InfluxDB takes a more fixed approach for its Parquet memory cache, defaulting to 1000MB regardless of total memory[^8]. This provides predictability but might cause suboptimal resource utilization on nodes with significantly different memory capacities. Quickwit's configurable parameters for each cache type, with guidance on sizing based on workload characteristics, show awareness of deployment diversity[^11]. All four systems employ node-local caching approaches, resulting in linear scaling of total cache capacity with node count. This avoids coordination overhead but can cause cache redundancy for workloads where similar queries execute on different nodes.

### Consistency vs. Performance Trade-offs

The systems demonstrate different approaches to the classic consistency versus performance trade-off in their implementations. Elasticsearch's segment-level invalidation, where cached queries for merged segments are automatically invalidated, balances freshness against performance without requiring full cache clears[^1]. This works well for append-only workloads but might cause unnecessary invalidations in update-heavy scenarios. GreptimeDB's proposed write-back strategy represents an effort to improve consistency-performance balance, acknowledging challenges in maintaining coherence between local caches and remote storage[^10].

InfluxDB's time-based eviction strategy, with configurable intervals defaulting to 10 seconds, prioritizes cache freshness over maximizing hit rates[^8]. This approach recognizes that in observability workloads, stale data can be worse than no cached data at all. Quickwit's dual invalidation approach, evicting splits older than two days or when cache limits are reached, similarly balances freshness against performance while maintaining bounded resource usage[^11]. Across all four systems, we see prioritization of query performance for reads while maintaining reasonable freshness guarantees, aligning with the read-heavy nature of most observability workloads.

## Conclusion

This comprehensive analysis of query cache mechanisms in Elasticsearch, GreptimeDB, InfluxDB, and Quickwit reveals distinct architectural approaches and optimization strategies tailored to different aspects of observability workloads. Elasticsearch provides a general-purpose caching system with selective query caching based on frequency and segment characteristics, suitable for diverse query patterns but potentially suboptimal for specialized time-series access. GreptimeDB implements multiple specialized caches focused on time-series operations, with particular strength in caching selector operations and recent data access patterns. InfluxDB offers perhaps the most observability-oriented approach with its explicit Last Value Cache and SQL integration, providing direct control over caching for current state queries. Quickwit combines both in-memory and on-disk caching mechanisms optimized for log search and analysis, with particular attention to cloud deployment scenarios.

These differences highlight how caching strategies reflect fundamental architectural decisions and target workload assumptions. For high-cardinality observability data, InfluxDB's explicit dimension selection and Quickwit's split-based approach offer the most scalable solutions. For performance characteristics, the specialized systems generally outperform Elasticsearch for time-series specific patterns, while Elasticsearch offers more balanced performance across diverse query types. In distributed deployments, all four systems make different trade-offs between consistency, availability, and partition tolerance, with varying degrees of coordination between node-local caches. Understanding these distinctions enables system architects and operators to select the appropriate technology based on their specific observability requirements, workload characteristics, and deployment constraints.

<div style="text-align: center">⁂</div>

[^1]: https://www.elastic.co/guide/en/elasticsearch/reference/current/query-cache.html

[^2]: https://github.com/elastic/elasticsearch/blob/master/server/src/main/java/org/elasticsearch/indices/IndicesQueryCache.java

[^3]: https://github.com/elastic/elasticsearch/issues/16259

[^4]: https://discuss.elastic.co/t/query-cache-is-always-empty/355582

[^5]: https://docs.influxdata.com/influxdb3/enterprise/reference/sql/functions/cache/

[^6]: https://quickwit.io/docs/main-branch/overview/concepts/querying

[^7]: https://www.npmjs.com/package/quick-lru

[^8]: https://docs.influxdata.com/influxdb3/core/reference/config-options/

[^9]: https://docs.influxdata.com/influxdb3/enterprise/admin/last-value-cache/

[^10]: https://github.com/GreptimeTeam/greptimedb/issues/2516

[^11]: https://quickwit.io/docs/operating/data-directory

[^12]: https://github.com/GreptimeTeam/greptimedb/blob/main/config/config.md

[^13]: https://www.elastic.co/blog/elasticsearch-caching-deep-dive-boosting-query-speed-one-cache-at-a-time

[^14]: https://bigdataboutique.com/blog/properly-use-elasticsearch-query-cache-to-accelerate-search-performance-9566ad

[^15]: https://docs.influxdata.com/influxdb3/core/reference/sql/functions/cache/

[^16]: https://opster.com/guides/elasticsearch/glossary/elasticsearch-cache/

[^17]: https://opensourceconnections.com/blog/2017/07/10/caching_in_elasticsearch/

[^18]: https://help.hcl-software.com/commerce/9.1.0/search/concepts/csdcacheinvalid.html

[^19]: https://stackoverflow.com/questions/25212911/how-to-turn-off-caching-when-testing-elastic-search

[^20]: https://stackoverflow.com/questions/48536363/why-is-my-elasticsearch-query-cache-empty

[^21]: https://github.com/elastic/elasticsearch/blob/master/server/src/main/java/org/elasticsearch/index/IndexModule.java

[^22]: https://discuss.elastic.co/t/shard-query-cache-size/23663

[^23]: https://discuss.elastic.co/t/shard-request-cache-entirely-or-partially-invalidated-due-to-indexing/228178

[^24]: https://www.elastic.co/guide/en/elasticsearch/reference/8.18/query-cache.html

[^25]: https://docs.spring.io/spring-data/elasticsearch/reference/elasticsearch/repositories/elasticsearch-repository-queries.html

[^26]: https://github.com/elastic/elasticsearch/issues/26938

[^27]: https://discuss.elastic.co/t/query-cache-not-being-used/156610

[^28]: https://discuss.elastic.co/t/filter-cache-invalidation/17490

[^29]: https://www.youtube.com/watch?v=B14thcZmkqs

[^30]: https://www.elastic.co/blog/elasticsearch-caching-deep-dive-boosting-query-speed-one-cache-at-a-time

[^31]: https://clickhouse.com/docs/operations/query-cache

[^32]: https://learn.microsoft.com/en-us/kusto/management/cache-policy?view=microsoft-fabric

[^33]: https://www.elastic.co/docs/api/doc/elasticsearch/operation/operation-search

[^34]: https://stackoverflow.com/questions/77294088/how-can-i-invalidate-cache-only-on-specific-rtk-query-calls

[^35]: https://artifacts.elastic.co/javadoc/org/elasticsearch/elasticsearch/8.17.0/org.elasticsearch.server/org/elasticsearch/common/lucene/search/function/package-tree.html

[^36]: https://dev.mysql.com/doc/refman/5.7/en/query-cache.html

[^37]: https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/using-managed-cache-policies.html

[^38]: https://www.elastic.co/guide/en/kibana/current/lucene-query.html

[^39]: https://stackoverflow.com/questions/161427/automatic-query-cache-invalidation

[^40]: https://discuss.elastic.co/t/noclassdef-org-apache-lucene-util-version/315787

[^41]: https://mariadb.com/kb/en/query-cache/

[^42]: https://github.com/influxdata/influxdb/issues/207

[^43]: https://github.com/influxdata/influxdb-client-js/issues/739

[^44]: https://docs.influxdata.com/enterprise_influxdb/v1/flux/get-started/

[^45]: https://github.com/influxdata/influxdb/issues/18791

[^46]: https://tdengine.com/cache-in-speed-out-influxdb-vs-tdengine-in-real-time-data/

[^47]: https://github.com/influxdata/influxdb/issues/6109

[^48]: https://stackoverflow.com/questions/63802872/aggregate-flux-query-in-influxdb

[^49]: https://docs.influxdata.com/influxdb/v1/troubleshooting/frequently-asked-questions/

[^50]: https://docs.influxdata.com/influxdb3/core/tags/cache/

[^51]: https://github.com/influxdata/influxdb/issues/5499

[^52]: https://github.com/influxdata/influxdb/issues/18088

[^53]: https://www.reddit.com/r/nextjs/comments/1fdho5a/how_to_invalidate_cache_at_a_given_time_passing/

[^54]: https://docs.greptime.com/user-guide/administration/performance-tuning-tips

[^55]: https://github.com/GreptimeTeam/greptimedb/issues/5446

[^56]: https://db-engines.com/en/system/GreptimeDB;InfluxDB;NCache

[^57]: https://github.com/GreptimeTeam/greptimedb/issues/2516

[^58]: https://engineering.contentsquare.com/2022/quickwit-and-ch-in-cluster-context/

[^59]: https://quickwit.io/docs/reference/cli

[^60]: https://github.com/quickwit-oss/quickwit/issues/5445

[^61]: https://docs.greptime.com/0.10/user-guide/administration/performance-tuning-tips/

[^62]: https://www.bookstack.cn/read/greptimedb-0.8-en/e2e24db009069c67.md?wd=loki

[^63]: https://github.com/GreptimeTeam/greptimedb/blob/main/config/config.md

[^64]: https://quickwit.io/docs/reference/metrics

[^65]: https://quickwit.io/docs/get-started/quickstart

[^66]: https://github.com/influxdata/influxdb/issues/25562

[^67]: https://github.com/influxdata/influxdb/issues/25183

[^68]: https://github.com/influxdata/influxdb/issues/25607

[^69]: https://github.com/influxdata/influxdb/issues/19015

[^70]: https://www.youtube.com/watch?v=J4syKnsqQmg

[^71]: https://docs.influxdata.com/influxdb3/enterprise/reference/sql/functions/cache/

[^72]: https://github.com/influxdata/influxdb/issues/5946

[^73]: https://docs.influxdata.com/influxdb/v1/administration/config/

[^74]: https://github.com/influxdata/influxdb

[^75]: https://docs.influxdata.com/influxdb/v1/concepts/key_concepts/

[^76]: https://github.com/influxdata/influxdb/issues/3792

[^77]: https://github.com/influxdata/influxdb/issues/25887

[^78]: https://docs.influxdata.com/influxdb3/core/get-started/

[^79]: https://x.com/InfluxDB/status/1878906525273735606

[^80]: https://www.youtube.com/watch?v=AGS4GNGDK_4

[^81]: https://stackoverflow.com/questions/74618268/efficient-way-to-get-last-value-with-flux-influxdb

[^82]: https://www.influxdata.com/products/influxdb-overview/

[^83]: https://arrow.apache.org/blog/2022/12/26/querying-parquet-with-millisecond-latency/

[^84]: https://stackoverflow.com/questions/29193898/influxdb-getting-only-last-value-in-query

[^85]: https://docs.influxdata.com

[^86]: https://www.youtube.com/watch?v=EUgn8aa3kmI

[^87]: https://bsky.app/profile/influxdb.com/post/3lhtq2nziqs22

[^88]: https://docs.influxdata.com/influxdb3/core/release-notes/

[^89]: https://github.com/GreptimeTeam/greptimedb/issues/4813

[^90]: https://github.com/GreptimeTeam/greptimedb-operator/issues/201

[^91]: https://github.com/GreptimeTeam/greptimedb/issues/5268

[^92]: https://www.greptime.com/blogs/2025-03-10-log-benchmark-greptimedb

[^93]: https://greptime.com/blogs/2025-03-18-jsonbench-greptimedb-performance

[^94]: https://github.com/GreptimeTeam/greptimedb/issues/2434

[^95]: https://greptimedb.rs/src/mito2/lib.rs.html

[^96]: https://github.com/GreptimeTeam/greptimedb/issues/1790

[^97]: https://greptimedb.rs/src/store_api/mito_engine_options.rs.html

[^98]: https://dev.to/greptime/greptime-user-guide-how-to-store-greptimedb-data-on-aws-s3-147c

[^99]: https://greptimedb.rs/cache/index.html

[^100]: https://github.com/GreptimeTeam/greptimedb/blob/main/config/standalone.example.toml

[^101]: https://github.com/quickwit-oss/quickwit/pulls

[^102]: https://github.com/quickwit-oss/quickwit/blob/main/CONTRIBUTING.md

[^103]: https://newreleases.io/github/quickwit-oss/quickwit

[^104]: https://lib.rs/crates/quickwit-config

[^105]: https://docs.rs/crate/quickwit-indexing/latest

[^106]: https://quickwit.io/docs/reference/rest-api

[^107]: https://github.com/orgs/quickwit-inc/repositories

[^108]: https://pkg.go.dev/github.com/quickwit-oss/quickwit-datasource/pkg/quickwit/client

[^109]: https://www.codetriage.com/quickwit-oss/quickwit

[^110]: https://newreleases.io/project/github/quickwit-oss/quickwit/release/v0.8.0

[^111]: https://raw.githubusercontent.com/quickwit-oss/quickwit/main/README.md

[^112]: https://greptime.com/blogs/2025-03-03-greptimedb-v0.12-release

[^113]: https://github.com/GreptimeTeam/docs/blob/main/versioned_docs/version-0.10/user-guide/administration/performance-tuning-tips.md

[^114]: https://raw.githubusercontent.com/GreptimeTeam/greptimedb/develop/config/standalone.example.toml

[^115]: https://xie.infoq.cn/article/6e3489ab8d4c1fb9adf416b1a

[^116]: https://www.modb.pro/db/1896378309794082816

[^117]: https://greptimedb.rs/mito2/index.html

[^118]: https://newreleases.io/project/github/GreptimeTeam/greptimedb/release/v0.11.2

[^119]: https://github.com/GreptimeTeam/greptimedb/blob/v0.7.0/src/object-store/src/layers/lru_cache.rs

[^120]: https://newreleases.io/project/github/GreptimeTeam/greptimedb/release/v0.8.2

[^121]: https://github.com/GreptimeTeam/greptimedb/releases

[^122]: https://docs.greptime.com/v0.6/greptimecloud/getting-started/vector/

[^123]: https://newreleases.io/project/github/GreptimeTeam/greptimedb/release/v0.10.1

[^124]: https://github.com/GreptimeTeam/greptimedb/releases/tag/v0.9.0

[^125]: https://docs.greptime.com/greptimecloud/getting-started/vector/

[^126]: https://www.producthunt.com/p/greptimedb/greptimedb

[^127]: https://vector.dev/docs/reference/configuration/sinks/greptimedb_logs/

[^128]: https://dev.to/greptime/what-to-expect-next-greptimedb-roadmap-for-2024-33p7

[^129]: https://cloud.google.com/blog/products/databases/memorystore-for-redis-vector-search-and-langchain-integration

[^130]: https://opster.com/guides/elasticsearch/glossary/elasticsearch-cache/

[^131]: https://tanstack.com/query/v4/docs/reference/QueryCache

[^132]: https://lucene.apache.org/core/8_0_0/core/org/apache/lucene/search/UsageTrackingQueryCachingPolicy.html

[^133]: https://github.com/elastic/elasticsearch/issues/16259

[^134]: https://discuss.elastic.co/t/per-type-query-cache-invalidation/42154

[^135]: https://docs.influxdata.com/influxdb/v2/reference/internals/storage-engine/

[^136]: https://www.reddit.com/r/influxdb/comments/13qz4jd/understanding_performance_of_query_cache_in_v18/

[^137]: https://github.com/influxdata/influxdb/issues/7712

[^138]: https://docs.influxdata.com/enterprise_influxdb/v1/flux/get-started/query-influxdb/

[^139]: https://materialize.com/blog/redis-cache-invalidation/

[^140]: https://www.influxdata.com/blog/-influxdb3-last-value-cache/

[^141]: https://docs.greptime.com/0.9/user-guide/administration/performance-tuning-tips/

[^142]: https://github.com/GreptimeTeam/greptimedb-operator/blob/main/docs/api-references/docs.md

[^143]: https://www.linkedin.com/pulse/greptimedb-takes-billion-json-document-challenge-outperforms-335qc

[^144]: https://github.com/GreptimeTeam/docs/blob/main/versioned_docs/version-0.12/user-guide/administration/upgrade.md

[^145]: https://quickwit.io/docs

[^146]: https://www.linkedin.com/posts/dennis-zhuang_database-architecture-engineering-activity-7308374548855341056-QZ6a

[^147]: https://github.com/influxdata/influxdb/issues/25906

[^148]: https://docs.influxdata.com/influxdb/v2/query-data/execute-queries/influx-api/

[^149]: https://community.influxdata.com/t/how-managing-cache-snapshot-memory-size-with-very-different-databases/12294

[^150]: https://github.com/influxdata/influxdb/issues/10052

[^151]: https://docs.influxdata.com/influxdb/v1/concepts/storage_engine/

[^152]: https://docs.influxdata.com/influxdb/v2/install/upgrade/v1-to-v2/migrate-cqs/

[^153]: https://docs.influxdata.com/influxdb3/core/

[^154]: https://www.scribd.com/document/815266776/InfluxDB-3-0-vs-OSS-Benchmarks

[^155]: https://www.influxdata.com/products/influxdb/

[^156]: https://www.influxdata.com/blog/the-plan-for-influxdb-3-0-open-source/

[^157]: http://greptimedb.rs/pipeline/index.html

[^158]: https://github.com/GreptimeTeam/greptimedb/blob/main/docs/style-guide.md

[^159]: https://github.com/GreptimeTeam/greptimedb/blob/main/Cargo.lock

[^160]: https://tagpacker.com/api/users/5d1c6d1b64ce750934733a23/links?format=html\&filter={"userId"%3A"5d1c6d1b64ce750934733a23"%2C"tagIds"%3A["5d1d14f664ce750934736538"%2C"602844a76a647d31119e8715"]}

[^161]: https://www.youtube.com/watch?v=3gcKm3yoJK8

[^162]: https://docs.rs/crate/quickwit-search/latest

[^163]: https://pkg.go.dev/github.com/samber/go-quickwit

[^164]: https://github.com/quickwit-oss/quickwit/releases

[^165]: https://www.youtube.com/watch?v=SME-dvQIFxk

[^166]: https://docs.greptime.com/nightly/user-guide/deployments/configuration

[^167]: https://greptime.com/blogs/2023-11-29-biweekly-report

[^168]: https://github.com/GreptimeTeam/greptimedb/issues/2510

[^169]: https://github.com/GreptimeTeam/greptimedb/issues/4650

[^170]: https://docs.greptime.com/release-notes/

[^171]: https://www.bookstack.cn/read/greptimedb-0.9-en/b7c601882bde080f.md

[^172]: https://github.com/GreptimeTeam/greptimedb/blob/main/README.md

[^173]: https://github.com/GreptimeTeam/greptimedb

[^174]: https://github.com/shivendrasoni/vector-cache

[^175]: https://docs.greptime.com/user-guide/administration/upgrade/

[^176]: https://dev.to/greptime/greptimedb-quickstart-guide-seamlessly-launch-our-time-series-database-from-the-ground-up-1dlp

[^177]: https://vector.dev/docs/reference/configuration/sinks/greptimedb_metrics/

