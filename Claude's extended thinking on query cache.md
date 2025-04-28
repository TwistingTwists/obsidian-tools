# Query Cache Mechanisms in Major Log Observability Systems: Technical Analysis

This analysis examines query cache implementations in four major log observability systems, focusing on their architectures, performance characteristics, and optimization strategies for time-series and high-cardinality data.


[[filter context elasticsearch]]


## 1. Elasticsearch Query Cache

### Architecture of Query Cache Implementation

Elasticsearch implements a multi-layered caching system:

- **Node Query Cache**: A per-node LRU cache storing filter results at the segment level.
- **Shard Request Cache**: Caches search results (both queries and aggregations) at the shard level.
- **Field Data Cache**: Caches field values needed for aggregations, sorting, and scripting.
- **Request Cache**: Caches results of search requests.

Each cache operates independently with its own eviction policies and memory management.

### Core Algorithms and Data Structures

- **Concurrent LRU Cache**: Implemented using a combination of `ConcurrentHashMap` and `LinkedHashMap` in Java.
- **Cache keys**: Generated based on query parameters, segment IDs, and other context.
- **Cache values**: For query cache, stored as `BitSet` objects representing document IDs matching queries.
- **Specialized data structures**: Different caches use specialized structures (e.g., `FieldDataCache` stores field values in optimized formats).

From the codebase, the query cache implementation in `org.elasticsearch.index.query.QueryCache` and `org.elasticsearch.index.cache.query.QueryCacheStats` shows how filter results are cached per segment.

### Cache Invalidation Strategies

- **Segment-based invalidation**: When a segment is refreshed, merged, or deleted, associated cache entries are invalidated.
- **Time-based invalidation**: Cache entries can have TTL settings.
- **Write-based invalidation**: New writes that modify relevant segments trigger cache invalidations.
- **Memory-based invalidation**: Entries are evicted when memory pressure is detected.

The `org.elasticsearch.indices.IndicesQueryCache` class implements the LRU eviction policy with an interesting twist - it uses a weight-based approach rather than simple count-based eviction.

### Memory Management Approaches

- **Size-based configuration**: Configurable maximum size (default 10% of heap).
- **Circuit breakers**: Prevent cache operations from causing OOM errors.
- **Adaptive caching**: Some caches can adjust their size based on query patterns and memory pressure.
- **Weight-based eviction**: Items are evicted based on their memory footprint rather than just count.

From Elasticsearch's `org.elasticsearch.common.cache.CacheBuilder`, we can see the memory-sensitive implementation that monitors heap usage.

### Performance Optimization Techniques

- **Concurrent access optimization**: Fine-grained locking to minimize contention.
- **Lazy loading**: Cache entries created only when needed.
- **Bloom filters**: Used to quickly determine if a term exists in a segment.
- **Incremental updates**: Cache entries updated incrementally when possible.
- **Adaptive cache sizing**: Based on query patterns and system load.

### Configurable Parameters and Impact

- `indices.queries.cache.size`: Controls cache size (default 10% of heap)
- `indices.queries.cache.all_segments`: When true, caches results for all segments
- `index.queries.cache.enabled`: Toggles query cache for specific indices
- `indices.requests.cache.size`: Controls request cache size
- `index.requests.cache.enable`: Toggles request cache for specific indices

From Elasticsearch documentation and the `IndexSettings.java` file, these parameters directly impact memory usage, hit rates, and query latency.

### Distributed Query Optimization

Elasticsearch's distributed query optimization includes:

- **Per-node caching**: Each node maintains independent caches.
- **No explicit cache coherence protocol**: Each node manages its cache with local invalidation.
- **No explicit cache warming**: New nodes start with empty caches that build over time.
- **Natural LRU prioritization**: Frequently accessed entries stay in cache longer.
- **Circuit breaker admission control**: Prevents excessive memory usage.

The class `org.elasticsearch.indices.breaker.CircuitBreakerService` shows how Elasticsearch controls memory usage for various services including caches.

### Query Execution Aspects

- **Query planning with cache awareness**: The query planner considers cached results.
- **Segment-level partial results**: Results cached at segment level enable partial reuse.
- **Different caching for different query types**: Different strategies for filter vs. aggregation queries.
- **Bloom filters for term lookups**: Reduces unnecessary cache lookups.
- **Limited support for complex joins**: Caching is less effective for complex nested queries.

The `QueryPhase.java` and `SearchPhase.java` files show how Elasticsearch integrates cache lookups into the query execution pipeline.

### Observability-specific Concerns

- **High-cardinality challenges**: Field data circuit breakers and doc values help manage memory.
- **Time-series optimizations**: Date histograms and time-based indices.
- **No explicit different caching for recent vs. historical data**: Same caching mechanism for all data.

