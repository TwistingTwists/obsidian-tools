# LeetCode 2026-01 - 30 Day Graph Mastery Journey

## Overview
30-day intensive graph problem solving for Google interview prep. Goal: Master all graph patterns, algorithms, and problem-solving approaches.

**Start Date:** 2026-01-25
**Duration:** 30 days
**Target:** Graph mastery with interview-ready confidence

---

## Reference Documents

### Primary Sources
1. **Daily Worksheet:** `/home/abhishek/Downloads/experiments/learnings/Daily worksheet for graphs.md`
   - Complete 30-day schedule
   - Daily breakdown of theory/practice hours
   - Problem lists per day
   - Weekly milestones

2. **BFS/DFS Editorial Guide:** `/home/abhishek/Downloads/delittt/2026-01-06/TheEssay/BFS_DFS_EDITORIAL_GUIDE.md`
   - Learning path organized into 5 phases
   - Editorial code solutions available locally
   - Problem directory structure
   - Quick access commands

3. **Editorial Code Repository:** `/home/abhishek/Downloads/delittt/2026-01-06/TheEssay/leetcode-screenshotter/`
   - Editorial code: `editorial_code/` directory
   - Editorial screenshots: `editorial-screenshots/` directory
   - Organized by problem number ranges (1-999, 1000-1999, 2000-2999)

---

## Learning Path Structure

### Phase 1: Basic Traversals (Problems 94, 144, 145)
DFS foundations with binary trees - recursive understanding

### Phase 2: Level Order & Grids (Problems 102-107, 200, 695)
BFS and grid-based DFS patterns

### Phase 3: Advanced Patterns (Problems 133, 207, 210)
Graph cloning, cycle detection, topological sort

### Phase 4: Union-Find & Shortest Paths (Problems 323, 743, 787)
Connectivity and weighted graph algorithms

### Phase 5: Complex Problems (Problems 79, 212, 130, 417)
Backtracking, advanced grids, transformation graphs

---

## Week 1: Foundations & Core Traversals

### Day 1: Graph Basics & Representation ✅
**Status:** IN PROGRESS
**Theory Complete:** Yes (2 hrs)

**What I Did:**
- [x] Learned graph types: directed, undirected, weighted, unweighted
- [x] Studied representations: adjacency list, adjacency matrix, edge list
- [x] Understood time/space trade-offs

**Next Steps:**
- [ ] Implement graph representations from scratch
- [ ] Solve LeetCode 1971 - Find if Path Exists in Graph

---

### Day 2: DFS - Depth First Search
**Next Problem:** LeetCode 94 - Binary Tree Inorder Traversal (Easy)
**Learning Path Phase:** Phase 1 - Basic Traversals

**Key Topics:**
- Recursive DFS implementation
- Call stack visualization
- Visited set management
- Time/space complexity: O(n) time, O(h) space

**Problems to Solve:**
1. 94 - Binary Tree Inorder Traversal
2. 144 - Binary Tree Preorder Traversal
3. 145 - Binary Tree Postorder Traversal

**Editorial Resources:**
- Code: `editorial_code/1-999/094. Binary Tree Inorder Traversal/`
- Screenshots: `editorial-screenshots/1-999/094. Binary Tree Inorder Traversal.png`

---

### Day 3: BFS - Breadth First Search
**Problems:** 102, 103, 107
**Focus:** Level-order traversal with queue mechanics

---

### Day 4: Grid-based DFS
**Problems:** 200, 463, 1254
**Focus:** 4-directional navigation, boundary checking

---

### Day 5: Grid-based BFS & Multi-source BFS
**Problems:** 994, 286, 1765
**Focus:** Simultaneous propagation from multiple sources

---

### Day 6: Clone & Copy Problems
**Problems:** 133, 138
**Focus:** Graph reference handling, deep copying

---

### Day 7: Week 1 Review & Assessment
**Format:** Redo hardest problems + blind solve easy/medium

---

## Week 2: Advanced Patterns & Cycle Detection
(Days 8-14)

