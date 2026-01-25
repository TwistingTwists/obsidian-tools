# Graph Mastery: Question-Wise Path

## Legend
| Symbol | Meaning |
|--------|---------|
| ⭐ | Must-do (interview essential) |
| 🔑 | Key insight problem |
| 📍 | Pattern introduction |
| 💎 | Premium problem |
| → | Leads to / progression |

---

# Phase 1: Foundations (Days 1-7)

## Module 1.1: Graph Representation
**Goal:** Build any graph from scratch

### Q1: LC 1971 - Find if Path Exists in Graph 📍
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Easy | 51.80% | No |

**Pattern:** Graph construction + basic traversal
**Before solving:**
- Can you build adjacency list from edge list?
- What's the difference between directed vs undirected representation?

```python
# Template: Build adjacency list
def build_graph(n, edges, directed=False):
    adj = [[] for _ in range(n)]
    for u, v in edges:
        adj[u].append(v)
        if not directed:
            adj[v].append(u)
    return adj
```

**Checkpoint:** Solve this in <5 mins including graph construction.

---

## Module 1.2: DFS Mastery
**Goal:** Write DFS in sleep, understand stack behavior

### Q2: LC 797 - All Paths From Source to Target ⭐📍
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 82.30% | No |

**Pattern:** DFS path enumeration
**Key insight:** When do you need to backtrack vs when visited stays marked?
- Here: **Backtrack** (same node can appear in different paths)
- Connected components: **Stay marked** (just counting)

```python
# Template: DFS with path tracking (backtracking)
def dfs_paths(node, target, adj, path, result):
    if node == target:
        result.append(path[:])
        return
    for neighbor in adj[node]:
        path.append(neighbor)
        dfs_paths(neighbor, target, adj, path, result)
        path.pop()  # backtrack
```

### Q3: LC 547 - Number of Provinces ⭐🔑
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 64.00% | No |

**Pattern:** Connected components via DFS
**Key insight:** Each DFS call "floods" one component. Count the floods.

```python
# Template: Count connected components
def count_components(n, adj):
    visited = set()
    count = 0
    for node in range(n):
        if node not in visited:
            dfs(node, adj, visited)
            count += 1
    return count
```

**Warning:** This problem gives adjacency MATRIX, not list. Convert or adapt.

### Q4: LC 695 - Max Area of Island ⭐
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 71.80% | No |

**Pattern:** DFS on grid returning value
**Key insight:** DFS returns area count, sum up 1 + recursive calls.

```python
# Template: DFS on grid with return value
def dfs_grid(grid, r, c, visited):
    if r < 0 or r >= len(grid) or c < 0 or c >= len(grid[0]):
        return 0
    if (r, c) in visited or grid[r][c] == 0:
        return 0
    visited.add((r, c))
    return 1 + sum(dfs_grid(grid, r+dr, c+dc, visited)
                   for dr, dc in [(-1,0),(1,0),(0,-1),(0,1)])
```

---

## Module 1.3: BFS Mastery
**Goal:** Understand why BFS gives shortest path in unweighted graphs

### Q5: LC 1091 - Shortest Path in Binary Matrix ⭐⭐📍
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 46.40% | No |

**Pattern:** BFS shortest path in grid
**Key insight:** First time BFS reaches a cell = shortest distance (unweighted).

```python
# Template: BFS shortest path
from collections import deque

def bfs_shortest(grid, start, end):
    if grid[start[0]][start[1]] == 1:
        return -1

    queue = deque([(start[0], start[1], 1)])  # (r, c, distance)
    visited = {start}
    directions = [(-1,-1),(-1,0),(-1,1),(0,-1),(0,1),(1,-1),(1,0),(1,1)]

    while queue:
        r, c, dist = queue.popleft()
        if (r, c) == end:
            return dist
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            if 0 <= nr < len(grid) and 0 <= nc < len(grid[0]):
                if (nr, nc) not in visited and grid[nr][nc] == 0:
                    visited.add((nr, nc))
                    queue.append((nr, nc, dist + 1))
    return -1
```

