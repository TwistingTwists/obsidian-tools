# DFS Tree Traversals in Python

Here are the three main depth-first traversals with examples:

## 1. Inorder Traversal (Left → Root → Right)

```python
def inorder(root):
    if not root:
        return []
    
    result = []
    result.extend(inorder(root.left))
    result.append(root.val)
    result.extend(inorder(root.right))
    return result

# Iterative version using stack
def inorder_iterative(root):
    result = []
    stack = []
    curr = root
    
    while curr or stack:
        while curr:
            stack.append(curr)
            curr = curr.left
        
        curr = stack.pop()
        result.append(curr.val)
        curr = curr.right
    
    return result
```

## 2. Preorder Traversal (Root → Left → Right)

```python
def preorder(root):
    if not root:
        return []
    
    result = [root.val]
    result.extend(preorder(root.left))
    result.extend(preorder(root.right))
    return result

# Iterative version using stack
def preorder_iterative(root):
    if not root:
        return []
    
    result = []
    stack = [root]
    
    while stack:
        node = stack.pop()
        result.append(node.val)
        
        # Push right first so left is processed first
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)
    
    return result
```

## 3. Postorder Traversal (Left → Right → Root)

```python
def postorder(root):
    if not root:
        return []
    
    result = []
    result.extend(postorder(root.left))
    result.extend(postorder(root.right))
    result.append(root.val)
    return result

# Iterative version using two stacks
def postorder_iterative(root):
    if not root:
        return []
    
    result = []
    stack1 = [root]
    stack2 = []
    
    while stack1:
        node = stack1.pop()
        stack2.append(node)
        
        if node.left:
            stack1.append(node.left)
        if node.right:
            stack1.append(node.right)
    
    while stack2:
        result.append(stack2.pop().val)
    
    return result
```

## Example Usage

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

# Create tree:    1
#                / \
#               2   3
#              / \
#             4   5

root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)

print(inorder(root))     # [4, 2, 5, 1, 3]
print(preorder(root))    # [1, 2, 4, 5, 3]
print(postorder(root))   # [4, 5, 2, 3, 1]
```

**Key differences:**
- **Inorder** produces sorted output for BSTs
- **Preorder** useful for copying trees or prefix notation
- **Postorder** useful for deleting trees or postfix notation