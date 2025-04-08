---
tags:
  - interview
---

----
I love you ...❤️

---

# TWO POINTERS 

patterns: https://leetcode.com/discuss/study-guide/1688903/Solved-all-two-pointers-problems-in-100-days

sliding window pattern is a good candidate for any problem that involves searching for a continuous subsequence

| problem |     | sheet                            | concept                                                                |
| ------- | --- | -------------------------------- | ---------------------------------------------------------------------- |
| 611     |     | [[valid num triangles]]          | sort + two pointers help in skipping the entire i-1                    |
| 283     | e   | move zeroes                      | one pointer for zero, one for non-zero position                        |
| 75      | m   | sort colors                      | sorting with limited number of elements in array                       |
| 42      | h   | [[trapping rainwater]]           | two variables for 'previous' state of pointer +  two pointers for L, R |
| 238     | m   | [[product of array except self]] | optimal solution can skip creating two new arrays                      |
|         |     |                                  |                                                                        |
|         |     |                                  |                                                                        |


----
# TREES 

LIST : https://leetcode.com/problem-list/9ak7i9wv/
tree pattern : https://leetcode.com/discuss/study-guide/1337373/tree-question-pattern-2021-placement

graph templates : https://leetcode.com/discuss/study-guide/655708/Graph-For-Beginners-Problems-or-Pattern-or-Sample-Solutions 

**Longest Path:**

- **Longest Root-to-Leaf Path:** The path from the root node down to the farthest leaf node. Its length is often referred to as the **height** or **maximum depth** of the tree.
- **Diameter of Binary Tree:** The length of the _longest path between any two nodes_ in the tree. This path may or may not pass through the root.

| pattern                     | variations                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| LOT = level order traversal | - [[Level Order Traversal]]<br>- [[Binary Tree zigzag traversal]]<br>- N-ary Tree Zigzag LOT with constraints <br>- longest path in Binary (N-ary) Tree with LOT<br>- path sum      in Binary (N-ary) Tree with LOT<br>- convert tree to zigzag treej<br>- distance in zigzag binary tree<br>- zigzag with missing nodes (Handle zigzag traversal in trees that may not be complete, requiring dynamic adjustment of traversal paths )<br> |
| max depth of tree           | - [[max depth of tree]]<br>- [[Diameter of a Tree]]<br>- [[max path sum of a tree]]<br>                                                                                                                                                                                                                                                                                                                                                    |
| tree traversal              | - [[same tree or not]]<br>- [[validate BST]]<br>- [[recover BST]]<br><br>                                                                                                                                                                                                                                                                                                                                                                  |




- TRAVERSAL  - in-order, pre-order, post-order, level order 
- serialisation / deserialisation 
- **Tree Construction and Reconstruction**

|          | query                       | udpate                       |
| -------- | --------------------------- | ---------------------------- |
| sub-tree | Pre compute ( memoization ) | Euler Tour                   |
| path     | Binary Lifting              | HLD, Fenwick , Segment Trees |