### Q6: LC 542 - 01 Matrix ⭐⭐
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 45.00% | No |

**Pattern:** Multi-source BFS
**Key insight:** Start BFS from ALL zeros simultaneously. Distance propagates outward.

### Q7: LC 1162 - As Far from Land as Possible
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 51.80% | No |

**Pattern:** Multi-source BFS (inverse of 542)

### Q8: LC 994 - Rotting Oranges ⭐⭐⭐🔑
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 53.10% | No |

**Pattern:** Multi-source BFS + level tracking
**Core technique:**
1. Add ALL rotten oranges to queue initially
2. Process level by level
3. Track time = number of levels

```python
# Template: Multi-source BFS with level tracking
def multi_source_bfs(grid, sources):
    queue = deque(sources)  # all sources added initially
    time = 0

    while queue:
        for _ in range(len(queue)):  # process current level
            r, c = queue.popleft()
            for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)]:
                nr, nc = r + dr, c + dc
                if valid(nr, nc) and should_process(grid[nr][nc]):
                    grid[nr][nc] = mark_processed
                    queue.append((nr, nc))
        if queue:  # more levels to process
            time += 1
    return time
```

### Q9: LC 286 - Walls and Gates 💎
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 60.50% | Yes |

**Pattern:** Multi-source BFS from gates

### Q10: LC 1765 - Map of Highest Peak
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 60.50% | No |

**Pattern:** Multi-source BFS from water cells

---

## Module 1.4: Grid as Graph
**Goal:** Navigate any grid problem with confidence

### Q11: LC 200 - Number of Islands ⭐⭐⭐⭐⭐📍
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 57.20% | No |

**Pattern:** Connected components on grid
**This is THE grid-graph problem.** Master this, everything else is variation.

```python
# Core technique: "Sink" the island as you visit
def numIslands(grid):
    count = 0
    for r in range(len(grid)):
        for c in range(len(grid[0])):
            if grid[r][c] == '1':
                sink_island(grid, r, c)  # DFS that marks visited by changing to '0'
                count += 1
    return count
```

### Q12: LC 463 - Island Perimeter
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Easy | 69.70% | No |

**Pattern:** Grid counting (different from traversal)
**Insight:** Each land cell contributes 4 - (number of land neighbors).

### Q13: LC 1254 - Number of Closed Islands
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 66.90% | No |

**Pattern:** "Touched boundary" detection
**Key insight:** Island is NOT closed if any DFS reaches boundary.

### Q14: LC 1020 - Number of Enclaves
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 68.70% | No |

**Pattern:** Similar to closed islands - count land not connected to boundary

---

## Module 1.5: Clone Problems
**Goal:** Handle node references correctly

### Q15: LC 133 - Clone Graph ⭐⭐⭐⭐⭐🔑
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 53.40% | No |

**Pattern:** HashMap old→new mapping
**Key insight:** Build clone as you traverse, lookup prevents infinite loops.

```python
# Template: Graph cloning
def cloneGraph(node):
    if not node:
        return None

    old_to_new = {}

    def dfs(node):
        if node in old_to_new:
            return old_to_new[node]

        copy = Node(node.val)
        old_to_new[node] = copy  # map BEFORE recursing

        for neighbor in node.neighbors:
            copy.neighbors.append(dfs(neighbor))

        return copy

    return dfs(node)
```

### Q16: LC 138 - Copy List with Random Pointer
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 51.70% | No |

**Pattern:** Same HashMap technique for linked list

---

# Phase 2: Cycles & Ordering (Days 8-14)

## Module 2.1: Cycle Detection - Undirected
**Goal:** Detect cycles using parent tracking

### Q17: LC 684 - Redundant Connection ⭐⭐⭐⭐📍
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 62.40% | No |

**Pattern:** Find the edge that creates a cycle
**Two approaches:**
1. DFS with parent tracking
2. Union-Find (better for this problem)

