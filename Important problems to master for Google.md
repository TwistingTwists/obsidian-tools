Claude gave  this response. 


# Google Interview - Essential Graph Patterns

## Must-Know Patterns (Practice These First!)

| Pattern | Key Concept | Time | LeetCode Must-Do | Why Google Asks This |
|---------|-------------|------|------------------|---------------------|
| **BFS - Shortest Path** | Queue, level-order | O(V+E) | 🔥 **1091** (Shortest Path Binary Matrix)<br>🔥 **994** (Rotting Oranges)<br>**127** (Word Ladder) | Tests understanding of optimal pathfinding |
| **DFS - Graph Traversal** | Recursion, visited set | O(V+E) | 🔥 **200** (Number of Islands)<br>🔥 **133** (Clone Graph)<br>**695** (Max Area of Island) | Foundation for all graph problems |
| **Topological Sort** | In-degree, DAG detection | O(V+E) | 🔥 **207** (Course Schedule)<br>🔥 **210** (Course Schedule II)<br>**269** (Alien Dictionary) | Dependency resolution, build systems |
| **Union-Find** | Path compression, union by rank | O(E·α(V)) | 🔥 **323** (Number of Connected Components)<br>🔥 **684** (Redundant Connection)<br>**547** (Number of Provinces) | Network connectivity, clustering |
| **Grid DFS/BFS** | 4-directional traversal | O(M×N) | 🔥 **200** (Number of Islands)<br>🔥 **130** (Surrounded Regions)<br>**417** (Pacific Atlantic Water Flow) | Real-world map/image problems |
| **Dijkstra's Algorithm** | Min-heap, greedy | O((V+E)log V) | 🔥 **743** (Network Delay Time)<br>**787** (Cheapest Flights K Stops)<br>**1631** (Path With Minimum Effort) | Network routing, maps optimization |
| **Backtracking on Grid** | DFS with restore | O(M×N×4^L) | 🔥 **79** (Word Search)<br>**212** (Word Search II)<br>**980** (Unique Paths III) | Search space exploration |
| **Cycle Detection** | DFS with colors (directed)<br>DFS with parent (undirected) | O(V+E) | 🔥 **207** (Course Schedule)<br>**684** (Redundant Connection)<br>**261** (Graph Valid Tree) | Deadlock detection, validation |

## High-Frequency Google Problems (Last 6 Months)

| Difficulty | Problem # | Problem Name | Pattern Used | Priority |
|------------|-----------|--------------|--------------|----------|
| Medium | **200** | Number of Islands | DFS/BFS on Grid | ⭐⭐⭐⭐⭐ |
| Medium | **207** | Course Schedule | Topological Sort / Cycle Detection | ⭐⭐⭐⭐⭐ |
| Medium | **133** | Clone Graph | DFS/BFS Traversal | ⭐⭐⭐⭐⭐ |
| Medium | **1091** | Shortest Path in Binary Matrix | BFS Shortest Path | ⭐⭐⭐⭐⭐ |
| Medium | **994** | Rotting Oranges | Multi-source BFS | ⭐⭐⭐⭐⭐ |
| Medium | **210** | Course Schedule II | Topological Sort | ⭐⭐⭐⭐ |
| Medium | **323** | Number of Connected Components | Union-Find | ⭐⭐⭐⭐ |
| Medium | **79** | Word Search | Backtracking | ⭐⭐⭐⭐ |
| Medium | **743** | Network Delay Time | Dijkstra | ⭐⭐⭐⭐ |
| Medium | **542** | 01 Matrix | Multi-source BFS | ⭐⭐⭐⭐ |
| Medium | **130** | Surrounded Regions | Border DFS | ⭐⭐⭐⭐ |
| Hard | **212** | Word Search II | Trie + DFS | ⭐⭐⭐⭐ |
| Medium | **417** | Pacific Atlantic Water Flow | Multi-source DFS | ⭐⭐⭐ |
| Medium | **695** | Max Area of Island | DFS with counting | ⭐⭐⭐ |
| Hard | **127** | Word Ladder | BFS Shortest Path | ⭐⭐⭐ |

## 7-Day Preparation Plan

### Day 1-2: BFS & DFS Mastery (Foundation)
- **Morning:** 200, 133, 695
- **Afternoon:** 1091, 994, 542
- **Focus:** Get comfortable with visited sets, queue/stack operations

### Day 3: Topological Sort & Cycle Detection
- **Morning:** 207, 210
- **Afternoon:** 269 (if time), review cycle detection
- **Focus:** Kahn's algorithm, in-degree tracking

### Day 4: Union-Find
- **Morning:** Implement Union-Find with optimizations
- **Afternoon:** 323, 684, 547
- **Focus:** Path compression, union by rank

### Day 5: Grid Problems & Backtracking
- **Morning:** 79, 130
- **Afternoon:** 417, 212 (if time)
- **Focus:** 4-directional movement, state restoration

### Day 6: Shortest Path (Dijkstra)
- **Morning:** 743, 787
- **Afternoon:** 1631
- **Focus:** Min-heap usage, relaxation

### Day 7: Review & Mock Interview
- **Morning:** Redo 3 hardest problems
- **Afternoon:** Simulate interview with random problems

## Quick Decision Tree

```
Is it a GRID problem?
├─ Yes → Is it shortest path? 
│         ├─ Yes → BFS (994, 1091, 542)
│         └─ No → DFS (200, 695, 130)
│
└─ No → Is it about ORDERING/DEPENDENCIES?
          ├─ Yes → Topological Sort (207, 210, 269)
          │
          └─ No → Is it about CONNECTIVITY?
                    ├─ Yes → Union-Find OR DFS (323, 684, 547)
                    │
                    └─ No → Is it WEIGHTED graph?
                              ├─ Yes → Dijkstra (743, 787)
                              └─ No → BFS for shortest path (127, 1091)
```

## Common Interview Variations to Expect

| Base Problem | Likely Variations |
|--------------|-------------------|
| **Number of Islands** | "Find largest island", "Count islands with different shapes", "Islands with obstacles" |
| **Course Schedule** | "Return the order", "Minimum semesters", "Prerequisites with constraints" |
| **Shortest Path** | "With obstacles", "With different costs", "K stops maximum" |
| **Word Search** | "Return all words", "Count paths", "With wildcards" |

## Key Interview Tips

1. **Always clarify:**
   - Is graph directed/undirected?
   - Are there weights?
   - Can there be cycles?
   - What are edge cases (empty graph, single node)?

2. **State complexity upfront:**
   - "BFS will be O(V+E) time and O(V) space for the queue"

3. **Build the graph first if given edges:**
   ```python
   graph = [[] for _ in range(n)]
   for u, v in edges:
       graph[u].append(v)
   ```

4. **Google loves:**
   - Clean, readable code
   - Handling edge cases
   - Optimization discussion
   - Multiple approaches

## Template to Memorize

```python
# BFS Template
from collections import deque
def bfs(start):
    visited = {start}
    queue = deque([start])
    
    while queue:
        node = queue.popleft()
        
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)

# DFS Template
def dfs(node, visited):
    visited.add(node)
    
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(neighbor, visited)

# Union-Find Template
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py: return False
        if self.rank[px] < self.rank[py]:
            self.parent[px] = py
        elif self.rank[px] > self.rank[py]:
            self.parent[py] = px
        else:
            self.parent[py] = px
            self.rank[px] += 1
        return True
```

**Focus on the 🔥 problems above - they appear most frequently!**

Good luck with your Google interview! 🚀