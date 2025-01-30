```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def recoverTree(self, root: Optional[TreeNode]) -> None:
        """
        Do not return anything, modify root in-place instead.
        """
        swapped = last_processed = None

        def find_swap_pair(root: Optional[TreeNode]):
            nonlocal swapped, last_processed
            if not root:
                return
            
            find_swap_pair(root.left)
            # check None condition
            if last_processed:
                if last_processed.val > root.val:
                    if not swapped:
                        swapped = [last_processed, root]
                    else:
                        swapped[1] = root
            last_processed = root

            find_swap_pair(root.right)
        
        find_swap_pair(root)
        # catch - swap the `.val` not the nodes themselves
        swapped[0].val, swapped[1].val = swapped[1].val, swapped[0].val
```