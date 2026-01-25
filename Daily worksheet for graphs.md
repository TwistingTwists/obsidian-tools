# 30-Day Graph Mastery Schedule for Google Interviews

## Week 1: Foundations & Core Traversals

### **Day 1: Graph Basics & Representation**
**Key Focus:** Understanding graph fundamentals
- **Theory (2 hrs):**
  - Graph types: directed, undirected, weighted, unweighted
  - Representations: adjacency list, adjacency matrix, edge list
  - When to use each representation
- **Practice (2 hrs):**
  - Build graphs from edge lists manually
  - Implement adjacency list and matrix
  - **Problem:** 1971 (Find if Path Exists in Graph)
- **Achievement:** Can build any graph representation from scratch

---

### **Day 2: DFS - Depth First Search**
**Key Focus:** Master recursive and iterative DFS
- **Theory (1 hr):**
  - DFS algorithm, call stack visualization
  - Visited set management
  - Time/space complexity analysis
- **Practice (3 hrs):**
  - Implement both recursive and iterative DFS
  - **Problems:** 
    - 797 (All Paths From Source to Target) ⭐
    - 547 (Number of Provinces) ⭐⭐
    - 695 (Max Area of Island) ⭐⭐
- **Achievement:** Can write DFS in sleep, understand stack behavior

---

### **Day 3: BFS - Breadth First Search**
**Key Focus:** Master level-order traversal
- **Theory (1 hr):**
  - BFS algorithm, queue mechanics
  - Level tracking techniques
  - BFS vs DFS - when to use what
- **Practice (3 hrs):**
  - Implement BFS with level tracking
  - **Problems:**
    - 1091 (Shortest Path in Binary Matrix) ⭐⭐⭐⭐⭐
    - 542 (01 Matrix) ⭐⭐⭐⭐
    - 1162 (As Far from Land as Possible)
- **Achievement:** Understand why BFS gives shortest path in unweighted graphs

---

### **Day 4: Grid-based DFS**
**Key Focus:** 4-directional/8-directional traversal
- **Theory (1 hr):**
  - Grid as implicit graph
  - Direction arrays: `[(0,1), (1,0), (0,-1), (-1,0)]`
  - Boundary checking
- **Practice (3 hrs):**
  - **Problems:**
    - 200 (Number of Islands) ⭐⭐⭐⭐⭐
    - 463 (Island Perimeter)
    - 1254 (Number of Closed Islands)
- **Achievement:** Can navigate any grid problem with confidence

---

### **Day 5: Grid-based BFS & Multi-source BFS**
**Key Focus:** Simultaneous propagation from multiple sources
- **Theory (1 hr):**
  - Multi-source BFS pattern
  - Why it works for "nearest" problems
- **Practice (3 hrs):**
  - **Problems:**
    - 994 (Rotting Oranges) ⭐⭐⭐⭐⭐
    - 286 (Walls and Gates)
    - 1765 (Map of Highest Peak)
- **Achievement:** Can handle complex BFS scenarios with multiple starting points

---

### **Day 6: Clone & Copy Problems**
**Key Focus:** Deep copying graph structures
- **Theory (1 hr):**
  - Handling node references
  - HashMap for old→new node mapping
- **Practice (3 hrs):**
  - **Problems:**
    - 133 (Clone Graph) ⭐⭐⭐⭐⭐
    - 138 (Copy List with Random Pointer)
- **Achievement:** Understand reference handling in graph problems

---

### **Day 7: Week 1 Review & Assessment**
**Key Focus:** Consolidation
- **Morning (2 hrs):** Redo hardest problem from each day
- **Afternoon (2 hrs):** 
  - Solve 2 random easy/medium problems blind
  - Write out DFS/BFS templates from memory
- **Achievement:** Solid foundation in basic graph traversals

---

## Week 2: Advanced Patterns & Cycle Detection

### **Day 8: Cycle Detection in Undirected Graphs**
**Key Focus:** Parent tracking method
- **Theory (1.5 hrs):**
  - DFS with parent parameter
  - Why visiting parent doesn't count as cycle
