
#### python impl 


below solutions works if tree has only positive numbers.

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def max_height_of_tree(root):
	# maximum path sum
	maxi = float('-inf')  # caution
	
    def dfs(node):
	    nonlocal max_path_sum
        if not node:
            return 0

        # Recursively find the height of left and right subtrees
        left_sum = dfs(node.left)
        right_sum = dfs(node.right)
	    
	    # maximum sum update 
	    # variation
	    max_path_sum = max(max_path_sum, left_sum + right_sum + node.val)

        # Return the height of the current node
        # variation
        return max(right_sum, right_sum) + node.val

    max_height = dfs(root)
    return max_height

```

below solutions works if tree has both + and negative numbers:

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def max_height_of_tree(root):
	# maximum path sum
	maxi = float('-inf')  # caution
	
    def dfs(node):
	    nonlocal max_path_sum
        if not node:
            return 0

        # Recursively find the height of left and right subtrees
        left_sum = max(0,dfs(node.left))
        right_sum = max(0,dfs(node.right))
	    
	    # maximum sum update 
	    # variation
	    max_path_sum = max(max_path_sum, left_sum + right_sum + node.val)

        # Return the height of the current node
        # variation
        return max(right_sum, right_sum) + node.val

    max_height = dfs(root)
    return max_height

```

