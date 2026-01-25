---
tags:
  - leetcode
  - graph
  - dfs
  - logic
  - implementation
---

# LC 1971: Find if Path Exists in Graph

## Mistake #1: DFS Not Called
Forgot to invoke `dfs_traversal(source)` before checking visited dict.

### What Went Wrong
```python
❌ # Forgot to call dfs_traversal
def dfs_traversal(src):
    visited[src] = True
    for nbr in graph[src]:
        dfs_traversal(nbr)

# Never called! visited stays empty
if visited[source] and visited[destination]:
    return True
```

### Why
Defining a function doesn't execute it. The traversal never happens → visited dict stays empty → KeyError or wrong result. **Action requires invocation, not just definition.**

### Key Lesson
After defining a helper function, verify it's called at the right point in control flow. Check: "Did I actually invoke this function?"

---

## Mistake #2: Missing Visited Check in Loop
Recursed into already-visited neighbors, causing infinite loops.

### What Went Wrong
```python
❌ # No guard against re-visiting
def dfs_traversal(src):
    visited[src] = True
    for nbr in graph[src]:
        dfs_traversal(nbr)  # No check if nbr already visited
```

### Why
In an undirected graph, node A connects to B, and B connects back to A. Without `if nbr not in visited`, you revisit A infinitely: A→B→A→B→...

### Fix
```python
✅ def dfs_traversal(src):
    visited[src] = True
    for nbr in graph[src]:
        if nbr not in visited:
            dfs_traversal(nbr)
```

### Key Lesson
**Always guard recursive calls in graph problems.** Check `if neighbor not in visited` before recursing. This is non-negotiable in undirected graphs.

---

## Mistake #3: Unsafe Dictionary Access
Used `visited[destination]` which raises KeyError if destination was never visited.

### What Went Wrong
```python
❌ if visited[destination]:  # KeyError if destination unreachable
    return True
```

### Why
Direct dictionary indexing assumes the key exists. If source can't reach destination, destination never gets added to visited dict → KeyError on access.

### Fix
```python
✅ if destination in visited:
    return True
```

### Key Lesson
**Use `key in dict` before `dict[key]`.** Or use `dict.get(key, default)`. In graph problems especially, nodes may be unreachable → always guard access.

---

## Summary: Path to Correct Solution

| Iteration | Issue | Root Cause | Fix |
|-----------|-------|-----------|-----|
| 1 | No traversal happens | Function defined but not called | Call `dfs_traversal(source)` |
| 2 | Infinite loop on cycles | Revisit unguarded | Add `if nbr not in visited:` |
| 3 | KeyError on unreachable | Unsafe dict access | Use `in` operator |

### Final Working Solution
```python
✅ def validPath(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
    graph = defaultdict(list)

    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)

    visited = {}

    def dfs_traversal(src):
        visited[src] = True
        for nbr in graph[src]:
            if nbr not in visited:
                dfs_traversal(nbr)

    dfs_traversal(source)
    return destination in visited
```

---

## Transferable Patterns

1. **Function invocation:** Define → Call (don't confuse declaration with execution)
2. **Cycle prevention:** Always `if not visited[nbr]` before graph recursion
3. **Safe access:** Prefer `in` operator over direct indexing for lookups

