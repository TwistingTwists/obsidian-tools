---
tags:
  - tree
  - traversal
  - stack
  - implementation
  - leetcode
---

# Postorder Traversal: Missing Base Case & Stack Initialization

## Mistake
Base case check missing; stack1 initialized with root without null guard—adapted from preorder without proper modification.

## What Went Wrong

❌ **Original buggy code:**
```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        results = []
        stack1 = [root]  # No null check before adding
        stack2 = []
        # ... rest of logic
```

**Issues:**
1. No `if not root:` check → attempts to push None into stack1
2. Stack1 initialized with root unconditionally → breaks on None input

## Why
Copied preorder traversal pattern but removed the base case guard. Preorder/inorder/postorder all need early exit for empty trees—this isn't optional when using iterative two-stack approach.

## Fix

✅ **Corrected code:**
```python
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if not root:  # Base case guard
            return []

        results = []
        stack1 = [root]  # Safe to initialize now
        stack2 = []

        while stack1:
            node = stack1.pop()
            stack2.append(node)
            if node.left:
                stack1.append(node.left)
            if node.right:
                stack1.append(node.right)

        while stack2:
            node = stack2.pop()
            results.append(node.val)
        return results
```

## Key Lesson
**Boundary guards before collection initialization:** Always check for None/empty input before pushing into stacks or queues. Pattern adaptations (copying preorder→postorder) need full guard restoration, not partial refactoring.
