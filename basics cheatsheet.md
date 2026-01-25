# Graph Basics - Complete Guide

## Graph Representation

### 1. Adjacency List (Most Common)
```python
# Using Dictionary
graph = {
    0: [1, 2],
    1: [0, 2, 3],
    2: [0, 1],
    3: [1]
}

# Using List of Lists (when nodes are 0 to n-1)
graph = [
    [1, 2],      # neighbors of node 0
    [0, 2, 3],   # neighbors of node 1
    [0, 1],      # neighbors of node 2
    [1]          # neighbors of node 3
]
```

### 2. Adjacency Matrix
```python
# For n nodes
n = 4
graph = [[0] * n for _ in range(n)]

# Add edge between node 0 and 1
graph[0][1] = 1
graph[1][0] = 1  # for undirected graph

# Result:
# [[0, 1, 1, 0],
#  [1, 0, 1, 1],
#  [1, 1, 0, 0],
#  [0, 1, 0, 0]]
```

### 3. Edge List
```python
# List of tuples/lists representing edges
edges = [(0, 1), (0, 2), (1, 2), (1, 3)]

# Weighted edges
weighted_edges = [(0, 1, 5), (0, 2, 3), (1, 3, 2)]  # (from, to, weight)
```

## Building Graphs from Different Inputs

### From Edge List to Adjacency List
```python
def build_graph(n, edges, directed=False):
    """
    n: number of nodes (0 to n-1)
    edges: list of [u, v] or (u, v)
    directed: whether graph is directed
    """
    graph = [[] for _ in range(n)]
    
    for u, v in edges:
        graph[u].append(v)
        if not directed:
            graph[v].append(u)
    
    return graph

# Example
edges = [[0, 1], [0, 2], [1, 3]]
graph = build_graph(4, edges)
# Result: [[1, 2], [0, 3], [0], [1]]
```

### From Edge List to Dictionary (for non-numeric nodes)
```python
def build_graph_dict(edges, directed=False):
    graph = {}
    
    for u, v in edges:
        if u not in graph:
            graph[u] = []
        if v not in graph:
            graph[v] = []
        
        graph[u].append(v)
        if not directed:
            graph[v].append(u)
    
    return graph

# Example
edges = [["A", "B"], ["A", "C"], ["B", "D"]]
graph = build_graph_dict(edges)
# Result: {'A': ['B', 'C'], 'B': ['A', 'D'], 'C': ['A'], 'D': ['B']}
```

### Weighted Graph
```python
def build_weighted_graph(n, edges, directed=False):
    """edges: list of [u, v, weight]"""
    graph = [[] for _ in range(n)]
    
    for u, v, weight in edges:
        graph[u].append((v, weight))
        if not directed:
            graph[v].append((u, weight))
    
    return graph

# Example
edges = [[0, 1, 5], [0, 2, 3], [1, 3, 2]]
graph = build_weighted_graph(4, edges)
# Result: [[(1, 5), (2, 3)], [(0, 5), (3, 2)], [(0, 3)], [(1, 2)]]
```

## Graph Traversal

### DFS (Depth-First Search)

#### Recursive DFS
```python
def dfs_recursive(graph, node, visited=None):
    if visited is None:
        visited = set()
    
    visited.add(node)
    print(node, end=' ')
    
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)
    
    return visited
```

#### Iterative DFS (using stack)
```python
def dfs_iterative(graph, start):
    visited = set()
    stack = [start]
    result = []
    
    while stack:
        node = stack.pop()
        
        if node not in visited:
            visited.add(node)
            result.append(node)
            
            # Add neighbors in reverse order to match recursive DFS
            for neighbor in reversed(graph[node]):
                if neighbor not in visited:
                    stack.append(neighbor)
    
    return result
```

#### DFS with Path Tracking
```python
def dfs_with_path(graph, start, target):
    visited = set()
    path = []
    
    def dfs(node):
        if node in visited:
            return False
        
        visited.add(node)
        path.append(node)
        
        if node == target:
            return True
        
        for neighbor in graph[node]:
            if dfs(neighbor):
                return True
        
        path.pop()  # backtrack
        return False
    
    dfs(start)
    return path
```

### BFS (Breadth-First Search)