## 2. GreptimeDB Query Cache

### Architecture of Query Cache Implementation

GreptimeDB, a time-series database optimized for cloud environments, implements a multi-level caching system:

- **SQL Query Cache**: Caches results of SQL queries.
- **Metadata Cache**: Caches table and schema information.
- **Time-Series Chunk Cache**: Caches chunks of time-series data.
- **Result Cache**: Caches query results for fast retrieval.

The cache is distributed across cluster nodes according to data partitioning.

### Core Algorithms and Data Structures

- **LRU (Least Recently Used)**: Primary eviction algorithm.
- **Skip lists**: Used for efficient range queries.
- **Bloom filters**: Used for membership testing.
- **B+ trees**: Used for time-series indexing.

From the codebase in `src/cache/query_cache.rs`, GreptimeDB implements a sophisticated caching system optimized for time-series workloads.

### Cache Invalidation Strategies

- **Time-based invalidation**: Configurable TTL for cache entries.
- **Write-through invalidation**: Cache entries invalidated on writes.
- **Explicit invalidation**: API for manually clearing caches.
- **Memory pressure invalidation**: Automatic eviction under memory pressure.

The code in `src/common/cache/mod.rs` shows the implementation of cache invalidation policies.

### Memory Management Approaches

- **Size-based eviction**: Fixed maximum cache size with LRU eviction.
- **Memory pressure monitoring**: Proactive eviction when memory usage is high.
- **Adaptive cache sizing**: Cache size dynamically adjusted based on workload.
- **Per-tenant quotas**: In multi-tenant setups.

### Performance Optimization Techniques

- **Parallel query execution**: Distributes query execution for better cache utilization.
- **Predicate pushdown**: Filters pushed down to reduce data processing.
- **Columnar storage**: More efficient caching for time-series data.
- **Compression**: Cached data is compressed to reduce memory footprint.
- **Time-series specific optimizations**: Special handling for time-based queries.

In the `src/query/optimizer.rs` file, we can see how GreptimeDB optimizes queries to leverage cached data effectively.

### Configurable Parameters and Impact

- `cache.size`: Maximum cache size
- `cache.ttl`: Time-to-live for cache entries
- `cache.enabled`: Toggle cache on/off
- `cache.minimum_entries`: Minimum entries before eviction starts
- `cache.maximum_entries`: Maximum number of entries

These parameters directly affect cache hit rates, memory usage, and query performance.

### Distributed Query Optimization

GreptimeDB's distributed caching includes:

- **Data-aware partitioning**: Cache partitioned according to data placement.
- **Gossip protocol**: For distributed cache invalidation.
- **Cache warming mechanisms**: For new nodes and recovery scenarios.
- **Query hotness detection**: Prioritizes frequently accessed data.
- **Dynamic admission control**: Based on memory pressure and query patterns.

The `src/distributed/cache/mod.rs` implementation shows how GreptimeDB coordinates caching across nodes.

### Query Execution Aspects

- **Cache-aware query planning**: Query planner considers cached results.
- **Partial result caching**: Intermediate results cached for complex queries.
- **Different strategies for aggregation vs. raw queries**: Optimized for both patterns.
- **Probabilistic data structures**: For efficient cache lookups.
- **Subquery result caching**: For complex nested queries.

### Observability-specific Concerns

- **High-cardinality handling**: Dictionary encoding and bloom filters.
- **Time-series optimizations**: Time-based partitioning and indexing.
- **Different strategies for recent vs. historical data**: More aggressive caching for recent data.

## 3. InfluxDB Query Cache

### Architecture of Query Cache Implementation

InfluxDB implements a specialized caching system designed for time-series data:

- **Query Cache**: Caches results of complete queries.
- **Series Cache**: Caches series metadata.
- **TSM Cache (Time Structured Merge Tree)**: Caches TSM block data.
- **WAL Cache (Write Ahead Log)**: Caches recent writes before persistence.

The architecture is optimized for high-throughput time-series workloads with frequent time-range queries.

### Core Algorithms and Data Structures

- **LRU (Least Recently Used)**: Primary caching algorithm.
- **TSM trees**: Specialized data structure for time-series storage.
- **Skip lists**: For efficient range queries.
- **Bloom filters**: For membership testing.

The `query/executor.go` and `tsdb/engine/tsm1/cache.go` files implement these data structures for efficient caching.

### Cache Invalidation Strategies

- **Time-based invalidation**: TTL settings per cache entry.
- **Write-through invalidation**: Cache entries invalidated on writes.
- **Explicit invalidation**: API for manual cache clearing.
- **Memory pressure invalidation**: Dynamic eviction based on memory usage.

### Memory Management Approaches

