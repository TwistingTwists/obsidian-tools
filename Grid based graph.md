# Grid-Based Graph Problems

Grids are 2D arrays where each cell is a node, and edges connect adjacent cells. These are extremely common in interviews!

## Pattern 1: Grid Traversal Basics

### 4-Directional Movement
```python
def get_neighbors_4(grid, row, col):
    """Returns valid 4-directional neighbors"""
    rows, cols = len(grid), len(grid[0])
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # right, down, left, up
    neighbors = []
    
    for dr, dc in directions:
        new_row, new_col = row + dr, col + dc
        if 0 <= new_row < rows and 0 <= new_col < cols:
            neighbors.append((new_row, new_col))
    
    return neighbors

# Alternative: inline version (most common)
def dfs_grid(grid, row, col):
    rows, cols = len(grid), len(grid[0])
    
    for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
        new_row, new_col = row + dr, col + dc
        if 0 <= new_row < rows and 0 <= new_col < cols:
            # process neighbor
            pass
```

### 8-Directional Movement
```python
def get_neighbors_8(grid, row, col):
    """Returns valid 8-directional neighbors (including diagonals)"""
    rows, cols = len(grid), len(grid[0])
    directions = [
        (-1, -1), (-1, 0), (-1, 1),
        (0, -1),           (0, 1),
        (1, -1),  (1, 0),  (1, 1)
    ]
    neighbors = []
    
    for dr, dc in directions:
        new_row, new_col = row + dr, col + dc
        if 0 <= new_row < rows and 0 <= new_col < cols:
            neighbors.append((new_row, new_col))
    
    return neighbors
```

## Pattern 2: Island Problems (DFS/BFS)

### Number of Islands
```python
def num_islands(grid):
    """LeetCode 200 - Count islands (1s) surrounded by water (0s)"""
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    visited = set()
    count = 0
    
    def dfs(r, c):
        if (r < 0 or r >= rows or c < 0 or c >= cols or
            (r, c) in visited or grid[r][c] == '0'):
            return
        
        visited.add((r, c))
        
        # Visit all 4 directions
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1' and (r, c) not in visited:
                dfs(r, c)
                count += 1
    
    return count

# Alternative: Modify grid in-place (no visited set needed)
def num_islands_inplace(grid):
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    count = 0
    
    def dfs(r, c):
        if (r < 0 or r >= rows or c < 0 or c >= cols or 
            grid[r][c] != '1'):
            return
        
        grid[r][c] = '#'  # mark as visited
        
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1':
                dfs(r, c)
                count += 1
    
    return count
```

### Max Area of Island
```python
def max_area_of_island(grid):
    """LeetCode 695"""
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    max_area = 0
    
    def dfs(r, c):
        if (r < 0 or r >= rows or c < 0 or c >= cols or 
            grid[r][c] != 1):
            return 0
        
        grid[r][c] = 0  # mark as visited
        
        area = 1
        area += dfs(r + 1, c)
        area += dfs(r - 1, c)
        area += dfs(r, c + 1)
        area += dfs(r, c - 1)
        
        return area
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 1:
                max_area = max(max_area, dfs(r, c))
    
    return max_area
```

### Surrounded Regions
```python
def solve(board):
    """LeetCode 130 - Capture surrounded regions"""
    if not board:
        return
    
    rows, cols = len(board), len(board[0])
    
    def dfs(r, c):
        if (r < 0 or r >= rows or c < 0 or c >= cols or 
            board[r][c] != 'O'):
            return
        
        board[r][c] = 'T'  # temporary mark
        
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)
    
    # Mark 'O's on borders and their connected 'O's
    for r in range(rows):
        dfs(r, 0)
        dfs(r, cols - 1)
    
    for c in range(cols):
        dfs(0, c)
        dfs(rows - 1, c)
    
    # Convert remaining 'O's to 'X', and 'T's back to 'O'
    for r in range(rows):
        for c in range(cols):
            if board[r][c] == 'O':
                board[r][c] = 'X'
            elif board[r][c] == 'T':
                board[r][c] = 'O'
```

## Pattern 3: Shortest Path in Grid (BFS)