```python
# Template: Cycle detection in undirected (DFS)
def has_cycle_undirected(node, parent, adj, visited):
    visited.add(node)
    for neighbor in adj[node]:
        if neighbor == parent:
            continue  # don't go back to parent
        if neighbor in visited:
            return True  # cycle found!
        if has_cycle_undirected(neighbor, node, adj, visited):
            return True
    return False
```

### Q18: LC 261 - Graph Valid Tree ⭐⭐⭐⭐💎
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 47.20% | Yes |

**Pattern:** Tree = connected + no cycle
**Checks:**
1. n-1 edges (necessary but not sufficient)
2. All nodes reachable from node 0
3. No cycle

---

## Module 2.2: Cycle Detection - Directed
**Goal:** Master three-color algorithm

### Q19: LC 207 - Course Schedule ⭐⭐⭐⭐⭐📍🔑
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 45.40% | No |

**Pattern:** Detect cycle in directed graph
**THE core problem for topological sort/cycle detection.**

```python
# Template: Three-color cycle detection
# WHITE=0 (unvisited), GRAY=1 (in current path), BLACK=2 (done)
def has_cycle_directed(node, adj, color):
    color[node] = 1  # GRAY - currently exploring

    for neighbor in adj[node]:
        if color[neighbor] == 1:  # back edge to node in current path
            return True
        if color[neighbor] == 0:  # unvisited
            if has_cycle_directed(neighbor, adj, color):
                return True

    color[node] = 2  # BLACK - done exploring
    return False
```

**Interview phrase:** "I'm using the three-color algorithm. Gray nodes are in the current DFS path. If we reach a gray node, we've found a back edge, which means a cycle."

### Q20: LC 802 - Find Eventual Safe States
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 57.30% | No |

**Pattern:** Reverse cycle detection
**Insight:** Safe = not part of any cycle. Find all nodes that are BLACK after DFS.

---

## Module 2.3: Topological Sort
**Goal:** Know both approaches cold

### Q21: LC 210 - Course Schedule II ⭐⭐⭐⭐⭐📍
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 48.70% | No |

**Pattern:** Topological ordering

**Approach 1: DFS Post-order (reverse)**
```python
def topo_dfs(n, adj):
    color = [0] * n
    order = []

    def dfs(node):
        color[node] = 1
        for neighbor in adj[node]:
            if color[neighbor] == 1:
                return False  # cycle
            if color[neighbor] == 0:
                if not dfs(neighbor):
                    return False
        color[node] = 2
        order.append(node)  # post-order
        return True

    for i in range(n):
        if color[i] == 0:
            if not dfs(i):
                return []  # cycle exists

    return order[::-1]  # reverse post-order
```

### Q22: LC 210 - Course Schedule II (Kahn's) ⭐⭐⭐⭐⭐
**Approach 2: BFS with in-degree**
```python
def topo_kahn(n, adj):
    in_degree = [0] * n
    for u in range(n):
        for v in adj[u]:
            in_degree[v] += 1

    queue = deque([i for i in range(n) if in_degree[i] == 0])
    order = []

    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in adj[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    return order if len(order) == n else []  # cycle if not all processed
```

**When to use which:**
- DFS: When you need to detect cycle + order together
- Kahn's: When you need level-by-level processing (parallel courses)

### Q23: LC 1462 - Course Schedule IV
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 49.10% | No |

**Pattern:** Topological sort + reachability queries

### Q24: LC 310 - Minimum Height Trees 🔑
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 38.60% | No |

**Pattern:** Kahn's from leaves inward
**Insight:** Keep removing leaves until 1-2 nodes remain. Those are the centers.

### Q25: LC 2050 - Parallel Courses III
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Hard | 59.10% | No |

**Pattern:** Kahn's + track longest path to each node

### Q26: LC 1136 - Parallel Courses 💎
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 61.40% | Yes |

**Pattern:** Kahn's with level tracking for minimum semesters

---

## Module 2.4: Build Graph from Constraints

### Q27: LC 269 - Alien Dictionary ⭐⭐⭐⭐🔑💎
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Hard | ~35% | Yes |

**Pattern:** Extract edges from ordering, then topo sort
**Key insight:** Compare adjacent words to find ONE edge per pair.

