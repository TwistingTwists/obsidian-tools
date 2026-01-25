---
tags:
  - leetcode
  - tree
  - traversal
---

# Tree Traversal Orders - Terminology

## TIL
The name tells you **where Root goes**:

| Order | Position | Pattern |
|-------|----------|---------|
| **Inorder** | Middle | Left → **Root** → Right |
| **Preorder** | First | **Root** → Left → Right |
| **Postorder** | Last | Left → Right → **Root** |

## Memory Hook
**"Pre" = before** (Root comes first)
**"Post" = after** (Root comes last)
**"In" = in the middle** (Root in between)

## Code Pattern
```python
# Preorder: process node, then children
def preorder(node):
    if not node: return
    process(node)        # ROOT first
    preorder(node.left)
    preorder(node.right)

# Inorder: left child, node, right child
def inorder(node):
    if not node: return
    inorder(node.left)
    process(node)        # ROOT middle
    inorder(node.right)

# Postorder: process children, then node
def postorder(node):
    if not node: return
    postorder(node.left)
    postorder(node.right)
    process(node)        # ROOT last
```

## Key Lesson
Don't memorize patterns—understand the rule: **"pre/in/post" = position of root in execution order**