### Shortest Path in Binary Matrix
```python
from collections import deque

def shortest_path_binary_matrix(grid):
    """LeetCode 1091"""
    n = len(grid)
    
    if grid[0][0] == 1 or grid[n-1][n-1] == 1:
        return -1
    
    if n == 1:
        return 1
    
    queue = deque([(0, 0, 1)])  # (row, col, distance)
    visited = {(0, 0)}
    
    directions = [(-1,-1), (-1,0), (-1,1), 
                  (0,-1),          (0,1),
                  (1,-1),  (1,0),  (1,1)]
    
    while queue:
        r, c, dist = queue.popleft()
        
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            
            if nr == n-1 and nc == n-1:
                return dist + 1
            
            if (0 <= nr < n and 0 <= nc < n and 
                grid[nr][nc] == 0 and (nr, nc) not in visited):
                visited.add((nr, nc))
                queue.append((nr, nc, dist + 1))
    
    return -1
```

### Rotting Oranges
```python
def oranges_rotting(grid):
    """LeetCode 994"""
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh = 0
    
    # Find all rotten oranges and count fresh ones
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c, 0))  # (row, col, time)
            elif grid[r][c] == 1:
                fresh += 1
    
    if fresh == 0:
        return 0
    
    max_time = 0
    
    while queue:
        r, c, time = queue.popleft()
        max_time = max(max_time, time)
        
        for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
            nr, nc = r + dr, c + dc
            
            if (0 <= nr < rows and 0 <= nc < cols and 
                grid[nr][nc] == 1):
                grid[nr][nc] = 2
                fresh -= 1
                queue.append((nr, nc, time + 1))
    
    return max_time if fresh == 0 else -1
```

### Walls and Gates
```python
def walls_and_gates(rooms):
    """LeetCode 286 - Fill empty rooms with distance to nearest gate"""
    if not rooms:
        return
    
    rows, cols = len(rooms), len(rooms[0])
    queue = deque()
    
    # Find all gates
    for r in range(rows):
        for c in range(cols):
            if rooms[r][c] == 0:
                queue.append((r, c))
    
    # Multi-source BFS
    while queue:
        r, c = queue.popleft()
        
        for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
            nr, nc = r + dr, c + dc
            
            if (0 <= nr < rows and 0 <= nc < cols and 
                rooms[nr][nc] == 2147483647):  # INF
                rooms[nr][nc] = rooms[r][c] + 1
                queue.append((nr, nc))
```

## Pattern 4: Matrix Transformation

### Flood Fill
```python
def flood_fill(image, sr, sc, color):
    """LeetCode 733"""
    original_color = image[sr][sc]
    
    if original_color == color:
        return image
    
    rows, cols = len(image), len(image[0])
    
    def dfs(r, c):
        if (r < 0 or r >= rows or c < 0 or c >= cols or 
            image[r][c] != original_color):
            return
        
        image[r][c] = color
        
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)
    
    dfs(sr, sc)
    return image
```

### 01 Matrix (Distance to Nearest 0)
```python
def update_matrix(mat):
    """LeetCode 542"""
    rows, cols = len(mat), len(mat[0])
    queue = deque()
    
    # Initialize: add all 0s to queue, mark 1s as unvisited
    for r in range(rows):
        for c in range(cols):
            if mat[r][c] == 0:
                queue.append((r, c))
            else:
                mat[r][c] = -1  # mark as unvisited
    
    # Multi-source BFS from all 0s
    while queue:
        r, c = queue.popleft()
        
        for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
            nr, nc = r + dr, c + dc
            
            if (0 <= nr < rows and 0 <= nc < cols and 
                mat[nr][nc] == -1):
                mat[nr][nc] = mat[r][c] + 1
                queue.append((nr, nc))
    
    return mat
```

## Pattern 5: Word Search & Backtracking

### Word Search
```python
def exist(board, word):
    """LeetCode 79"""
    rows, cols = len(board), len(board[0])
    
    def backtrack(r, c, index):
        if index == len(word):
            return True
        
        if (r < 0 or r >= rows or c < 0 or c >= cols or 
            board[r][c] != word[index]):
            return False
        
        # Mark as visited
        temp = board[r][c]
        board[r][c] = '#'
        
        # Explore all 4 directions
        found = (backtrack(r + 1, c, index + 1) or
                 backtrack(r - 1, c, index + 1) or
                 backtrack(r, c + 1, index + 1) or
                 backtrack(r, c - 1, index + 1))
        
        # Restore
        board[r][c] = temp
        
        return found
    
    for r in range(rows):
        for c in range(cols):
            if board[r][c] == word[0]:
                if backtrack(r, c, 0):
                    return True
    
    return False
```

### Word Search II (Trie + DFS)
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.word = None