```python
# Extract edges from word ordering
def extract_edges(words):
    edges = []
    for i in range(len(words) - 1):
        w1, w2 = words[i], words[i+1]
        for c1, c2 in zip(w1, w2):
            if c1 != c2:
                edges.append((c1, c2))  # c1 comes before c2
                break
        else:
            if len(w1) > len(w2):  # "abc" before "ab" is invalid
                return None
    return edges
```

---

## Module 2.5: Bipartite Graphs

### Q28: LC 785 - Is Graph Bipartite? ⭐⭐⭐📍
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 54.50% | No |

**Pattern:** Two-coloring
**Insight:** Bipartite ⟺ no odd-length cycles ⟺ 2-colorable

```python
# Template: Bipartite check
def is_bipartite(adj, n):
    color = [-1] * n

    for start in range(n):
        if color[start] != -1:
            continue
        queue = deque([start])
        color[start] = 0

        while queue:
            node = queue.popleft()
            for neighbor in adj[node]:
                if color[neighbor] == color[node]:
                    return False  # same color = not bipartite
                if color[neighbor] == -1:
                    color[neighbor] = 1 - color[node]
                    queue.append(neighbor)
    return True
```

### Q29: LC 886 - Possible Bipartition
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 50.10% | No |

**Pattern:** Bipartite on implicit graph
**Build graph:** If A dislikes B, they must be in different groups.

---

# Phase 3: Union-Find & Shortest Paths (Days 15-21)

## Module 3.1: Union-Find

### Template: Union-Find with Path Compression
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
        self.components = n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # path compression
        return self.parent[x]

    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False  # already connected

        # union by rank
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1

        self.components -= 1
        return True
```

### Q30: LC 323 - Number of Connected Components in an Undirected Graph ⭐⭐⭐⭐📍💎
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 62.30% | Yes |

**Pattern:** Basic Union-Find application
**Compare:** DFS also works. UF is better for dynamic connectivity.

### Q31: LC 684 - Redundant Connection (revisit with UF) ⭐⭐⭐⭐
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 62.40% | No |

**Pattern:** First edge that connects already-connected nodes
**With UF:** Process edges in order. First `union()` that returns False = answer.

### Q32: LC 547 - Number of Provinces (revisit with UF) ⭐⭐⭐
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 64.00% | No |

**Pattern:** Connected components - compare DFS vs UF approach

### Q33: LC 1319 - Number of Operations to Make Network Connected
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 62.20% | No |

**Pattern:** Count components, check if enough extra edges exist

### Q34: LC 721 - Accounts Merge ⭐⭐⭐🔑
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 56.30% | No |

**Pattern:** Union-Find with string mapping
**Key:** Map each email to an index, union emails within same account.

### Q35: LC 990 - Satisfiability of Equality Equations
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 50.50% | No |

**Pattern:** Process == first (union), then check != (should not be same root)

### Q36: LC 1579 - Remove Max Number of Edges to Keep Graph Fully Traversable
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Hard | 65.40% | No |

**Pattern:** Two Union-Find instances (Alice & Bob) + greedy edge removal

---

## Module 3.2: Dijkstra's Algorithm

### Core Understanding
**When to use:** Weighted graph + non-negative weights + single-source shortest path

**Why it works:** When we pop a node from min-heap, its distance is final. Any alternative path through a farther node can only add non-negative cost.

**Why negative edges break it:** A "farther" node could provide a shortcut via negative edge.

### Template: Dijkstra
```python
import heapq

def dijkstra(n, adj, source):
    dist = [float('inf')] * n
    dist[source] = 0
    heap = [(0, source)]

    while heap:
        d, node = heapq.heappop(heap)

        if d > dist[node]:  # stale entry
            continue

        for neighbor, weight in adj[node]:
            new_dist = d + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                heapq.heappush(heap, (new_dist, neighbor))

    return dist
