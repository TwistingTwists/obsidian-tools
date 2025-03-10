

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
use std::collections::VecDeque;

impl Solution {
    pub fn zigzag_level_order(root: Option<Rc<RefCell<TreeNode>>>) -> Vec<Vec<i32>> {
        // decide recursive vs ierative approach
        let mut results = Vec::new();
        if root.is_none(){
            return results;
        }

        let mut queue = VecDeque::new();
        queue.push_back(root);

        while !queue.is_empty(){
            let mut level = Vec::new();
            let level_size = queue.len();
    
            for _ in 0..level_size{
    
                if let Some(Some(node)) = queue.pop_front() {
                    let n = node.borrow();
                    level.push(n.val);
    
                    if  n.left.clone().is_some() {
                        queue.push_back(n.left.clone())    ;
                    }
    
                    if  n.right.clone().is_some() {
                        queue.push_back(n.right.clone())  ;
                    }
                }
            }
            results.push(level);
        };
        results
    }
}


```




Binary ZigZag LOT

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
use std::collections::VecDeque;

impl Solution {
    pub fn zigzag_level_order(root: Option<Rc<RefCell<TreeNode>>>) -> Vec<Vec<i32>> {
        // decide recursive vs ierative approach
        let mut results = Vec::new();
        if root.is_none(){
            return results;
        }

        let mut queue = VecDeque::new();
        queue.push_back(root);
        
        let mut is_direction_l_to_r = true;
        
        while !queue.is_empty(){
            let mut level = VecDeque::new();
            let level_size = queue.len();
            
    
            for _ in 0..level_size{
    
                if let Some(Some(node)) = queue.pop_front() {
                    let n = node.borrow();
                    
                    if is_direction_l_to_r{ 
                        level.push_back(n.val);
                    } else {
                        level.push_front(n.val);
                    }
    
                    if  n.left.clone().is_some() {
                        queue.push_back(n.left.clone())    ;
                    }
    
                    if  n.right.clone().is_some() {
                        queue.push_back(n.right.clone())  ;
                    }
                }
            }
            results.push(level.into_iter().collect());
            is_direction_l_to_r = !is_direction_l_to_r
        };
        results
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


---
Binary ZigZag LOT 

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
        LTR = true # <<<<<<<<<<<<<<

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
            # >>>>>>
			if LTR:
	            results.append(level)
	            LTR = !LTR
	        else:
		        results.append(level.reverse())
		        LTR = !LTR
# >>>>>>
        return results

```