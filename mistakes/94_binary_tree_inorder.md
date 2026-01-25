---
tags:
  - leetcode
---

# Problem 94: Binary Tree Inorder Traversal

## Mistake
Appending return values from recursive calls instead of directly modifying results list.

## What Went Wrong
```python
# ❌ WRONG
results.append(dfs_traversal(root.left))  # Appends [] or None
results.append(root.val)
results.append(dfs_traversal(root.right)) # Appends [] or None
```

Output: `[[], 1, [], 3, [], None, 2, [], None]` ← nested lists + None values

## Why
- `dfs_traversal()` returns `[]` when root is None
- `dfs_traversal()` returns `None` implicitly (no explicit return)
- Appending these return values creates nested structures

## Fix
```python
# ✅ CORRECT
def dfs_traversal(node):
    if not node:
        return
    dfs_traversal(node.left)
    results.append(node.val)  # Directly append to outer results
    dfs_traversal(node.right)
```

## Key Lesson
In recursive traversal with shared state (results list), don't append recursive calls' returns. Instead:
- Pass the list through closure
- Modify it directly in base case
- Let recursion handle the traversal logic

---

## Mistake 2: Iterative Approach - Variable Mismatch
Popped node into wrong variable, then accessed stale `current` instead.

## What Went Wrong
```python
# ❌ WRONG
node = stack.pop()
results.append(node.val)
current = current.right  # BUG: current is stale, not the popped node
```

Skips right subtrees because `current` hasn't been updated to the popped node.

## Why
The variable you pop into must match the variable you use for the next state transition. Here, `node` received the pop, but `current.right` was referenced—accessing an outdated `current` from before the pop.

## Fix
```python
# ✅ CORRECT
current = stack.pop()
results.append(current.val)
current = current.right  # NOW current is the popped node
```

## Key Lesson
In iterative traversals with stacks: **popped variable = next state variable**. Mismatched names silently break logic—code runs but visits wrong nodes since there's no syntax error.