```

### Q37: LC 743 - Network Delay Time ⭐⭐⭐⭐📍
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 52.10% | No |

**Pattern:** Basic Dijkstra
**Answer:** Max of all distances (time for signal to reach farthest node)

### Q38: LC 787 - Cheapest Flights Within K Stops ⭐⭐⭐🔑
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 37.10% | No |

**Pattern:** Modified Dijkstra with state = (cost, node, stops)
**Key:** Can't just skip stale entries. Node might be reached with fewer stops later.

```python
# Modified state: (cost, node, stops_remaining)
def cheapest_k_stops(n, flights, src, dst, k):
    adj = defaultdict(list)
    for u, v, w in flights:
        adj[u].append((v, w))

    heap = [(0, src, k + 1)]  # (cost, node, stops_remaining)

    while heap:
        cost, node, stops = heapq.heappop(heap)

        if node == dst:
            return cost

        if stops > 0:
            for neighbor, price in adj[node]:
                heapq.heappush(heap, (cost + price, neighbor, stops - 1))

    return -1
```

### Q39: LC 1631 - Path With Minimum Effort ⭐⭐⭐
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 55.90% | No |

**Pattern:** Dijkstra on grid with custom weight
**Weight:** max absolute difference along path (not sum)

### Q40: LC 1514 - Path with Maximum Probability
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 48.90% | No |

**Pattern:** Dijkstra with max-heap (or negate probabilities)

---

## Module 3.3: Bellman-Ford & Floyd-Warshall

### When to use
| Algorithm | Use case | Complexity |
|-----------|----------|------------|
| Dijkstra | Single-source, non-negative weights | O((V+E) log V) |
| Bellman-Ford | Single-source, handles negative weights | O(V × E) |
| Floyd-Warshall | All-pairs shortest path | O(V³) |

### Q41: LC 787 - Cheapest Flights (Bellman-Ford approach)
**Why it works here:** K stops = K+1 edges = K+1 relaxation rounds

### Q42: LC 1334 - Find the City With the Smallest Number of Neighbors at a Threshold Distance
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 55.40% | No |

**Pattern:** Floyd-Warshall to get all-pairs, then filter by threshold

---

# Phase 4: Advanced Patterns (Days 22-27)

## Module 4.1: Backtracking on Graphs

### Q43: LC 79 - Word Search ⭐⭐⭐⭐📍
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 40.30% | No |

**Pattern:** DFS with backtracking
**Key difference from regular DFS:** Unmark visited after exploring (same cell can be in different paths)

```python
def exist(board, word):
    def dfs(r, c, idx):
        if idx == len(word):
            return True
        if r < 0 or r >= len(board) or c < 0 or c >= len(board[0]):
            return False
        if board[r][c] != word[idx]:
            return False

        temp = board[r][c]
        board[r][c] = '#'  # mark visited

        found = any(dfs(r+dr, c+dc, idx+1)
                   for dr, dc in [(0,1),(0,-1),(1,0),(-1,0)])

        board[r][c] = temp  # backtrack
        return found

    for r in range(len(board)):
        for c in range(len(board[0])):
            if dfs(r, c, 0):
                return True
    return False
