# Advanced Graph Patterns

## Pattern 1: Topological Sort (DAG)

### Kahn's Algorithm (BFS-based)
```python
from collections import deque

def topological_sort_bfs(n, edges):
    """
    Returns topological order or [] if cycle exists
    """
    graph = [[] for _ in range(n)]
    indegree = [0] * n
    
    # Build graph and calculate indegrees
    for u, v in edges:
        graph[u].append(v)
        indegree[v] += 1
    
    # Start with nodes having 0 indegree
    queue = deque([i for i in range(n) if indegree[i] == 0])
    result = []
    
    while queue:
        node = queue.popleft()
        result.append(node)
        
        for neighbor in graph[node]:
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                queue.append(neighbor)
    
    # If result length != n, there's a cycle
    return result if len(result) == n else []

# Example: Course Schedule
def can_finish(numCourses, prerequisites):
    """LeetCode 207"""
    return len(topological_sort_bfs(numCourses, prerequisites)) == numCourses
```

### DFS-based Topological Sort
```python
def topological_sort_dfs(n, edges):
    graph = [[] for _ in range(n)]
    for u, v in edges:
        graph[u].append(v)
    
    WHITE, GRAY, BLACK = 0, 1, 2
    color = [WHITE] * n
    result = []
    has_cycle = False
    
    def dfs(node):
        nonlocal has_cycle
        if color[node] == GRAY:
            has_cycle = True
            return
        if color[node] == BLACK:
            return
        
        color[node] = GRAY
        for neighbor in graph[node]:
            dfs(neighbor)
        
        color[node] = BLACK
        result.append(node)
    
    for i in range(n):
        if color[i] == WHITE:
            dfs(i)
    
    return [] if has_cycle else result[::-1]
```

## Pattern 2: Union-Find (Disjoint Set)

### Union-Find with Path Compression & Union by Rank
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
        self.components = n
    
    def find(self, x):
        """Find with path compression"""
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        """Union by rank"""
        root_x = self.find(x)
        root_y = self.find(y)
        
        if root_x == root_y:
            return False  # already connected
        
        # Union by rank
        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        elif self.rank[root_x] > self.rank[root_y]:
            self.parent[root_y] = root_x
        else:
            self.parent[root_y] = root_x
            self.rank[root_x] += 1
        
        self.components -= 1
        return True
    
    def connected(self, x, y):
        return self.find(x) == self.find(y)
    
    def count_components(self):
        return self.components

# Example: Number of Connected Components
def count_components(n, edges):
    uf = UnionFind(n)
    for u, v in edges:
        uf.union(u, v)
    return uf.count_components()

# Example: Redundant Connection
def find_redundant_connection(edges):
    """LeetCode 684"""
    uf = UnionFind(len(edges) + 1)
    for u, v in edges:
        if not uf.union(u, v):
            return [u, v]
    return []
```

## Pattern 3: Bipartite Graph Detection

### Two-Coloring with BFS
```python
from collections import deque

def is_bipartite_bfs(graph):
    n = len(graph)
    color = [-1] * n  # -1: uncolored, 0: color A, 1: color B
    
    for start in range(n):
        if color[start] != -1:
            continue
        
        queue = deque([start])
        color[start] = 0
        
        while queue:
            node = queue.popleft()
            
            for neighbor in graph[node]:
                if color[neighbor] == -1:
                    color[neighbor] = 1 - color[node]
                    queue.append(neighbor)
                elif color[neighbor] == color[node]:
                    return False  # odd cycle found
    
    return True
```

### Two-Coloring with DFS
```python
def is_bipartite_dfs(graph):
    n = len(graph)
    color = [-1] * n
    
    def dfs(node, c):
        color[node] = c
        for neighbor in graph[node]:
            if color[neighbor] == -1:
                if not dfs(neighbor, 1 - c):
                    return False
            elif color[neighbor] == c:
                return False
        return True
    
    for i in range(n):
        if color[i] == -1:
            if not dfs(i, 0):
                return False
    
    return True
```

## Pattern 4: Shortest Path Algorithms

### Dijkstra's Algorithm (Single Source, Non-negative Weights)
```python
import heapq

