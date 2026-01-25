
Graph For Beginners [Problems | Pattern | Sample Solutions](https://leetcode.com/discuss/study-guide/655708/Graph-For-Beginners-Problems-or-Pattern-or-Sample-Solutions)

---

[[basics cheatsheet]]
[[Beyond basics graph]]
[[Grid based graph]]
[[Important problems to master for Google]]

[[Daily worksheet for graphs]]
[[Union find - use cases]]


# Graph Patterns Master Table

## Basic Patterns

| Pattern | Concepts Needed | Key Techniques | LeetCode Problems |
|---------|----------------|----------------|-------------------|
| **Graph Traversal - DFS** | Recursion, Stack, Visited set | Recursive DFS, Iterative DFS with stack | 133 (Clone Graph)<br>797 (All Paths Source to Target)<br>200 (Number of Islands) |
| **Graph Traversal - BFS** | Queue, Level-order traversal | Deque, Level tracking | 102 (Binary Tree Level Order)<br>207 (Course Schedule)<br>1091 (Shortest Path Binary Matrix) |
| **Connected Components** | DFS/BFS, Visited tracking | Component counting | 200 (Number of Islands)<br>547 (Number of Provinces)<br>323 (Number of Connected Components) |
| **Cycle Detection - Undirected** | DFS with parent tracking | Back edge detection | 684 (Redundant Connection)<br>261 (Graph Valid Tree) |
| **Cycle Detection - Directed** | DFS with 3-color marking | White-Gray-Black coloring | 207 (Course Schedule)<br>210 (Course Schedule II) |
| **Bipartite Graph** | BFS/DFS with 2-coloring | Odd cycle detection | 785 (Is Graph Bipartite)<br>886 (Possible Bipartition) |
| **Path Finding** | DFS/BFS, Path tracking | Backtracking, Path storage | 797 (All Paths Source to Target)<br>1971 (Find if Path Exists) |

## Intermediate Patterns

| Pattern | Concepts Needed | Key Techniques | LeetCode Problems |
|---------|----------------|----------------|-------------------|
| **Topological Sort** | DAG, In-degree, DFS/BFS | Kahn's Algorithm, DFS post-order | 207 (Course Schedule)<br>210 (Course Schedule II)<br>269 (Alien Dictionary)<br>310 (Minimum Height Trees) |
| **Union-Find (Disjoint Set)** | Path compression, Union by rank | Find, Union operations | 684 (Redundant Connection)<br>323 (Connected Components)<br>547 (Number of Provinces)<br>200 (Number of Islands) |
| **Shortest Path - Unweighted** | BFS, Level tracking | Multi-source BFS | 1091 (Shortest Path Binary Matrix)<br>127 (Word Ladder)<br>542 (01 Matrix)<br>286 (Walls and Gates) |
| **Shortest Path - Weighted (Dijkstra)** | Min-heap, Priority queue | Relaxation technique | 743 (Network Delay Time)<br>787 (Cheapest Flights K Stops)<br>1631 (Path With Minimum Effort)<br>778 (Swim in Rising Water) |
| **Minimum Spanning Tree** | Union-Find OR Min-heap | Kruskal's, Prim's | 1584 (Min Cost to Connect Points)<br>1135 (Connecting Cities Min Cost)<br>1168 (Optimize Water Distribution) |
| **Grid - Island Problems** | DFS/BFS on 2D matrix | 4/8-directional traversal | 200 (Number of Islands)<br>695 (Max Area of Island)<br>463 (Island Perimeter)<br>694 (Number of Distinct Islands) |
| **Grid - Shortest Path** | BFS on grid | Multi-source BFS, Obstacles | 1091 (Shortest Path Binary Matrix)<br>994 (Rotting Oranges)<br>286 (Walls and Gates)<br>542 (01 Matrix) |
| **Matrix Modification** | DFS/BFS, In-place marking | Border DFS, Flood fill | 130 (Surrounded Regions)<br>733 (Flood Fill)<br>1020 (Number of Enclaves) |

## Advanced Patterns

| Pattern | Concepts Needed | Key Techniques | LeetCode Problems |
|---------|----------------|----------------|-------------------|
| **Strongly Connected Components** | DFS, Graph reversal | Kosaraju's, Tarjan's | 1192 (Critical Connections)<br>1568 (Minimum Days to Disconnect) |
| **Articulation Points & Bridges** | DFS, Discovery/Low times | Tarjan's algorithm | 1192 (Critical Connections)<br>1489 (Find Critical and Pseudo-Critical Edges) |
| **Eulerian Path/Circuit** | Degree counting, DFS | Hierholzer's algorithm | 332 (Reconstruct Itinerary)<br>753 (Cracking the Safe) |
| **Bellman-Ford** | Edge relaxation, Negative cycles | N-1 iterations | 787 (Cheapest Flights K Stops)<br>743 (Network Delay Time with negatives) |
| **Floyd-Warshall** | DP on graphs, All-pairs | 3-nested loops DP | 1334 (Find City With Smallest Number)<br>1462 (Course Schedule IV) |
| **A* Search** | Heuristics, Priority queue | Manhattan/Euclidean distance | 1091 (Shortest Path Binary Matrix)<br>1631 (Path With Minimum Effort) |
| **Trie + Graph DFS** | Trie structure, Backtracking | Prefix matching + DFS | 212 (Word Search II)<br>1065 (Index Pairs of a String) |
| **Graph Backtracking** | DFS with state restore | Path tracking, Pruning | 79 (Word Search)<br>980 (Unique Paths III)<br>1631 (Path With Minimum Effort) |

## Expert Patterns

| Pattern | Concepts Needed | Key Techniques | LeetCode Problems |
|---------|----------------|----------------|-------------------|
| **Bidirectional BFS** | Two simultaneous BFS | Meet in the middle | 127 (Word Ladder)<br>752 (Open the Lock)<br>815 (Bus Routes) |
| **0-1 BFS** | Deque, Weight-based ordering | Add front for 0-weight edges | 1368 (Minimum Cost to Make Valid Path)<br>2290 (Minimum Obstacle Removal) |
| **State-Space Search** | Multi-dimensional visited | BFS/DFS with state tuples | 847 (Shortest Path Visiting All Nodes)<br>864 (Shortest Path to Get All Keys)<br>1091 with obstacles |
| **Dynamic Programming on Graphs** | DP + Graph traversal | Memoization on DAG | 1218 (Longest Arithmetic Subsequence)<br>329 (Longest Increasing Path Matrix)<br>1976 (Number of Ways to Arrive) |
| **Network Flow** | Max flow min cut, Residual graphs | Ford-Fulkerson, Edmonds-Karp | N/A (Advanced competitive programming) |
| **Tree DP on Graphs** | Tree structure, Bottom-up DP | Re-rooting technique | 310 (Minimum Height Trees)<br>834 (Sum of Distances in Tree) |
| **Binary Lifting / LCA** | Sparse table, Preprocessing | Jump pointers | 1483 (Kth Ancestor of Tree Node)<br>1632 (Rank Transform of Matrix) |

## Problem Difficulty Distribution

| Difficulty | Focus Areas | Recommended Count |
|------------|-------------|-------------------|
| **Easy** | Basic DFS/BFS, Simple connected components | 10-15 problems |
| **Medium** | Topological sort, Union-Find, Dijkstra, Grid problems | 30-40 problems |
| **Hard** | Advanced shortest path, SCC, State-space, DP on graphs | 15-20 problems |

## Study Path Recommendation

### Week 1-2: Foundations
- Graph representation and building
- DFS and BFS (both recursive and iterative)
- Connected components
- Cycle detection (undirected and directed)
- **Practice:** 133, 200, 547, 684, 207, 797

### Week 3-4: Core Algorithms
- Topological sort (both methods)
- Union-Find with optimizations
- Basic grid traversal
- Shortest path (unweighted)
- **Practice:** 210, 323, 695, 1091, 994, 542

### Week 5-6: Weighted Graphs
- Dijkstra's algorithm
- Minimum spanning tree
- Bipartite graphs
- Grid shortest path variations
- **Practice:** 743, 1584, 785, 286, 1631

### Week 7-8: Advanced Topics
- Strongly connected components
- Bridges and articulation points
- Bellman-Ford
- Backtracking on grids
- **Practice:** 1192, 79, 212, 787

### Week 9-10: Expert Level
- State-space search
- DP on graphs
- Bidirectional BFS
- Complex grid problems
- **Practice:** 847, 864, 329, 127, 815

## Quick Reference: When to Use What

| Scenario | Best Algorithm | Time Complexity |
|----------|---------------|-----------------|
| Check connectivity | DFS/BFS or Union-Find | O(V+E) or O(E·α(V)) |
| Count components | DFS/BFS or Union-Find | O(V+E) |
| Detect cycle (undirected) | DFS with parent or Union-Find | O(V+E) |
| Detect cycle (directed) | DFS with colors | O(V+E) |
| Shortest path (unweighted) | BFS | O(V+E) |
| Shortest path (weighted, non-negative) | Dijkstra | O((V+E)·log V) |
| Shortest path (negative weights) | Bellman-Ford | O(V·E) |
| All pairs shortest path | Floyd-Warshall | O(V³) |
| Minimum spanning tree | Kruskal's or Prim's | O(E·log E) |
| Topological order | Kahn's or DFS | O(V+E) |
| Check bipartite | BFS/DFS with 2-coloring | O(V+E) |

