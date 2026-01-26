---
tags:
  - graph
  - dfs
  - connected-components
  - adjacency-matrix
---

# LC 547: Number of Provinces (Graph DFS Pattern)

## Pattern
Connected components counting via DFS traversal on adjacency matrix.

## Solution
```python
def findCircleNum(self, isConnected: List[List[int]]) -> int:
    num_cities = len(isConnected)
    visited = set()
    provinces = 0

    def dfs_traversal(source_city):
        if source_city not in visited:
            visited.add(source_city)

        for nbr_city in range(num_cities):
            if isConnected[source_city][nbr_city] == 1 and nbr_city not in visited:
                dfs_traversal(nbr_city)

    for city in range(num_cities):
        if city not in visited:
            dfs_traversal(city)
            provinces += 1
    return provinces
```

## Key Points
- **Adjacency Matrix**: Graph as 2D array (dense representation)
- **DFS Logic**: Visit city → explore all connected unvisited neighbors
- **Component Count**: Increment on fresh DFS (new component found)
- **Visited Tracking**: Set to avoid cycles and redundant traversals

## Complexity
- **Time**: O(n²) — check all edges in adjacency matrix
- **Space**: O(n) — visited set + recursion stack

## Why This Works
Each DFS finds one complete connected component (province). When outer loop can't find unvisited city in that component, count increments. All n cities visited exactly once across all DFS calls.

## Alternative
Union-Find approach: Same logic, different structure (DSU better for dynamic graph updates)