| **Category** | **Query**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | **Update**                                                                                                                                                                                                                   |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Sub-tree** | - [All Possible Full Binary Trees](https://leetcode.com/problems/all-possible-full-binary-trees/)  <br>    _Enumerate all full binary trees that satisfy certain path conditions._<br><br>- [Sum of Nodes with Even Valued Grandparent](https://leetcode.com/problems/sum-of-nodes-with-even-valued-grandparent/)  <br>- [Number of Nodes in the Subtree With the Same Label](https://leetcode.com/problems/number-of-nodes-in-the-sub-tree-with-the-same-label/)  <br>- [Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)  <br>- [Number of Good Leaf Nodes Pairs](https://leetcode.com/problems/number-of-good-leaf-nodes-pairs/)  <br>- [Maximum Difference Between Node and Ancestor](https://leetcode.com/problems/maximum-difference-between-node-and-ancestor/)<br><br>- 337. House Robber III  <br>- 208. Implement Trie  <br>- 543. Diameter of Binary Tree<br><br>                                                                                                                                                               | - 814. Prune Binary Tree  <br>- [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)  <br>- [669. Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)           |
| **Path**     | <br>- [Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)  <br>- [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)  <br>- [Lowest Common Ancestor of Deepest Leaves](https://leetcode.com/problems/lowest-common-ancestor-of-deepest-leaves/)  <br>- [Kth Ancestor of a Tree Node](https://leetcode.com/problems/kth-ancestor-of-a-tree-node/)  <br><br><br>- [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)  <br>- 1654. Minimum Jumps to Reach Home  <br>- 124. Binary Tree Maximum Path Sum<br><br><br>- [Count Good Nodes in Binary Tree](https://leetcode.com/problems/count-good-nodes-in-binary-tree/)  <br>- [Number of Good Leaf Nodes Pairs](https://leetcode.com/problems/number-of-good-leaf-nodes-pairs/)  <br>- [Path Sum III](https://leetcode.com/problems/path-sum-iii/)  <br>- Binary Tree Maximum Path Sum<br>- Longest Univalue Path | - 124. Binary Tree Maximum Path Sum<br>- [Path Sum III](https://leetcode.com/problems/path-sum-iii/)  <br>- [Sum of Distances in Tree](https://leetcode.com/problems/sum-of-distances-in-tree/)<br><br>- 1245. Tree Diameter |
|              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                              |
|              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                              |


Tree Modifications:

1. Convert Binary Tree to Doubly Linked List (in-order traversal)
2. Flatten Binary Tree to Linked List (preorder)
3. Add One Row to Tree at Given Depth
4. Merge Two Binary Trees (node values add up)
5. Transform to Sum Tree (each node = sum of left and right subtree)

BST Problems:

1. Convert Sorted Array to Balanced BST
2. Trim BST (remove nodes outside range [low, high])
3. Recover BST (exactly two nodes swapped)
4. BST Iterator (next() and hasNext() with O(h) space)
5. Convert BST to Greater Tree (each node = sum of all greater nodes)

Advanced Data Structures:

1. Range Sum Query - Mutable (Segment Tree)
2. Count of Range Sum (Fenwick Tree)
3. Count of Smaller Numbers After Self (Segment/Fenwick)
4. Range XOR Queries (Segment Tree)
5. Maximum Sum Queries (2D Segment Tree)

Combined Algorithm:

1. Serialize and Deserialize N-ary Tree
2. Longest Path With Different Adjacent Characters (Tree + DP)
3. Sum of Distances in Tree (Tree + Graph)
4. Number of Ways to Reorder Array to Get Same BST
5. Longest ZigZag Path in Binary Tree (Tree + DP)




|          | query                                                        | udpate     |
| -------- | ------------------------------------------------------------ | ---------- |
| sub-tree | Pre compute                                                  | Euler Tour |
| path     | - LCA of BST = recursion<br>- Kth ancestory = Binary Lifting | HLD        |


-----

# DYNAMIC PROGRAMMING 

[https://leetcode.com/list/55ac4kuc](https://leetcode.com/list/55ac4kuc) - (Min - max path to target)  
[https://leetcode.com/list/55ajm50i](https://leetcode.com/list/55ajm50i) - (distinct ways)  
[https://leetcode.com/list/55aj8s16](https://leetcode.com/list/55aj8s16) - (merging intervals)  
[https://leetcode.com/list/55afh7m7](https://leetcode.com/list/55afh7m7) - (DP on strings)  
[https://leetcode.com/list/55af7bu7](https://leetcode.com/list/55af7bu7) - (decision making)


----


# Array problems 


|                                  |                |
| -------------------------------- | -------------- |
| [[Encode Decode Strings]]        | array / String |
| [[Product of array except self]] | prefix sum     |
|                                  |                |

------

# GRAPHS 

Graph Tushar Roy platylist  - https://www.youtube.com/playlist?list=PLrmLmBdmIlpu2f2g8ltqaaCZiq6GJvl1j

https://leetcode.com/problem-list/topological-sort/


-----


#  DP 
1. Two properties 
	1. Overlapping sub problem 
	2. optimal substructure 

Steps to master DSA: By [Harish](https://www.youtube.com/@Harisamsharma)?

- [ ] solve 20 easy - array + strings + language basics + binary search 
- [ ] 20-30 medium on leetcode - for DP practice - 2D and 3D 
- [ ] Binary search on answer

DP mental model : 
1. recurrence relation
2. DP solution to a problem exists =>  graph with DAG
https://www.youtube.com/watch?v=EgG3jsGoPvQ&list=PLgUwDviBIf0qUlt5H_kiKYaNSqJ81PMMY&index=4

| Type  | No. | Problem                               | Notes                                                                                                                                 |
| ----- | --- | ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| DP    | 70  | Climbing stairs                       | make the tree and see ; similar to fibonacci                                                                                          |
| DP    | 746 | Min cost climbing stairs              |                                                                                                                                       |
| Array | 15  | 3sum                                  | sort array -> loop over it and use two pointers (L, R) to see if the values sum upto 0 -> increment L until it doesn't repeat itself. |
| Array | 167 | Two Sum II                            | L, R pointers and keep checking the condition to adjust L and R                                                                       |
| Array | 11  | container with most water             |                                                                                                                                       |
| DP    | 198 | [[House Robber]]                      |                                                                                                                                       |
|       |     | [[121 - Best time to buy sell stock]] |                                                                                                                                       |
|       |     |                                       |                                                                                                                                       |


| algorithm | focus                                                          |
| --------- | -------------------------------------------------------------- |
| Kadane    | finding a maximum sum subarray (buy stocks + max sum subarray) |
|           |                                                                |



DP 

| Problem      | SubProblem                          | remark |
| ------------ | ----------------------------------- | ------ |
| 0-1 KnapSack | Subset Sum                          |        |
|              | Equal Sum Partition                 |        |
|              | Count of subset sum                 |        |
|              | min subset sum off                  |        |
|              | target sum                          |        |
|              | num of subset with given difference |        |
|              |                                     |        |



#### 0 -1 Knapsack 

Three types 
- fractional knapsack    :  greedy algo. No DP.
- 0/1 knapsack               : DP needed.
- unbounded knapsack :

Input : weight array, value array 
Optimise: Max value for the bag (knapsack) or min weight for the bag 
constraint: bag has fixed weight 


https://www.youtube.com/watch?v=w8vY2G5YFq4


# Binary Search 
- codeforces -> Binary search : https://codeforces.com/blog/entry/110077
- https://codeforces.com/edu/course/2/lesson/6/4



two pointers, 
sieve of erastotheneses : CP algorithms, prime numbers 



CP algorithms: 
monotonic stack, monotonic queues, (BLACK BOX - template - generic) segment trees 

DP problems: (few medium practices)
20-30 leetcode hard  + 20 codeforces  - (1500-1800 rating) 


graphs and trees
- dfs / bfs / - 15-20 leetcode medium / hard 
- graph algo - bellman ford, topological sort, indreicted graph , djikstra : 30 leetcode (20 hard, 10 medium)





---- 
[[Priority Queue]]