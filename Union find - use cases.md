---
tags:
  - leetcode
---

## 1. **Network Connectivity**

**Problem**: Determine if two computers in a network can communicate (directly or indirectly)

**Applications**:
- Social networks (are two people connected through mutual friends?)
- Internet routing (can packets reach from A to B?)
- Telephone networks

**Example LeetCode**: 
- #323 Number of Connected Components in an Undirected Graph
- #1319 Number of Operations to Make Network Connected

## 2. **Kruskal's Minimum Spanning Tree Algorithm**

**Problem**: Find the minimum cost to connect all nodes in a weighted graph

**How Union-Find helps**:
```
1. Sort edges by weight (smallest first)
2. For each edge (u, v):
   - If find(u) != find(v): // Not already connected
     - Add edge to MST
     - union(u, v)
   - Else: skip (would create cycle)
```

**Real-world uses**:
- Designing road networks (minimize construction cost)
- Computer network design
- Circuit design on chips

**LeetCode**: #1584 Min Cost to Connect All Points

## 3. **Detecting Cycles in Undirected Graphs**

**Problem**: Does adding an edge create a cycle?

**Logic**: If you try to union two nodes that already have the same root parent, adding that edge creates a cycle!

```python
def hasCycle(edges, n):
    parent = list(range(n))
    
    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]
    
    for u, v in edges:
        rootU, rootV = find(u), find(v)
        if rootU == rootV:  # Already in same set!
            return True  # Adding this edge creates a cycle
        parent[rootU] = rootV
    
    return False
```

**LeetCode**: #684 Redundant Connection

## 4. **Image Processing - Connected Components**

**Problem**: Find regions of connected pixels (same color)

**Use case**:
- Flood fill in paint programs
- Object detection in images
- Finding islands in a grid

**Example**: Count number of distinct "blobs" in an image
```
1 1 0 0 0
1 1 0 1 1
0 0 0 1 1
```
Answer: 2 components (top-left blob and right blob)

**LeetCode**: #200 Number of Islands, #695 Max Area of Island

## 5. **Accounts Merge**

**Problem**: Merge user accounts that share common emails

**Example**:
```
Input:
Account 1: John (john@mail.com, john@gmail.com)
Account 2: John (john@yahoo.com)
Account 3: Mary (mary@mail.com)
Account 4: John (john@gmail.com, john@yahoo.com)

Output:
John: {john@mail.com, john@gmail.com, john@yahoo.com}
Mary: {mary@mail.com}
```

**Approach**: Use Union-Find where each email is a node. If two emails belong to same account, union them!

**LeetCode**: #721 Accounts Merge

## 6. **Earliest Friends / Earliest Connection Time**

**Problem**: Given friendships added over time, when did two people become connected?

**Example**:
```
Time 1: Connect A-B
Time 2: Connect B-C
Time 3: Connect D-E
Time 4: Connect C-D

Query: When did A and E become friends?
Answer: Time 4 (through chain A-B-C-D-E)
```

**LeetCode**: #1101 The Earliest Moment When Everyone Become Friends

## 7. **Satisfiability Problems**

**Problem**: Check if equations are satisfiable

**Example**:
```
Equations: a == b, b == c, c != a
Can this be satisfied? NO! (a == c by transitivity)
```

**Approach**:
- Union variables that must be equal
- Check if any "not equal" constraint conflicts

**LeetCode**: #990 Satisfiability of Equality Equations

## 8. **Percolation Theory**

**Problem**: In a grid, if you randomly block cells, when does liquid stop flowing from top to bottom?

**Applications**:
- Material science (when does a porous material become impermeable?)
- Disease spread modeling
- Forest fire propagation

**Approach**: Use Union-Find to track connected open cells from top to bottom

## 9. **Similar String Groups**

**Problem**: Group strings where each pair differs by at most 2 character swaps

**Example**:
```
"tars", "rats", "arts", "star"
All can be grouped together through swaps
```

**LeetCode**: #839 Similar String Groups

## 10. **Redundant Connections in Graphs**

**Problem**: Remove one edge to make a tree (no cycles)

**Applications**:
- Network optimization
- Removing redundant links in infrastructure

**LeetCode**: #684 Redundant Connection, #685 Redundant Connection II

## 11. **Longest Consecutive Sequence**

**Problem**: Find longest sequence of consecutive numbers

**Example**: `[100, 4, 200, 1, 3, 2]` → `[1,2,3,4]` (length 4)

**Union-Find approach**: Union consecutive numbers, then find largest component

**LeetCode**: #128 Longest Consecutive Sequence

## 12. **Bricks Falling When Hit**

**Problem**: When bricks are hit, which other bricks fall (not connected to ceiling)?

**Approach**: Use Union-Find in reverse! Start from final state and add bricks back.

**LeetCode**: #803 Bricks Falling When Hit

## When to Use Union-Find?

Use Union-Find when you need:
- ✅ Group/cluster elements dynamically
- ✅ Check if two elements are in same group (quickly!)
- ✅ Merge groups efficiently
- ✅ Count number of distinct groups
- ✅ Detect cycles in undirected graphs
- ✅ Handle "equivalence relations" (transitive connections)

**Don't use** when you need:
- ❌ Shortest paths (use BFS/Dijkstra)
- ❌ Actual path between nodes (use DFS/BFS)
- ❌ Directed graph strongly connected components (use Tarjan's/Kosaraju's)

Union-Find is one of those elegant data structures that appears everywhere once you know to look for it! Which application interests you most?