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
impl Solution {
    pub fn is_valid_bst(root: Option<Rc<RefCell<TreeNode>>>) -> bool {
       Solution::validate(&root, None, None)
    }

    pub fn validate(root: &Option<Rc<RefCell<TreeNode>>>, lower: Option<i32>, upper: Option<i32>) -> bool {
        if let Some(node) = root{
            let n = node.borrow();
            let val = n.val;

            if lower.is_some() && val <= lower.unwrap(){
                return false;
            }

            if upper.is_some() && val >= upper.unwrap() {
                return false;
            }

            // Validate left subtree
            if !Self::validate(&n.left, lower, Some(val)) {
                return false;
            }
            
            // Validate right subtree
            if !Self::validate(&n.right, Some(val), upper) {
                return false;
            }

        }

        
        // default, root is None, return true
        true

    }
}
```