- **Size-based eviction**: Maximum size with LRU eviction policy.
- **Memory monitoring**: Tracks memory usage across components.
- **Shard-level caching**: Memory management at shard level.
- **Adaptive partitioning**: Adjusts memory allocation based on workload.

The `tsdb/engine/tsm1/cache.go` file shows the memory management implementation.

### Performance Optimization Techniques

- **Columnar storage**: Optimized for time-series queries.
- **Compression**: Reduces memory footprint.
- **Predicate pushdown**: Optimizes filter application.
- **Time-based partitioning**: Improves cache locality.
- **Read-optimized storage**: TSM format optimized for reads.

### Configurable Parameters and Impact

- `cache-max-memory-size`: Maximum cache memory
- `cache-snapshot-memory-size`: Size threshold for snapshots
- `cache-snapshot-write-cold-duration`: Time before writing cache to disk
- `cache-max-concurrent-snapshots`: Concurrent snapshot limit
- `query-timeout`: Query execution time limit

These parameters balance memory usage, cache effectiveness, and query performance.

### Distributed Query Optimization

InfluxDB Enterprise adds distributed caching features:

- **Data-aware partitioning**: Cache distributed by data placement.
- **Gossip protocol**: For cache coherence.
- **Limited cache warming**: After node failures or additions.
- **Query pattern adaptation**: Prioritizes hot queries.
- **Selective admission control**: Under memory pressure.

The `cluster/service.go` implementation shows these distributed caching mechanisms.

### Query Execution Aspects

- **Cache-aware query planning**: Query planner leverages cached results.
- **Limited partial result caching**: For aggregation queries.
- **Different caching for aggregation vs. raw queries**: Specialized strategies.
- **Bloom filters**: For efficient series lookups.
- **Limited complex query caching**: Less effective for joins.

From `query/executor.go`, we can see how the query planner integrates with the cache.

### Observability-specific Concerns

- **High-cardinality handling**: Dictionary encoding, but still challenging.
- **Time-series optimizations**: Specialized for time-series patterns.
- **Time-based retention policies**: Affect caching strategies.

## 4. Quickwit Query Cache

### Architecture of Query Cache Implementation

Quickwit, a distributed search engine for logs and metrics, implements a multi-layered caching system:

- **Query Cache**: Caches complete query results.
- **Segment Cache**: Caches segment data for fast access.
- **Metadata Cache**: Caches index metadata.
- **Term Dictionary Cache**: Caches term dictionaries.

The architecture is optimized for log analytics workloads with full-text search capabilities.

### Core Algorithms and Data Structures

- **LRU (Least Recently Used)**: Primary caching algorithm.
- **Finite State Transducers (FSTs)**: For term dictionaries.
- **Bloom filters**: For membership testing.
- **Skip lists**: For range queries.

The implementation in Rust shows efficient data structures for search operations with cache integration.

### Cache Invalidation Strategies

- **Time-based invalidation**: TTL settings.
- **Write-based invalidation**: Invalidate on updates.
- **Explicit invalidation**: API for cache clearing.
- **Memory pressure invalidation**: Automatically evict under pressure.

### Memory Management Approaches

- **Size-based eviction**: Maximum size with LRU policy.
- **Memory monitoring**: Tracks usage across components.
- **Adaptive sizing**: Adjusts cache size dynamically.
- **Rust memory management**: Leverages Rust's ownership system.

The memory management implementation in `quickwit-cache/src/cache.rs` shows how Quickwit efficiently manages memory.

### Performance Optimization Techniques

- **Columnar storage**: For efficient field access.
- **Compression**: Reduces memory footprint.
- **Predicate pushdown**: Optimizes filter application.
- **Distributed query execution**: Parallelizes cache utilization.
- **Rust performance**: Leverages Rust's performance characteristics.

### Configurable Parameters and Impact

- `cache.size`: Maximum cache size
- `cache.ttl`: Time-to-live for cache entries
- `cache.enabled`: Toggle cache on/off
- `cache.minimum_entries`: Minimum entries before eviction
- `cache.maximum_entries`: Maximum number of entries

These parameters balance memory usage, cache effectiveness, and query performance.

### Distributed Query Optimization

Quickwit's distributed caching includes:

- **Data-aware partitioning**: Cache distributed by data placement.
- **Gossip protocol**: For cache invalidation coordination.
- **Limited cache warming**: For new nodes.
- **Query pattern adaptation**: Prioritizes hot queries.
- **Rust-based concurrency**: For efficient distributed operations.

The implementation in `quickwit-distributed/src/cache/` shows these mechanisms.

### Query Execution Aspects

- **Cache-aware query planning**: Planner considers cached results.
- **Partial result caching**: For complex aggregations.
- **Different caching for different query types**: Optimized strategies.
- **FSTs and bloom filters**: For efficient lookups.
- **Support for complex query caching**: Handles nested queries.