- **Practice (2.5 hrs):**
  - **Problems:**
    - 684 (Redundant Connection) ⭐⭐⭐⭐
    - 261 (Graph Valid Tree) ⭐⭐⭐⭐
- **Achievement:** Can detect cycles in undirected graphs

---

### **Day 9: Cycle Detection in Directed Graphs**
**Key Focus:** Three-color algorithm
- **Theory (1.5 hrs):**
  - White (unvisited), Gray (visiting), Black (visited)
  - Recursion stack vs visited set
- **Practice (2.5 hrs):**
  - **Problems:**
    - 207 (Course Schedule) ⭐⭐⭐⭐⭐
    - 802 (Find Eventual Safe States)
- **Achievement:** Master directed graph cycle detection

---

### **Day 10: Topological Sort - DFS Approach**
**Key Focus:** Post-order DFS for topological ordering
- **Theory (1.5 hrs):**
  - Why post-order gives reverse topological order
  - Detecting cycles as prerequisite
- **Practice (2.5 hrs):**
  - **Problems:**
    - 210 (Course Schedule II) ⭐⭐⭐⭐⭐
    - 1462 (Course Schedule IV)
- **Achievement:** Can produce topological ordering with DFS

---

### **Day 11: Topological Sort - Kahn's Algorithm (BFS)**
**Key Focus:** In-degree based approach
- **Theory (1.5 hrs):**
  - In-degree calculation
  - Processing nodes with in-degree 0
  - Cycle detection via count
