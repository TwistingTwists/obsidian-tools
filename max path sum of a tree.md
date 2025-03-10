
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


----

### 1161 - max level sum 

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxLevelSum(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0

        maxLevel = 1 # <<<<<<<<<<<<<<<<<<<<<< the first level is the default one
        mls  = root.val  # <<<<<<<<<<<<<<<<<<<<<< so, the sum takes the first value

        level_so_far  = 0 
        queue = deque([root])
        while len(queue) >0:
            level_so_far += 1
            level_size = len(queue)
            level_sum = 0

            for _ in range(level_size):
                node = queue.popleft()
                # if node.val < 0:
                level_sum += node.val

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            if level_sum > mls :
                maxLevel = level_so_far
                mls = level_sum
        return maxLevel

```