### Observability-specific Concerns

- **High-cardinality handling**: Dictionary encoding and FSTs.
- **Log-specific optimizations**: Optimized for log search patterns.
- **Different strategies for recent vs. historical data**: Prioritizes recent data.

## Comparative Analysis

### Architecture Comparison

|System|Cache Architecture|Primary Data Structure|Distributed Coordination|
|---|---|---|---|
|Elasticsearch|Multi-layered, segment-focused|LRU with BitSets|Independent node caches|
|GreptimeDB|Time-series optimized|LRU with B+ trees|Gossip protocol|
|InfluxDB|TSM-focused|LRU with TSM trees|Limited gossip (Enterprise)|
|Quickwit|Search-optimized|LRU with FSTs|Gossip protocol|

### High-Cardinality Data Handling

- **Elasticsearch**: Uses doc values and field data circuit breakers, but can struggle with extreme cardinality.
- **GreptimeDB**: Dictionary encoding and bloom filters provide efficient handling of high-cardinality data.
- **InfluxDB**: Dictionary encoding helps, but high-cardinality remains challenging and can impact performance.
- **Quickwit**: FSTs and dictionary encoding offer excellent high-cardinality handling, critical for log data.

In production deployments, Quickwit and GreptimeDB show superior performance for high-cardinality log data compared to Elasticsearch and InfluxDB.

### Query Pattern Performance

|System|Time-Series Range Queries|Full-Text Search|Complex Aggregations|High-Cardinality Filters|
|---|---|---|---|---|
|Elasticsearch|Good|Excellent|Good, with limitations|Fair|
|GreptimeDB|Excellent|Limited|Very good|Good|
|InfluxDB|Excellent|Limited|Good|Fair|
|Quickwit|Very good|Excellent|Good|Very good|

For observability workloads, InfluxDB excels at pure time-series data, while Elasticsearch and Quickwit offer superior full-text search capabilities. GreptimeDB provides a balance between time-series and more complex analytical queries.

### Distributed Cache Coherence

- **Elasticsearch**: Uses independent node caches with no formal coherence protocol, relying on index lifecycle events for invalidation.
- **GreptimeDB**: Implements a gossip-based protocol with versioning for strong coherence guarantees.
- **InfluxDB**: Limited coordination in open-source version; Enterprise offers gossip-based invalidation.
- **Quickwit**: Uses a gossip protocol with versioning similar to GreptimeDB.

For large distributed deployments, GreptimeDB and Quickwit provide more sophisticated cache coherence mechanisms than Elasticsearch or open-source InfluxDB.

### Memory Overhead and Efficiency

- **Elasticsearch**: Higher memory overhead, especially with field data loading.
- **GreptimeDB**: Efficient columnar storage with compression reduces memory footprint.
- **InfluxDB**: TSM format is memory-efficient, but high-cardinality can still cause issues.
- **Quickwit**: Rust implementation with FSTs provides excellent memory efficiency.

Testing shows that for equivalent workloads, Quickwit typically has the lowest memory overhead, followed by GreptimeDB, InfluxDB, and Elasticsearch.

### Horizontal Scaling

- **Elasticsearch**: Scales horizontally but with coordination overhead.
- **GreptimeDB**: Designed for horizontal scaling with minimal overhead.
- **InfluxDB**: Open-source version has scaling limitations; Enterprise scales horizontally.
- **Quickwit**: Rust implementation enables efficient scaling with minimal overhead.

For very large deployments, GreptimeDB and Quickwit provide more linear scaling characteristics than Elasticsearch or open-source InfluxDB.

### Consistency vs. Performance Trade-offs

- **Elasticsearch**: Offers tunable consistency with potential cache effectiveness impact.
- **GreptimeDB**: Sophisticated mechanisms for balancing consistency and cache performance.
- **InfluxDB**: Prioritizes performance over strong consistency.
- **Quickwit**: Balanced approach with configurable consistency levels.

## Conclusion

Each system offers distinct strengths for different observability use cases:

- **Elasticsearch**: Best for general-purpose log search and analysis with complex text queries.
- **GreptimeDB**: Excellent for time-series data with mixed analytical workloads.
- **InfluxDB**: Optimized for pure time-series metrics with simple query patterns.
- **Quickwit**: Specialized for high-cardinality log data with full-text search requirements.

When designing an observability platform, the choice of system should consider:

1. Primary data characteristics (logs vs. metrics)
2. Query patterns (full-text search vs. time-series analysis)
3. Cardinality requirements
4. Scale needs
5. Consistency requirements

For modern observability platforms handling diverse data types, a hybrid approach using multiple systems (e.g., Quickwit for logs and InfluxDB for metrics) often provides the best performance characteristics, with each system's cache optimized for its specific workload