#### Basic BFS
```python
from collections import deque

def bfs(graph, start):
    visited = set([start])
    queue = deque([start])
    result = []
    
    while queue:
        node = queue.popleft()
        result.append(node)
        
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    
    return result
```

#### BFS with Level Tracking
```python
def bfs_with_levels(graph, start):
    visited = set([start])
    queue = deque([(start, 0)])  # (node, level)
    levels = {}
    
    while queue:
        node, level = queue.popleft()
        levels[node] = level
        
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, level + 1))
    
    return levels
```

#### BFS Shortest Path
```python
def bfs_shortest_path(graph, start, target):
    if start == target:
        return [start]
    
    visited = set([start])
    queue = deque([(start, [start])])  # (node, path)
    
    while queue:
        node, path = queue.popleft()
        
        for neighbor in graph[node]:
            if neighbor == target:
                return path + [neighbor]
            
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, path + [neighbor]))
    
    return []  # no path found
```

## Common Graph Algorithms

### Check if Graph is Connected
```python
def is_connected(graph):
    if not graph:
        return True
    
    # Start from any node (0 in this case)
    visited = dfs_recursive(graph, 0)
    return len(visited) == len(graph)
```

### Count Connected Components
```python
def count_components(n, edges):
    graph = build_graph(n, edges)
    visited = set()
    count = 0
    
    for node in range(n):
        if node not in visited:
            dfs_recursive(graph, node, visited)
            count += 1
    
    return count
```

### Detect Cycle in Undirected Graph
```python
def has_cycle_undirected(graph):
    visited = set()
    
    def dfs(node, parent):
        visited.add(node)
        
        for neighbor in graph[node]:
            if neighbor not in visited:
                if dfs(neighbor, node):
                    return True
            elif neighbor != parent:
                return True  # back edge found
        
        return False
    
    for node in range(len(graph)):
        if node not in visited:
            if dfs(node, -1):
                return True
    
    return False
```

### Detect Cycle in Directed Graph
```python
def has_cycle_directed(graph):
    WHITE, GRAY, BLACK = 0, 1, 2
    color = [WHITE] * len(graph)
    
    def dfs(node):
        color[node] = GRAY
        
        for neighbor in graph[node]:
            if color[neighbor] == GRAY:
                return True  # back edge
            if color[neighbor] == WHITE:
                if dfs(neighbor):
                    return True
        
        color[node] = BLACK
        return False
    
    for node in range(len(graph)):
        if color[node] == WHITE:
            if dfs(node):
                return True
    
    return False
```

### Find All Paths Between Two Nodes
```python
def find_all_paths(graph, start, target):
    all_paths = []
    
    def dfs(node, path):
        if node == target:
            all_paths.append(path[:])
            return
        
        for neighbor in graph[node]:
            if neighbor not in path:  # avoid cycles
                path.append(neighbor)
                dfs(neighbor, path)
                path.pop()
    
    dfs(start, [start])
    return all_paths
```

## Example Usage

```python
# Create a sample graph
#     0 --- 1
#     |     |
#     2 --- 3

edges = [[0, 1], [0, 2], [1, 3], [2, 3]]
graph = build_graph(4, edges)

print("Graph:", graph)
# [[1, 2], [0, 3], [0, 3], [1, 2]]

print("\nDFS from 0:", dfs_recursive(graph, 0))
# 0 1 3 2

print("\nBFS from 0:", bfs(graph, 0))
# [0, 1, 2, 3]

print("\nShortest path 0->3:", bfs_shortest_path(graph, 0, 3))
# [0, 1, 3] or [0, 2, 3]

print("\nAll paths 0->3:", find_all_paths(graph, 0, 3))
# [[0, 1, 3], [0, 2, 3]]

print("\nConnected components:", count_components(4, edges))
# 1

print("\nHas cycle:", has_cycle_undirected(graph))
# True
```

## Space & Time Complexity

| Operation | Adjacency List | Adjacency Matrix |
|-----------|---------------|------------------|
| Space | O(V + E) | O(V²) |
| Add Edge | O(1) | O(1) |
| Check Edge | O(degree) | O(1) |
| DFS/BFS | O(V + E) | O(V²) |

Where V = vertices, E = edges

---

This covers the fundamentals! Ready to explore graph patterns?