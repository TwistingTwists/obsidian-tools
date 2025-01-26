

```rust
use std::rc::Rc;
use std::cell::RefCell;
use std::collections::VecDeque;

// Definition for a binary tree node.
#[derive(Debug, PartialEq, Eq)]
pub struct TreeNode {
    pub val: i32,
    pub left: Option<Rc<RefCell<TreeNode>>>,
    pub right: Option<Rc<RefCell<TreeNode>>>,
}

impl TreeNode {
    #[inline]
    pub fn new(val: i32) -> Self {
        TreeNode {
            val,
            left: None,
            right: None,
        }
    }
}

impl Solution {
    pub fn level_order(root: Option<Rc<RefCell<TreeNode>>>) -> Vec<Vec<i32>> {
        let mut result = Vec::new();
        
        // If the tree is empty, return an empty result
        if root.is_none() {
            return result;
        }

        // Use a queue for level order traversal
        let mut queue = VecDeque::new();
        queue.push_back(root);
        
        while !queue.is_empty() {
            let mut level = Vec::new();
            let level_size = queue.len();

            // Process all nodes at the current level
            for _ in 0..level_size {
                if let Some(Some(node)) = queue.pop_front() {
                    let node_ref = node.borrow();
                    level.push(node_ref.val);

                    // Add left and right children to the queue if they exist
                    if let Some(left) = node_ref.left.clone() {
                        queue.push_back(Some(left));
                    }
                    if let Some(right) = node_ref.right.clone() {
                        queue.push_back(Some(right));
                    }
                }
            }

            // Add the current level to the result
            result.push(level);
        }

        result
    }
}

```

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        results = []
        queue = deque([root])
        while queue:
            level = []
            for _ in range(len(queue)):
                node = queue.popleft()
                if not node:
                    continue
                level.append(node.val)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            if level:
                results.append(level)
        return results
        
```


much simpmler solution 
```python
from collections import deque
from typing import List, Optional

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []

        results = []
        queue = deque([root])

        while queue:
            level_size = len(queue)
            level = []

            for _ in range(level_size):
                node = queue.popleft()
                level.append(node.val)

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            results.append(level)

        return results

```