

#### Using nonlocal 


```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def diameter_of_binary_tree(root):
    """
    Calculate the diameter of a binary tree.

    :param root: TreeNode, root of the binary tree
    :return: int, diameter of the binary tree
    """
    # Initialize a variable to store the diameter
    diameter = 0

    def dfs(node):
        nonlocal diameter
        if not node:
            return 0

        # Recursively find the height of left and right subtrees
        left_height = dfs(node.left)
        right_height = dfs(node.right)

        # Update the diameter if the path through the current node is larger
        diameter = max(diameter, left_height + right_height)

        # Return the height of the current node
        return max(left_height, right_height) + 1

    dfs(root)
    return diameter

# Example Usage
# Construct a sample binary tree
#         1
#        / \
#       2   3
#      / \
#     4   5
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)

# Calculate the diameter
print("Diameter of the binary tree:", diameter_of_binary_tree(root))

```


### Without using nonlocal 

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def diameter_of_binary_tree(root):
    """
    Calculate the diameter of a binary tree without using a nonlocal variable.

    :param root: TreeNode, root of the binary tree
    :return: int, diameter of the binary tree
    """
    def dfs(node):
        if not node:
            return 0, 0  # (height, diameter)

        # Recursively calculate height and diameter for left and right subtrees
        left_height, left_diameter = dfs(node.left)
        right_height, right_diameter = dfs(node.right)

        # Calculate the height of the current node
        current_height = max(left_height, right_height) + 1

        # Calculate the diameter through the current node
        current_diameter = max(left_diameter, right_diameter, left_height + right_height)

        return current_height, current_diameter

    # Only the diameter is needed from the result of dfs
    return dfs(root)[1]

# Example Usage
# Construct a sample binary tree
#         1
#        / \
#       2   3
#      / \
#     4   5
root = TreeNode(1)
root.left = TreeNode(2)
root.right = TreeNode(3)
root.left.left = TreeNode(4)
root.left.right = TreeNode(5)

# Calculate the diameter
print("Diameter of the binary tree:", diameter_of_binary_tree(root))

```