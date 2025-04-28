# How Elasticsearch Executes Queries with Filter Context


`flowchart TD`
    `A[Query arrives] --> B{Filter cached?}`
    `B -- Yes --> C[Fetch Roaring Bitmap]`
    `B -- No --> D[Evaluate Filter]`
    `D --> E[Create BitSet]`
    `E --> F[Compress to Roaring Bitmap]`
    `F --> G[Store in Cache]`
    `G --> C`
    `C --> H[Find Matching Docs]`


 the query execution flow  in Elasticsearch. Let me clarify how it actually works:

## Actual Query Execution Flow

For a query like:
```json
GET /products/_search
{
  "query": {
    "bool": {
      "must": {
        "match": { "description": "bluetooth earbuds" }
      },
      "filter": [
        { "term": { "in_stock": true } },
        { "term": { "brand": "sony" } }
      ]
    }
  }
}
```

Elasticsearch executes this in the following order:

1. **First, execute all filter clauses** (`in_stock: true` and `brand: sony`)
   - These are executed using the filter context (not scored)
   - They produce binary yes/no decisions for each document
   - The result is a bitset of matching document IDs

2. **Then, execute the query clauses** (the `match` for "bluetooth earbuds")
   - This runs only on the documents that passed the filters
   - These use query context (scored for relevance)

3. **Combine the results** to return only documents that satisfy both the filters and the query

## Why This Order Matters

This execution order is more efficient because:
- Filters are usually faster to execute than full-text searches
- Filters can use the node query cache
- By applying filters first, full-text search runs on a smaller set of documents

## How Query Cache Works in This Example

Let's break down the caching for our example:

1. When the query runs on a node:
   - Elasticsearch checks if `{ "term": { "in_stock": true } }` is in the node query cache
   - If not cached, it executes the filter and caches the resulting bitset
   - Same for `{ "term": { "brand": "sony" } }`

2. For subsequent queries:
   - If another query uses `{ "term": { "in_stock": true } }` on any shard on the same node, even with different query terms, the cached bitset is reused
   - This happens regardless of what the main query is searching for ("wireless headphones" vs "bluetooth earbuds")

3. This caching is per-node, not per-shard:
   - All shards on the node share the same cache
   - The cache entries are keyed by the exact filter definition

## Example of Cache Benefit

Consider this sequence of events on a single node with multiple shards:

1. User 1 searches for "bluetooth earbuds" with filters `in_stock: true` and `brand: sony`
   - Both filters execute and cache their results
   - Full-text search runs on filtered documents
   
2. User 2 searches for "noise canceling" with filter `in_stock: true`
   - The `in_stock: true` filter reuses the cached bitset
   - Only the new search term "noise canceling" needs full computation
   
3. User 3 searches for "headphones" with filters `brand: sony` and `price <= 200`
   - The `brand: sony` filter reuses the cached bitset
   - The `price <= 200` filter is new and gets computed and cached
   - The search for "headphones" runs on the filtered results

## Key Points to Remember

1. Filters are applied before full-text queries for efficiency
2. Each unique filter is cached independently in the node query cache
3. The cache is shared by all shards on a node
4. Filter caching happens automatically for filter context queries
5. The node query cache works with the LRU (Least Recently Used) eviction policy
6. Cache entries are invalidated when data changes

This execution model is why filter context is so important for performance in Elasticsearch, especially for frequently used filters on large datasets.​​​​​​​​​​​​​​​​