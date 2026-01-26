---
tags:
  - leetcode
  - dfs
  - backtracking
  - graph
  - logic
  - implementation
---

# LC 797: DFS Path Backtracking & Reference Copying

## Mistake
Three critical mental model gaps prevented correct DFS path enumeration: unnecessary `visited` set for DAGs, backtracking at wrong position, and appending list reference instead of copy.

## What Went Wrong

### Iteration 1: Using `visited` Set
❌ **Wrong**
```python
visited = set()
def dfs_traversal(source):
    if source not in visited:
        current_path.append(source)
        visited.add(source)
    if source == final_node:
        all_paths.append(current_path)
        val1 = current_path.pop()
        visited.remove(val1)
```

**Problem:** DAGs (Directed Acyclic Graphs) have no cycles. Marking nodes as visited prevents valid paths that revisit nodes from different source paths.

**Example:** Graph `[[1,2],[3],[3],[]]` has two valid paths: `0→1→3` and `0→2→3`. After first path marks 3 visited, second path gets blocked.

---

### Iteration 2: Wrong Backtracking Position
❌ **Wrong**
```python
def dfs_traversal(source):
    current_path.append(source)
    if source == final_node:
        all_paths.append(current_path)
    else:
        current_path.pop()  # ← Only pops if NOT target!
    for nbr in graph[source]:
        dfs_traversal(nbr)
```

**Problem:** Nodes only pop if they're NOT the target. Other nodes accumulate in `current_path` forever.

**Trace:** `dfs(0)` → `current_path=[0]` → `dfs(1)` → `current_path=[0,1]` → `dfs(3)` → saves, no pop → return to `dfs(1)` → `current_path` still `[0,1,3]` instead of `[0,1]`.

---

### Iteration 3: Missing Copy Before Save
❌ **Wrong**
```python
def dfs_traversal(source):
    current_path.append(source)
    if source == final_node:
        all_paths.append(current_path)  # ← Appends reference, not copy!
    for nbr in graph[source]:
        dfs_traversal(nbr)
    current_path.pop()
```

**Problem:** Lists are mutable references. All "saved" paths point to the SAME list object.

**Trace:**
- `current_path = [0,1,3]` → append to `all_paths` → `all_paths = [[0,1,3]]` (reference)
- Continue exploring → `current_path = [0,1,3,2,3]` → ALL entries in `all_paths` now show `[0,1,3,2,3]`

---

## Why

### 1. DAG vs Cycle Confusion
- **Cycles:** Visited set prevents infinite loops
- **DAGs:** No cycles exist → visited set only blocks valid paths
- **Key distinction:** For "find all paths," always check graph type first

### 2. DFS Backtracking Pattern
- Structure must be **symmetric:** Add before exploring, remove after exploring ALL children
- Backtracking MUST happen **outside the if/else**, after the for loop
- This ensures path is restored for sibling exploration

### 3. List Reference vs Copy
- Python lists are pass-by-reference
- `append(list)` saves pointer to same object, not snapshot
- Subsequent mutations affect ALL saved references
- Solution: Use `copy()`, `[:]` slice, or `list()` constructor

---

## Fix

✅ **Correct**
```python
def dfs_traversal(source):
    current_path.append(source)

    if source == final_node:
        all_paths.append(current_path.copy())  # ← Copy on save

    for nbr in graph[source]:
        dfs_traversal(nbr)

    current_path.pop()  # ← Backtrack AFTER all neighbors explored
```

**Key changes:**
1. **Removed** `visited` set entirely (DAG property)
2. **Moved** `pop()` outside loop (symmetric structure)
3. **Added** `.copy()` when saving path (snapshot, not reference)

---

## Key Lesson

**DFS with Path Tracking = Add → Recurse → Remove**

Pattern applies to any "find all paths/configurations" problem:
- **Add node** to current state
- **If terminal condition:** save **COPY** of state
- **Recurse** into children (path builds deeper)
- **Remove node** to restore state for siblings

Mental model test:
- "Will the same node appear in different valid paths?" → No `visited` set needed
- "Do I save the state?" → Always copy, never reference
- "When do I restore?" → After exploring all children, not just in base case

---

## Universal Backtracking Template

**Memorize this structure for ANY backtracking problem:**

```python
def backtrack(state):
    if is_solution(state):
        save_solution(state)
        return

    for choice in get_choices(state):
        make_choice(choice)      # Add/modify state
        backtrack(new_state)     # Explore deeper
        undo_choice(choice)      # Remove/restore (backtrack)
```

**Applied to LC 797:**
```python
def backtrack(node):
    current_path.append(node)           # make_choice: Add node

    if node == target:
        all_paths.append(current_path.copy())  # save_solution: Copy!
    else:
        for neighbor in graph[node]:
            backtrack(neighbor)         # Explore

    current_path.pop()                  # undo_choice: Restore state
```

**Pattern checklist:**
- ✓ Add state → Explore → Remove state (symmetric)
- ✓ Save happens INSIDE the function, restore happens OUTSIDE
- ✓ Always copy collections before saving
- ✓ Backtrack AFTER the loop/all children explored
