---
tags:
  - graph
  - dfs
  - grid-traversal
  - connected-components
---

# LC 695: Max Area of Island (DFS Grid Pattern)

## Pattern
Connected component size calculation via DFS on 2D grid with directional exploration.

## Solution
```python
def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
    visited = set()

    def dfs(r, c):
        if r < 0 or r >= len(grid) or c < 0 or c >= len(grid[0]):
            return 0
        if (r, c) in visited or grid[r][c] == 0:
            return 0
        visited.add((r, c))
        return 1 + sum(dfs(r + rd, c + cd) for rd, cd in [(0,1), (1,0), (-1,0), (0,-1)])

    rows, cols, area = len(grid), len(grid[0]), 0
    for r in range(rows):
        for c in range(cols):
            if (r, c) not in visited:
                area = max(area, dfs(r, c))
    return area
```

## Key Points
- **Boundary Check First**: Out-of-bounds returns 0 immediately
- **Visited + Zero Check**: Skip already-visited or water cells
- **Accumulate Size**: Return 1 + sum of all 4 neighbors' sizes
- **4-Directional**: Right, down, up, left — exhaustive exploration
- **Max Tracking**: Store largest island area found

## Complexity
- **Time**: O(r × c) — each cell visited once
- **Space**: O(r × c) — visited set + recursion depth

## Why This Works
DFS from each unvisited land cell explores entire island, summing all connected 1s. Returning to outer loop with max() ensures largest island is captured. Each land cell counted exactly once.

## Contrast: LC 547
Same DFS pattern, different context (adjacency matrix vs. grid). Grid adds boundary checking; matrix implicitly handles it via loop bounds.
