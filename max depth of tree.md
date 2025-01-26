

Rust

```rust
// Definition for a binary tree node.
// #[derive(Debug, PartialEq, Eq)]
// pub struct TreeNode {
//   pub val: i32,
//   pub left: Option<Rc<RefCell<TreeNode>>>,
//   pub right: Option<Rc<RefCell<TreeNode>>>,
// }
// 
// impl TreeNode {
//   #[inline]
//   pub fn new(val: i32) -> Self {
//     TreeNode {
//       val,
//       left: None,
//       right: None
//     }
//   }
// }
use std::rc::Rc;
use std::cell::RefCell;
use std::cmp::max;

impl Solution {
    pub fn max_depth(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        match root{
            None => 0,
            Some(node)=>{
                let n = node.borrow();
                let max_left_depth = Solution::max_depth(n.left.clone());
                let max_right_depth = Solution::max_depth(n.right.clone());
                1 + max(max_left_depth , max_right_depth)
            }
        }
    }
}
```


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