def dijkstra(graph, start):
    """
    graph: adjacency list with (neighbor, weight) tuples
    Returns: dict of shortest distances from start
    """
    n = len(graph)
    dist = [float('inf')] * n
    dist[start] = 0
    
    pq = [(0, start)]  # (distance, node)
    
    while pq:
        d, node = heapq.heappop(pq)
        
        if d > dist[node]:
            continue
        
        for neighbor, weight in graph[node]:
            new_dist = d + weight
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))
    
    return dist

# Example: Network Delay Time
def network_delay_time(times, n, k):
    """LeetCode 743"""
    graph = [[] for _ in range(n + 1)]
    for u, v, w in times:
        graph[u].append((v, w))
    
    dist = dijkstra(graph, k)
    max_dist = max(dist[1:])
    return max_dist if max_dist != float('inf') else -1
```

### Bellman-Ford (Handles Negative Weights)
```python
def bellman_ford(n, edges, start):
    """
    edges: list of (u, v, weight)
    Returns: distances or None if negative cycle exists
    """
    dist = [float('inf')] * n
    dist[start] = 0
    
    # Relax edges n-1 times
    for _ in range(n - 1):
        for u, v, w in edges:
            if dist[u] != float('inf') and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
    
    # Check for negative cycles
    for u, v, w in edges:
        if dist[u] != float('inf') and dist[u] + w < dist[v]:
            return None  # negative cycle exists
    
    return dist
```

### Floyd-Warshall (All Pairs Shortest Path)
```python
def floyd_warshall(n, edges):
    """Returns 2D array of shortest distances between all pairs"""
    dist = [[float('inf')] * n for _ in range(n)]
    
    # Initialize diagonal
    for i in range(n):
        dist[i][i] = 0
    
    # Add edges
    for u, v, w in edges:
        dist[u][v] = w
    
    # Floyd-Warshall
    for k in range(n):
        for i in range(n):
            for j in range(n):
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    
    return dist
```

## Pattern 5: Minimum Spanning Tree

### Kruskal's Algorithm (Union-Find)
```python
def kruskal(n, edges):
    """
    edges: list of (u, v, weight)
    Returns: list of edges in MST and total weight
    """
    edges.sort(key=lambda x: x[2])  # sort by weight
    uf = UnionFind(n)
    mst = []
    total_weight = 0
    
    for u, v, w in edges:
        if uf.union(u, v):
            mst.append((u, v, w))
            total_weight += w
            if len(mst) == n - 1:
                break
    
    return mst, total_weight

# Example: Min Cost to Connect All Points
def min_cost_connect_points(points):
    """LeetCode 1584"""
    n = len(points)
    
    # Generate all edges
    edges = []
    for i in range(n):
        for j in range(i + 1, n):
            dist = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1])
            edges.append((i, j, dist))
    
    _, total = kruskal(n, edges)
    return total
```

### Prim's Algorithm (Heap)
```python
def prim(graph, start=0):
    """
    graph: adjacency list with (neighbor, weight)
    Returns: total weight of MST
    """
    n = len(graph)
    visited = [False] * n
    pq = [(0, start)]  # (weight, node)
    total_weight = 0
    edges_added = 0
    
    while pq and edges_added < n:
        weight, node = heapq.heappop(pq)
        
        if visited[node]:
            continue
        
        visited[node] = True
        total_weight += weight
        edges_added += 1
        
        for neighbor, w in graph[node]:
            if not visited[neighbor]:
                heapq.heappush(pq, (w, neighbor))
    
    return total_weight if edges_added == n else -1
```

## Pattern 6: Graph Cloning

```python
class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []

def clone_graph(node):
    """LeetCode 133 - Clone Graph"""
    if not node:
        return None
    
    clones = {}  # old_node -> new_node
    
    def dfs(n):
        if n in clones:
            return clones[n]
        
        clone = Node(n.val)
        clones[n] = clone
        
        for neighbor in n.neighbors:
            clone.neighbors.append(dfs(neighbor))
        
        return clone
    
    return dfs(node)