```

### Q44: LC 980 - Unique Paths III ⭐⭐⭐
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Hard | 81.60% | No |

**Pattern:** Backtracking + count all valid paths
**Must visit:** All empty cells exactly once

### Q45: LC 1066 - Campus Bikes II 💎
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 54.90% | Yes |

**Pattern:** Backtracking with bitmask DP optimization

---

## Module 4.2: Trie + Graph

### Q46: LC 212 - Word Search II ⭐⭐⭐⭐🔑
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Hard | 36.30% | No |

**Pattern:** Trie optimization for multiple word search
**Key insight:** Build Trie of words, DFS on grid while traversing Trie simultaneously

### Q47: LC 1233 - Remove Sub-Folders from the Filesystem
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 65.60% | No |

**Pattern:** Trie for path matching

---

## Module 4.3: Advanced Grid Problems

### Q48: LC 130 - Surrounded Regions ⭐⭐⭐⭐📍
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 37.10% | No |

**Pattern:** Boundary-connected DFS
**Insight:** Mark 'O's connected to boundary. Everything else gets captured.

### Q49: LC 417 - Pacific Atlantic Water Flow ⭐⭐⭐⭐🔑
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 54.50% | No |

**Pattern:** Reverse DFS from oceans
**Key insight:** Instead of "can water flow to ocean?", ask "where can ocean reach?"
Start DFS from all ocean borders, find intersection.

### Q50: LC 1559 - Detect Cycles in 2D Grid
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 48.10% | No |

**Pattern:** Cycle detection on grid (4-directional)

### Q51: LC 1905 - Count Sub Islands
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 67.80% | No |

**Pattern:** DFS with validation against parent grid

---

## Module 4.4: Transformation Graphs

### Q52: LC 127 - Word Ladder ⭐⭐⭐⭐📍
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Hard | 37.30% | No |

**Pattern:** BFS on implicit graph (words are nodes)
**Key insight:** Graph isn't given. Each word connects to words differing by 1 char.

```python
def ladderLength(beginWord, endWord, wordList):
    wordSet = set(wordList)
    if endWord not in wordSet:
        return 0

    queue = deque([(beginWord, 1)])
    visited = {beginWord}

    while queue:
        word, length = queue.popleft()

        if word == endWord:
            return length

        for i in range(len(word)):
            for c in 'abcdefghijklmnopqrstuvwxyz':
                next_word = word[:i] + c + word[i+1:]
                if next_word in wordSet and next_word not in visited:
                    visited.add(next_word)
                    queue.append((next_word, length + 1))

    return 0
```

### Q53: LC 126 - Word Ladder II
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Hard | 27.50% | No |

**Pattern:** BFS for shortest path + backtracking for all paths

### Q54: LC 433 - Minimum Genetic Mutation
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 52.50% | No |

**Pattern:** Same as Word Ladder with 4-char alphabet (A,C,G,T)

### Q55: LC 752 - Open the Lock
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 55.70% | No |

**Pattern:** BFS on state space (4-digit lock combinations)

---

## Module 4.5: Minimum Spanning Tree

### Q56: LC 1584 - Min Cost to Connect All Points ⭐⭐⭐📍
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 64.00% | No |

**Pattern:** Kruskal's with Union-Find

```python
def minCostConnectPoints(points):
    n = len(points)
    edges = []

    for i in range(n):
        for j in range(i+1, n):
            dist = abs(points[i][0]-points[j][0]) + abs(points[i][1]-points[j][1])
            edges.append((dist, i, j))

    edges.sort()
    uf = UnionFind(n)
    cost = 0

    for d, u, v in edges:
        if uf.union(u, v):
            cost += d
            if uf.components == 1:
                break

    return cost
