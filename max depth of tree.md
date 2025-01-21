
#### python impl 


```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def max_height_of_tree(root):
    
    def dfs(node):
        if not node:
            return 0

        # Recursively find the height of left and right subtrees
        left_height = dfs(node.left)
        right_height = dfs(node.right)

        # Return the height of the current node
        return max(left_height, right_height) + 1

    max_height = dfs(root)
    return max_height

```