def find_words(board, words):
    """LeetCode 212"""
    # Build Trie
    root = TrieNode()
    for word in words:
        node = root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.word = word
    
    rows, cols = len(board), len(board[0])
    result = []
    
    def dfs(r, c, node):
        char = board[r][c]
        
        if char not in node.children:
            return
        
        next_node = node.children[char]
        
        # Found a word
        if next_node.word:
            result.append(next_node.word)
            next_node.word = None  # avoid duplicates
        
        # Mark as visited
        board[r][c] = '#'
        
        # Explore neighbors
        for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
            nr, nc = r + dr, c + dc
            if 0 <= nr < rows and 0 <= nc < cols and board[nr][nc] != '#':
                dfs(nr, nc, next_node)
        
        # Restore
        board[r][c] = char
        
        # Optimization: remove leaf nodes
        if not next_node.children:
            del node.children[char]
    
    for r in range(rows):
        for c in range(cols):
            dfs(r, c, root)
    
    return result
```

## Pattern 6: Advanced Grid Problems

### Pacific Atlantic Water Flow
```python
def pacific_atlantic(heights):
    """LeetCode 417"""
    if not heights:
        return []
    
    rows, cols = len(heights), len(heights[0])
    pacific = set()
    atlantic = set()
    
    def dfs(r, c, visited):
        visited.add((r, c))
        
        for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
            nr, nc = r + dr, c + dc
            if (0 <= nr < rows and 0 <= nc < cols and 
                (nr, nc) not in visited and 
                heights[nr][nc] >= heights[r][c]):
                dfs(nr, nc, visited)
    
    # DFS from Pacific (top and left edges)
    for c in range(cols):
        dfs(0, c, pacific)
        dfs(rows - 1, c, atlantic)
    
    for r in range(rows):
        dfs(r, 0, pacific)
        dfs(r, cols - 1, atlantic)
    
    return list(pacific & atlantic)
```

### Shortest Bridge
```python
def shortest_bridge(grid):
    """LeetCode 934"""
    n = len(grid)
    
    def dfs(r, c, island):
        if (r < 0 or r >= n or c < 0 or c >= n or 
            grid[r][c] != 1):
            return
        
        grid[r][c] = 2
        island.append((r, c))
        
        dfs(r + 1, c, island)
        dfs(r - 1, c, island)
        dfs(r, c + 1, island)
        dfs(r, c - 1, island)
    
    # Find first island
    island1 = []
    found = False
    for r in range(n):
        if found:
            break
        for c in range(n):
            if grid[r][c] == 1:
                dfs(r, c, island1)
                found = True
                break
    
    # BFS to find shortest bridge
    queue = deque([(r, c, 0) for r, c in island1])
    
    while queue:
        r, c, dist = queue.popleft()
        
        for dr, dc in [(0,1), (1,0), (0,-1), (-1,0)]:
            nr, nc = r + dr, c + dc
            
            if 0 <= nr < n and 0 <= nc < n:
                if grid[nr][nc] == 1:
                    return dist
                if grid[nr][nc] == 0:
                    grid[nr][nc] = 2
                    queue.append((nr, nc, dist + 1))
    
    return -1
```

### Number of Distinct Islands
```python
def num_distinct_islands(grid):
    """LeetCode 694"""
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    islands = set()
    
    def dfs(r, c, r0, c0, shape):
        if (r < 0 or r >= rows or c < 0 or c >= cols or 
            grid[r][c] != 1):
            return
        
        grid[r][c] = 0
        shape.append((r - r0, c - c0))  # relative coordinates
        
        dfs(r + 1, c, r0, c0, shape)
        dfs(r - 1, c, r0, c0, shape)
        dfs(r, c + 1, r0, c0, shape)
        dfs(r, c - 1, r0, c0, shape)
    
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 1:
                shape = []
                dfs(r, c, r, c, shape)
                islands.add(tuple(sorted(shape)))
    
    return len(islands)
```

## Common Grid Patterns Summary

| Pattern | Algorithm | Typical Use Case |
|---------|-----------|------------------|
| Count components | DFS/BFS | Number of islands |
| Find max area | DFS with sum | Max area of island |
| Shortest path | BFS | Minimum steps/distance |
| Multi-source BFS | BFS from multiple starts | Rotting oranges, 01 Matrix |
| Backtracking | DFS with restore | Word search |
| Trie + DFS | Optimize multiple searches | Word search II |
| Border DFS | Mark from edges | Surrounded regions |

Ready for practice problems or want to see specific advanced topics (like A* search, game theory on grids)?