### Key Topics:
- Cycle detection (undirected & directed graphs)
- Topological sort (DFS & Kahn's algorithm)
- Bipartite graphs
- Building graphs from constraints

---

## Week 3: Union-Find & Shortest Paths
(Days 15-21)

### Key Topics:
- Union-Find with path compression
- Dijkstra's algorithm
- Bellman-Ford algorithm
- Floyd-Warshall algorithm

---

## Week 4: Advanced Topics & Interview Prep
(Days 22-30)

### Key Topics:
- Backtracking on graphs
- Trie + Graph hybrids
- Complex grid problems
- Minimum spanning trees
- Mock interviews

---

## Daily Templates to Build

### DFS Template (Recursive)
```python
def dfs(node, visited, graph):
    visited.add(node)
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(neighbor, visited, graph)
```

### DFS Template (Iterative)
```python
def dfs(start, graph):
    stack = [start]
    visited = set()
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            stack.extend(graph[node])
```

### BFS Template (Level Order)
```python
from collections import deque
def bfs(start, graph):
    queue = deque([start])
    visited = {start}
    while queue:
        node = queue.popleft()
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

### Union-Find Template
```python
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
        if px == py:
            return False
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        return True
```

---

## Problem Tracking

### Status Legend
- ⬜ Not Started
- 🟨 In Progress
- ✅ Completed
- 🔄 Need to Redo

### Week 1
- Day 1: Graph Basics
  - ✅ 1971 (Find if Path Exists)

- Day 2: DFS
  - ⬜ 94 (Binary Tree Inorder)
  - ⬜ 144 (Binary Tree Preorder)
  - ⬜ 145 (Binary Tree Postorder)

- Day 3: BFS
  - ⬜ 102 (Binary Tree Level Order)
  - ⬜ 103 (Binary Tree Zigzag)
  - ⬜ 107 (Binary Tree Level Order II)

- Day 4: Grid DFS
  - ⬜ 200 (Number of Islands)
  - ⬜ 463 (Island Perimeter)
  - ⬜ 1254 (Closed Islands)

- Day 5: Multi-source BFS
  - ⬜ 994 (Rotting Oranges)
  - ⬜ 286 (Walls and Gates)
  - ⬜ 1765 (Map of Highest Peak)

- Day 6: Clone & Copy
  - ⬜ 133 (Clone Graph)
  - ⬜ 138 (Copy List with Random Pointer)

- Day 7: Week 1 Review
  - ⬜ Redo all hardest problems

---

## Key Insights & Notes

### Adjacency List vs Matrix
- **List:** O(V + E) space, better for sparse graphs, preferred for interviews
- **Matrix:** O(V²) space, better for dense graphs, easier to implement

### When to Use DFS vs BFS
- **DFS:** Backtracking, topological sort, cycle detection, path existence
- **BFS:** Shortest path in unweighted graphs, level-order traversal, multi-source problems

### Critical Success Metrics
- ✅ Can recognize pattern in 30 seconds
- ✅ Can code solution in 30 minutes
- ✅ Can explain time/space complexity
- ✅ Can handle follow-up variations

---

## Progress Summary

| Week | Focus | Status |
|------|-------|--------|
| 1 | Foundations & Core Traversals | 🟨 In Progress (Day 1/7) |
| 2 | Cycles & Ordering | ⬜ Not Started |
| 3 | Union-Find & Shortest Paths | ⬜ Not Started |
| 4 | Advanced & Interview Prep | ⬜ Not Started |

---

## How to Access Editorial Solutions

```bash
# View editorial code for a problem
ls /home/abhishek/Downloads/delittt/2026-01-06/TheEssay/leetcode-screenshotter/editorial_code/1-999/

# Find Python solution for problem 94
find . -path "*094*" -name "*.py"

# View editorial screenshot
ls /home/abhishek/Downloads/delittt/2026-01-06/TheEssay/leetcode-screenshotter/editorial-screenshots/1-999/
```

---

## Last Updated
2026-01-25 - Initial setup & Day 1 theory completion