- **Practice (2.5 hrs):**
  - **Problems:**
    - 210 (Course Schedule II - redo with Kahn's) ⭐⭐⭐⭐⭐
    - 310 (Minimum Height Trees)
    - 2050 (Parallel Courses III)
- **Achievement:** Know both topological sort approaches cold

---

### **Day 12: Alien Dictionary & Build Order Problems**
**Key Focus:** Building graph from implicit constraints
- **Theory (1 hr):**
  - Extracting edges from ordering constraints
  - Handling edge cases (cycles, invalid inputs)
- **Practice (3 hrs):**
  - **Problems:**
    - 269 (Alien Dictionary) ⭐⭐⭐⭐
    - 1203 (Sort Items by Groups Respecting Dependencies)
- **Achievement:** Can construct graphs from problem constraints

---

### **Day 13: Bipartite Graphs**
**Key Focus:** Two-coloring with BFS/DFS
- **Theory (1.5 hrs):**
  - Bipartite definition
  - Coloring/grouping algorithm
  - Relationship with odd-length cycles
- **Practice (2.5 hrs):**
  - **Problems:**
    - 785 (Is Graph Bipartite?) ⭐⭐⭐
    - 886 (Possible Bipartition)
- **Achievement:** Understand graph partitioning

---

### **Day 14: Week 2 Review & Mixed Practice**
**Key Focus:** Applying patterns
- **Morning (2 hrs):** Redo one problem from each pattern
- **Afternoon (2 hrs):** Solve 3 random medium problems
- **Achievement:** Can identify which pattern to use quickly

---

## Week 3: Union-Find & Shortest Paths

### **Day 15: Union-Find Basics**
**Key Focus:** Path compression & union by rank
- **Theory (2 hrs):**
  - Disjoint sets concept
  - Path compression optimization
  - Union by rank/size
  - Amortized time complexity
- **Implementation (2 hrs):**
  - Code UnionFind class from scratch
  - Test with simple examples
- **Achievement:** Have working UnionFind template

---

### **Day 16: Union-Find Applications**
**Key Focus:** Connected components
- **Practice (4 hrs):**
  - **Problems:**
    - 323 (Number of Connected Components) ⭐⭐⭐⭐
    - 684 (Redundant Connection - redo with UF) ⭐⭐⭐⭐
    - 547 (Number of Provinces - redo with UF) ⭐⭐⭐
    - 1319 (Number of Operations to Make Network Connected)
- **Achievement:** Can choose between DFS and Union-Find appropriately

---

### **Day 17: Advanced Union-Find**
**Key Focus:** Dynamic connectivity
- **Practice (4 hrs):**
  - **Problems:**
    - 721 (Accounts Merge) ⭐⭐⭐
    - 990 (Satisfiability of Equality Equations)
    - 1579 (Remove Max Number of Edges to Keep Graph Fully Traversable)
- **Achievement:** Handle complex union-find scenarios

---

### **Day 18: Dijkstra's Algorithm - Theory & Implementation**
**Key Focus:** Single-source shortest path in weighted graphs
- **Theory (2 hrs):**
  - Greedy approach with min-heap
  - Relaxation concept
  - Why negative weights break it
- **Implementation (2 hrs):**
  - Code from scratch with heapq
  - Test on small examples
- **Achievement:** Can implement Dijkstra's without reference

---

### **Day 19: Dijkstra's Applications**
**Key Focus:** Variants and modifications
- **Practice (4 hrs):**
  - **Problems:**
    - 743 (Network Delay Time) ⭐⭐⭐⭐
    - 787 (Cheapest Flights Within K Stops) ⭐⭐⭐
    - 1631 (Path With Minimum Effort) ⭐⭐⭐
- **Achievement:** Can modify Dijkstra's for constraints

---

### **Day 20: Bellman-Ford & Floyd-Warshall**
**Key Focus:** Alternative shortest path algorithms
- **Theory (2 hrs):**
  - Bellman-Ford: handles negative weights
  - Floyd-Warshall: all-pairs shortest path
- **Practice (2 hrs):**
  - **Problems:**
    - 787 (Cheapest Flights - redo with Bellman-Ford)
    - 1334 (Find the City With Smallest Number of Neighbors)
- **Achievement:** Know when to use each shortest path algorithm

---

### **Day 21: Week 3 Review & Speed Practice**
**Key Focus:** Building speed
- **Timed Practice (4 hrs):** 
  - 6 problems, 40 minutes each
  - Mix of Union-Find and shortest path
- **Achievement:** Can solve medium problems in interview time

---

## Week 4: Advanced Topics & Interview Prep

### **Day 22: Backtracking on Graphs**
**Key Focus:** State restoration in search
- **Theory (1 hr):**
  - Difference from regular DFS
  - When to use backtracking
- **Practice (3 hrs):**
  - **Problems:**
    - 79 (Word Search) ⭐⭐⭐⭐
    - 980 (Unique Paths III) ⭐⭐⭐
    - 1066 (Campus Bikes II)
- **Achievement:** Master backtracking pattern

---

### **Day 23: Trie + Graph Hybrid**
**Key Focus:** Combining data structures
- **Theory (1 hr):**
  - Trie basics
  - Using Trie to optimize graph search
- **Practice (3 hrs):**
  - **Problems:**
    - 212 (Word Search II) ⭐⭐⭐⭐
    - 1233 (Remove Sub-Folders from Filesystem)
- **Achievement:** Can combine data structures effectively

---

### **Day 24: Advanced Grid Problems**
**Key Focus:** Complex constraints on grids
- **Practice (4 hrs):**
  - **Problems:**
    - 130 (Surrounded Regions) ⭐⭐⭐⭐
    - 417 (Pacific Atlantic Water Flow) ⭐⭐⭐⭐
    - 1559 (Detect Cycles in 2D Grid)
    - 1905 (Count Sub Islands)
- **Achievement:** Handle multi-constraint grid problems

---

### **Day 25: Matrix Problems & Traversals**
**Key Focus:** Advanced matrix manipulations
- **Practice (4 hrs):**
  - **Problems:**
    - 127 (Word Ladder) ⭐⭐⭐⭐
    - 433 (Minimum Genetic Mutation)
    - 752 (Open the Lock)
- **Achievement:** Master transformation graphs

---

### **Day 26: Minimum Spanning Tree**
**Key Focus:** Kruskal's and Prim's algorithms
- **Theory (2 hrs):**
  - MST concept
  - Kruskal's with Union-Find
  - Prim's with min-heap
- **Practice (2 hrs):**
  - **Problems:**
    - 1584 (Min Cost to Connect All Points) ⭐⭐⭐
    - 1135 (Connecting Cities With Minimum Cost)
- **Achievement:** Understand MST algorithms

---

### **Day 27: Graph Coloring & Advanced Patterns**
**Key Focus:** Less common but important patterns
- **Practice (4 hrs):**
  - **Problems:**
    - 332 (Reconstruct Itinerary) - Eulerian path
    - 1136 (Parallel Courses) - Critical path
    - 1436 (Destination City)
- **Achievement:** Broaden pattern recognition

---

### **Day 28: Mock Interview Day 1**
**Key Focus:** Simulated pressure
- **Format:** 
  - 2 problems, 45 minutes each
  - Explain approach out loud
  - Code in real environment
- **Problems:** Pick 1 medium, 1 medium-hard randomly
- **Achievement:** Practice interview conditions

---

### **Day 29: Mock Interview Day 2 & Weak Areas**
**Key Focus:** Targeted improvement
- **Morning (2 hrs):** Mock with 2 more problems
- **Afternoon (2 hrs):** Review mistakes, practice weak areas
- **Achievement:** Identify and fix gaps

---

### **Day 30: Final Review & Confidence Building**
**Key Focus:** Solidify knowledge
- **Morning (2 hrs):**
  - Review all templates
  - Quick redo of 5 key problems
- **Afternoon (2 hrs):**
  - Read problem statements only, identify patterns
  - Mental walkthrough of solutions
- **Achievement:** Ready for any graph problem Google throws at you!

---

# Summary Markdown

```markdown
## 30-Day Graph Mastery Summary

### Week 1: Foundations (Days 1-7)
- Graph representations & basics
- DFS (recursive/iterative) - Problems: 797, 547, 695
- BFS & shortest path - Problems: 1091, 542, 1162
- Grid DFS - Problems: 200, 463, 1254
- Multi-source BFS - Problems: 994, 286, 1765
- Graph cloning - Problems: 133, 138

**Milestone:** Master basic graph traversals

### Week 2: Cycles & Ordering (Days 8-14)
- Cycle detection (undirected) - Problems: 684, 261
- Cycle detection (directed) - Problems: 207, 802
- Topological sort (DFS) - Problems: 210, 1462
- Topological sort (Kahn's) - Problems: 210, 310, 2050
- Constraint graphs - Problems: 269, 1203
- Bipartite graphs - Problems: 785, 886

**Milestone:** Master topological sort & cycle detection

### Week 3: Union-Find & Shortest Paths (Days 15-21)
- Union-Find basics & applications - Problems: 323, 684, 547
- Advanced Union-Find - Problems: 721, 990, 1579
- Dijkstra's algorithm - Problems: 743, 787, 1631
- Bellman-Ford & Floyd-Warshall - Problems: 787, 1334

**Milestone:** Master connectivity & shortest paths

### Week 4: Advanced & Interview Prep (Days 22-30)
- Backtracking - Problems: 79, 980, 1066
- Trie + Graph - Problems: 212, 1233
- Complex grids - Problems: 130, 417, 1559, 1905
- Transformation graphs - Problems: 127, 433, 752
- MST algorithms - Problems: 1584, 1135
- Mock interviews & review

**Milestone:** Interview-ready confidence

## Daily Time Commitment
- **Weekdays:** 4 hours (2 hrs theory/review, 2 hrs practice)
- **Weekends:** 4-5 hours (deeper practice)

## Key Templates to Memorize
1. DFS (recursive & iterative)
2. BFS (with level tracking)
3. Union-Find (with path compression)
4. Dijkstra's algorithm
5. Topological sort (both methods)
6. Cycle detection (both graph types)

## Success Metrics
- ✅ Can recognize pattern in 30 seconds
- ✅ Can code solution in 30 minutes
- ✅ Can explain time/space complexity
- ✅ Can handle follow-up variations
```

---

## Pro Tips for Success

1. **Code by hand first** - Don't jump to IDE immediately
2. **Explain out loud** - Pretend you're in an interview
3. **Time yourself** - Build pressure tolerance
4. **Review mistakes thoroughly** - Understand why you got stuck
5. **Focus on patterns, not memorization** - Understand the "why"
6. **Do problems twice** - First attempt + redo after 3-5 days

**You've got this! 🚀 Consistent daily practice will make you graph problem master.**