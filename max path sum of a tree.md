
#### python impl 


```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def max_height_of_tree(root):
	# maximum path sum
	max_path_sum = 0
    
    def dfs(node):
	    nonlocal max_path_sum
        if not node:
            return 0

        # Recursively find the height of left and right subtrees
        left_sum = dfs(node.left)
        right_sum = dfs(node.right)
	    
	    # maximum sum update
	    max_path_sum = max(max_path_sum, left_height)

        # Return the height of the current node
        return max(right_sum, right_sum) + 1

    max_height = dfs(root)
    return max_height

```