```

### Q57: LC 1135 - Connecting Cities With Minimum Cost 💎
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Medium | 61.40% | Yes |

**Pattern:** Classic MST with Kruskal's

---

## Module 4.6: Advanced Graph Patterns

### Q58: LC 332 - Reconstruct Itinerary 🔑
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Hard | 41.30% | No |

**Pattern:** Eulerian path (Hierholzer's algorithm)
**Key:** Visit all edges exactly once, lexicographically smallest

### Q59: LC 1436 - Destination City
| Difficulty | Acceptance | Premium |
|------------|------------|---------|
| Easy | 77.30% | No |

**Pattern:** Find node with in-degree > 0 and out-degree = 0

---

# Quick Reference: Pattern Recognition

| Keywords | Pattern | First Problem to Try |
|----------|---------|---------------------|
| "shortest path" + unweighted | BFS | LC 1091 |
| "shortest path" + weighted | Dijkstra | LC 743 |
| "all paths" | DFS backtracking | LC 797 |
| "number of islands/components" | DFS flood fill | LC 200 |
| "rotting/spreading" | Multi-source BFS | LC 994 |
| "course schedule" / "prerequisites" | Topological sort | LC 207 |
| "valid tree" | n-1 edges + connected + no cycle | LC 261 |
| "bipartite" / "two groups" | 2-coloring BFS/DFS | LC 785 |
| "redundant edge" | Union-Find | LC 684 |
| "minimum cost to connect" | MST (Kruskal/Prim) | LC 1584 |
| "word ladder" / "transformation" | BFS on implicit graph | LC 127 |
| "surrounded" / "boundary" | DFS from boundary | LC 130 |
| "within K stops" | Modified Dijkstra/Bellman-Ford | LC 787 |
| "all pairs distance" | Floyd-Warshall | LC 1334 |

---

# Complete Problem List by Day

## Week 1: Foundations
| Day | Focus | Problems |
|-----|-------|----------|
| 1 | Graph construction | 1971 Find if Path Exists in Graph |
| 2 | DFS | 797 All Paths From Source to Target, 547 Number of Provinces, 695 Max Area of Island |
| 3 | BFS | 1091 Shortest Path in Binary Matrix, 542 01 Matrix, 1162 As Far from Land as Possible |
| 4 | Grid DFS | 200 Number of Islands, 463 Island Perimeter, 1254 Number of Closed Islands |
| 5 | Multi-source BFS | 994 Rotting Oranges, 286 Walls and Gates💎, 1765 Map of Highest Peak |
| 6 | Clone problems | 133 Clone Graph, 138 Copy List with Random Pointer |
| 7 | Review | Redo hardest from each day |

## Week 2: Cycles & Ordering
| Day | Focus | Problems |
|-----|-------|----------|
| 8 | Undirected cycle | 684 Redundant Connection, 261 Graph Valid Tree💎 |
| 9 | Directed cycle | 207 Course Schedule, 802 Find Eventual Safe States |
| 10 | Topo sort (DFS) | 210 Course Schedule II, 1462 Course Schedule IV |
| 11 | Topo sort (Kahn's) | 210 Course Schedule II, 310 Minimum Height Trees, 2050 Parallel Courses III |
| 12 | Constraint graphs | 269 Alien Dictionary💎 |
| 13 | Bipartite | 785 Is Graph Bipartite?, 886 Possible Bipartition |
| 14 | Review | Mixed practice |

## Week 3: Union-Find & Shortest Paths
| Day | Focus | Problems |
|-----|-------|----------|
| 15 | Union-Find basics | Template implementation |
| 16 | UF applications | 323 Number of Connected Components💎, 684 Redundant Connection, 547 Number of Provinces |
| 17 | Advanced UF | 721 Accounts Merge, 990 Satisfiability of Equality Equations, 1579 Remove Max Edges |
| 18 | Dijkstra theory | Template implementation |
| 19 | Dijkstra applications | 743 Network Delay Time, 787 Cheapest Flights K Stops, 1631 Path With Min Effort |
| 20 | Bellman-Ford | 787 (redo), 1334 Find the City |
| 21 | Speed practice | Timed problems |

## Week 4: Advanced & Mock
| Day | Focus | Problems |
|-----|-------|----------|
| 22 | Backtracking | 79 Word Search, 980 Unique Paths III, 1066 Campus Bikes II💎 |
| 23 | Trie + Graph | 212 Word Search II, 1233 Remove Sub-Folders |
| 24 | Complex grids | 130 Surrounded Regions, 417 Pacific Atlantic Water Flow, 1559 Detect Cycles 2D, 1905 Count Sub Islands |
| 25 | Transformation graphs | 127 Word Ladder, 433 Minimum Genetic Mutation, 752 Open the Lock |
| 26 | MST | 1584 Min Cost to Connect All Points, 1135 Connecting Cities💎 |
| 27 | Advanced patterns | 332 Reconstruct Itinerary, 1436 Destination City |
| 28-30 | Mock interviews | Random selection |

---

# Success Metrics Checklist

- [ ] Can recognize pattern in <30 seconds
- [ ] Can code DFS template without reference
- [ ] Can code BFS template without reference
- [ ] Can code Union-Find without reference
- [ ] Can code Dijkstra without reference
- [ ] Can code Topo Sort (both) without reference
- [ ] Can explain time/space complexity for each
- [ ] Can handle follow-up variations
- [ ] Complete 50+ problems