```

## Pattern 7: Strongly Connected Components (Kosaraju's)

```python
def kosaraju_scc(graph):
    """
    Find all strongly connected components in directed graph
    """
    n = len(graph)
    
    # Step 1: Fill order by finish time (DFS)
    visited = [False] * n
    stack = []
    
    def dfs1(node):
        visited[node] = True
        for neighbor in graph[node]:
            if not visited[neighbor]:
                dfs1(neighbor)
        stack.append(node)
    
    for i in range(n):
        if not visited[i]:
            dfs1(i)
    
    # Step 2: Create reverse graph
    reverse_graph = [[] for _ in range(n)]
    for u in range(n):
        for v in graph[u]:
            reverse_graph[v].append(u)
    
    # Step 3: DFS on reverse graph in stack order
    visited = [False] * n
    sccs = []
    
    def dfs2(node, component):
        visited[node] = True
        component.append(node)
        for neighbor in reverse_graph[node]:
            if not visited[neighbor]:
                dfs2(neighbor, component)
    
    while stack:
        node = stack.pop()
        if not visited[node]:
            component = []
            dfs2(node, component)
            sccs.append(component)
    
    return sccs
```

## Pattern 8: Articulation Points & Bridges (Tarjan's)

### Find Bridges
```python
def find_bridges(n, edges):
    """Critical connections - LeetCode 1192"""
    graph = [[] for _ in range(n)]
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
    
    discovery = [-1] * n
    low = [-1] * n
    time = [0]
    bridges = []
    
    def dfs(node, parent):
        discovery[node] = low[node] = time[0]
        time[0] += 1
        
        for neighbor in graph[node]:
            if discovery[neighbor] == -1:
                dfs(neighbor, node)
                low[node] = min(low[node], low[neighbor])
                
                # Bridge condition
                if low[neighbor] > discovery[node]:
                    bridges.append([node, neighbor])
            elif neighbor != parent:
                low[node] = min(low[node], discovery[neighbor])
    
    for i in range(n):
        if discovery[i] == -1:
            dfs(i, -1)
    
    return bridges
```

### Find Articulation Points
```python
def find_articulation_points(n, edges):
    graph = [[] for _ in range(n)]
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)
    
    discovery = [-1] * n
    low = [-1] * n
    time = [0]
    articulation_points = set()
    
    def dfs(node, parent):
        children = 0
        discovery[node] = low[node] = time[0]
        time[0] += 1
        
        for neighbor in graph[node]:
            if discovery[neighbor] == -1:
                children += 1
                dfs(neighbor, node)
                low[node] = min(low[node], low[neighbor])
                
                # Articulation point conditions
                if parent == -1 and children > 1:
                    articulation_points.add(node)
                if parent != -1 and low[neighbor] >= discovery[node]:
                    articulation_points.add(node)
            elif neighbor != parent:
                low[node] = min(low[node], discovery[neighbor])
    
    for i in range(n):
        if discovery[i] == -1:
            dfs(i, -1)
    
    return list(articulation_points)
```

## Pattern 9: Euler Path/Circuit

```python
def find_euler_path(graph):
    """
    Hierholzer's algorithm for finding Eulerian path
    Works for both directed and undirected graphs
    """
    from collections import defaultdict
    
    # Calculate degrees
    in_degree = defaultdict(int)
    out_degree = defaultdict(int)
    
    for u in graph:
        out_degree[u] += len(graph[u])
        for v in graph[u]:
            in_degree[v] += 1
    
    # Find start node
    start = None
    for node in set(list(in_degree.keys()) + list(out_degree.keys())):
        if out_degree[node] - in_degree[node] == 1:
            start = node
            break
    if start is None:
        start = list(graph.keys())[0]
    
    # Hierholzer's algorithm
    stack = [start]
    path = []
    current_graph = {u: list(v) for u, v in graph.items()}
    
    while stack:
        curr = stack[-1]
        if curr in current_graph and current_graph[curr]:
            next_node = current_graph[curr].pop()
            stack.append(next_node)
        else:
            path.append(stack.pop())
    
    return path[::-1]
```

