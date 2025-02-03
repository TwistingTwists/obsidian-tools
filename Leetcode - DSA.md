---
tags:
  - interview
---
## to work on 

Internal Implementation of HashMaps
Auth - Jwt + SAML + SSO + OIDC?


----


pattern master list --- https://algo.monster/problems/stats

binary patterns - https://leetcode.com/discuss/study-guide/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems

tree patterns - https://leetcode.com/discuss/study-guide/1337373/tree-question-pattern-2021-placement


------

HEAP 

patterns: https://leetcode.com/discuss/general-discussion/1127238/master-heap-by-solving-23-questions-in-4-patterns-category


----

BINARY search

patterns: https://leetcode.com/discuss/study-guide/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems

---

TWO POINTERS 

patterns: https://leetcode.com/discuss/study-guide/1688903/Solved-all-two-pointers-problems-in-100-days


----
TREES 

LIST : https://leetcode.com/problem-list/9ak7i9wv/
tree pattern : https://leetcode.com/discuss/study-guide/1337373/tree-question-pattern-2021-placement

graph templates : https://leetcode.com/discuss/study-guide/655708/Graph-For-Beginners-Problems-or-Pattern-or-Sample-Solutions 

**Longest Path:**

- **Longest Root-to-Leaf Path:** The path from the root node down to the farthest leaf node. Its length is often referred to as the **height** or **maximum depth** of the tree.
- **Diameter of Binary Tree:** The length of the _longest path between any two nodes_ in the tree. This path may or may not pass through the root.

| pattern                     | variations                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| LOT = level order traversal | - [[Level Order Traversal]]<br>- [[Binary Tree zigzag traversal]]<br>- N-ary Tree Zigzag LOT with constraints <br>- longest path in Binary (N-ary) Tree with LOT<br>- path sum      in Binary (N-ary) Tree with LOT<br>- convert tree to zigzag treej<br>- distance in zigzag binary tree<br>- zigzag with missing nodes (Handle zigzag traversal in trees that may not be complete, requiring dynamic adjustment of traversal paths )<br> |
| max depth of tree           | - [[max depth of tree]]<br>- [[Diameter of a Tree]]<br>- [[max path sum of a tree]]<br><br>                                                                                                                                                                                                                                                                                                                                                |
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

DYNAMIC PROGRAMMING 

[https://leetcode.com/list/55ac4kuc](https://leetcode.com/list/55ac4kuc) - (Min - max path to target)  
[https://leetcode.com/list/55ajm50i](https://leetcode.com/list/55ajm50i) - (distinct ways)  
[https://leetcode.com/list/55aj8s16](https://leetcode.com/list/55aj8s16) - (merging intervals)  
[https://leetcode.com/list/55afh7m7](https://leetcode.com/list/55afh7m7) - (DP on strings)  
[https://leetcode.com/list/55af7bu7](https://leetcode.com/list/55af7bu7) - (decision making)


----


Array problems 


|                                  |                |
| -------------------------------- | -------------- |
| [[Encode Decode Strings]]        | array / String |
| [[Product of array except self]] | prefix sum     |
|                                  |                |

------

GRAPHS 

Graph Tushar Roy platylist  - https://www.youtube.com/playlist?list=PLrmLmBdmIlpu2f2g8ltqaaCZiq6GJvl1j

https://leetcode.com/problem-list/topological-sort/


-----


##### DP 
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


Binary Search 
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



**

DSA Patterns Sheet:

  

Here is a 10-line template that can solve most 'substring' problems  
[https://leetcode.com/problems/minimum-window-substring/solutions/26808/Here-is-a-10-line-template-that-can-solve-most-'substring'-problems/](https://leetcode.com/problems/minimum-window-substring/solutions/26808/Here-is-a-10-line-template-that-can-solve-most-'substring'-problems/)

  

C++ Maximum Sliding Window Cheatsheet Template

[https://leetcode.com/problems/frequency-of-the-most-frequent-element/solutions/1175088/C++-Maximum-Sliding-Window-Cheatsheet-Template/](https://leetcode.com/problems/frequency-of-the-most-frequent-element/solutions/1175088/C++-Maximum-Sliding-Window-Cheatsheet-Template/)

  

2 Pointer Problems

[https://leetcode.com/discuss/study-guide/1688903/Solved-all-two-pointers-problems-in-100-days](https://leetcode.com/discuss/study-guide/1688903/Solved-all-two-pointers-problems-in-100-days)

  

Backtracking Pattern  
[https://medium.com/leetcode-patterns/leetcode-pattern-3-backtracking-5d9e5a03dc26](https://medium.com/leetcode-patterns/leetcode-pattern-3-backtracking-5d9e5a03dc26)

  

Dynamic Programming Patterns  
[https://leetcode.com/discuss/study-guide/458695/Dynamic-Programming-Patterns](https://leetcode.com/discuss/study-guide/458695/Dynamic-Programming-Patterns)

  

Dynamic Programming Patterns 2  
[https://leetcode.com/discuss/study-guide/1437879/Dynamic-Programming-Patterns](https://leetcode.com/discuss/study-guide/1437879/Dynamic-Programming-Patterns)

  

Powerful Ultimate Binary Search Template

[https://leetcode.com/discuss/study-guide/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems](https://leetcode.com/discuss/study-guide/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems)

  

A general approach to backtracking questions

[https://leetcode.com/problems/permutations/solutions/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning)/](https://leetcode.com/problems/permutations/solutions/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning)/)

  

Binary Tree Traversal & Views

[https://leetcode.com/discuss/study-guide/937307/Iterative-or-Recursive-or-DFS-and-BFS-Tree-Traversal-or-In-Pre-Post-and-LevelOrder-or-Views](https://leetcode.com/discuss/study-guide/937307/Iterative-or-Recursive-or-DFS-and-BFS-Tree-Traversal-or-In-Pre-Post-and-LevelOrder-or-Views)

  

Graph For Beginners [Problems | Pattern | Sample Solutions]

[https://leetcode.com/discuss/study-guide/655708/Graph-For-Beginners-Problems-or-Pattern-or-Sample-Solutions](https://leetcode.com/discuss/study-guide/655708/Graph-For-Beginners-Problems-or-Pattern-or-Sample-Solutions)

  

A comprehensive guide and template for monotonic stack based problems

[https://leetcode.com/discuss/study-guide/2347639/A-comprehensive-guide-and-template-for-monotonic-stack-based-problems](https://leetcode.com/discuss/study-guide/2347639/A-comprehensive-guide-and-template-for-monotonic-stack-based-problems)

  

All Types of Patterns for Bits Manipulations and How to use it

[https://leetcode.com/discuss/study-guide/4282051/all-types-of-patterns-for-bits-manipulations-and-how-to-use-it](https://leetcode.com/discuss/study-guide/4282051/all-types-of-patterns-for-bits-manipulations-and-how-to-use-it)

  

Collections of Important String questions Pattern  
[https://leetcode.com/discuss/study-guide/2001789/Collections-of-Important-String-questions-Pattern](https://leetcode.com/discuss/study-guide/2001789/Collections-of-Important-String-questions-Pattern)

  
Leetcode Pattern 1 | BFS + DFS == 25% of the problems

[https://medium.com/leetcode-patterns/leetcode-pattern-1-bfs-dfs-25-of-the-problems-part-1-519450a84353](https://medium.com/leetcode-patterns/leetcode-pattern-1-bfs-dfs-25-of-the-problems-part-1-519450a84353)

  

14 Patterns to Ace Any Coding Interview Question

[https://hackernoon.com/14-patterns-to-ace-any-coding-interview-question-c5bb3357f6ed](https://hackernoon.com/14-patterns-to-ace-any-coding-interview-question-c5bb3357f6ed)

  
  
  
  
  
  
  
  




Dynamic Programming 
  
[1] DP on Strings  
  
1. [https://lnkd.in/dpHdnbJg](https://lnkd.in/dpHdnbJg)  
2. [https://lnkd.in/dftf72nm](https://lnkd.in/dftf72nm)  
3. [https://lnkd.in/dHAn6fGW](https://lnkd.in/dHAn6fGW)  
4. [https://lnkd.in/dUnJw4bS](https://lnkd.in/dUnJw4bS)  
5. [https://lnkd.in/dM8aTrRv](https://lnkd.in/dM8aTrRv)  
6. [https://lnkd.in/dpSTcynK](https://lnkd.in/dpSTcynK)  
7. [https://lnkd.in/db9ZagnM](https://lnkd.in/db9ZagnM)  
8. [https://lnkd.in/dxUK2cCv](https://lnkd.in/dxUK2cCv)  
  
[2] DP on Decision Making  
  
1. [https://lnkd.in/dEiTg5yB](https://lnkd.in/dEiTg5yB)  
2. [https://lnkd.in/dk3zMy3s](https://lnkd.in/dk3zMy3s)  
3. [https://lnkd.in/dKhAzfUa](https://lnkd.in/dKhAzfUa)  
4. [https://lnkd.in/diWt_CpT](https://lnkd.in/diWt_CpT)  
5. [https://lnkd.in/dF4U5ZsV](https://lnkd.in/dF4U5ZsV)  
6. [https://lnkd.in/dMst59zc](https://lnkd.in/dMst59zc)  
  
[3] DP on Merging Intervals  
  
1. [https://lnkd.in/dHx3CTdg](https://lnkd.in/dHx3CTdg)  
2. [https://lnkd.in/de4ZDJVh](https://lnkd.in/de4ZDJVh)  
3. [https://lnkd.in/dA_Nh7VC](https://lnkd.in/dA_Nh7VC)  
4. [https://lnkd.in/dqiJp2Xh](https://lnkd.in/dqiJp2Xh)  
5. [https://lnkd.in/dp3eXBVq](https://lnkd.in/dp3eXBVq)  
6. [https://lnkd.in/dy3eKPbv](https://lnkd.in/dy3eKPbv)  
7. [https://lnkd.in/dEsEBt_Q](https://lnkd.in/dEsEBt_Q)



[4] DP on Distinct Ways  
  
1. [https://leetcode.com/problems/unique-paths/?envType=list&envId=55ajm50i](https://leetcode.com/problems/unique-paths/?envType=list&envId=55ajm50i)  
2. [https://leetcode.com/problems/unique-paths-ii/?envType=list&envId=55ajm50i](https://leetcode.com/problems/unique-paths-ii/?envType=list&envId=55ajm50i)  
3. [https://leetcode.com/problems/climbing-stairs/?envType=list&envId=55ajm50i](https://leetcode.com/problems/climbing-stairs/?envType=list&envId=55ajm50i)  
4. [https://leetcode.com/problems/combination-sum-iv/?envType=list&envId=55ajm50i](https://leetcode.com/problems/combination-sum-iv/?envType=list&envId=55ajm50i)  
5. [https://leetcode.com/problems/partition-equal-subset-sum/?envType=list&envId=55ajm50i](https://leetcode.com/problems/partition-equal-subset-sum/?envType=list&envId=55ajm50i)  
6. [https://leetcode.com/problems/target-sum/?envType=list&envId=55ajm50i](https://leetcode.com/problems/target-sum/?envType=list&envId=55ajm50i)  
7. [https://leetcode.com/problems/out-of-boundary-paths/?envType=list&envId=55ajm50i](https://leetcode.com/problems/out-of-boundary-paths/?envType=list&envId=55ajm50i)  
8. [https://leetcode.com/problems/number-of-longest-increasing-subsequence/?envType=list&envId=55ajm50i](https://leetcode.com/problems/number-of-longest-increasing-subsequence/?envType=list&envId=55ajm50i)  
9. [https://leetcode.com/problems/knight-probability-in-chessboard/?envType=list&envId=55ajm50i](https://leetcode.com/problems/knight-probability-in-chessboard/?envType=list&envId=55ajm50i)  
10. [https://leetcode.com/problems/domino-and-tromino-tiling/?envType=list&envId=55ajm50i](https://leetcode.com/problems/domino-and-tromino-tiling/?envType=list&envId=55ajm50i)
3. [https://leetcode.com/problems/dungeon-game/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/dungeon-game/?envType=list&envId=55ac4kuc)  
4. [https://leetcode.com/problems/maximal-square/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/maximal-square/?envType=list&envId=55ac4kuc)  
5. [https://leetcode.com/problems/perfect-squares/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/perfect-squares/?envType=list&envId=55ac4kuc)  
6. [https://leetcode.com/problems/coin-change/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/coin-change/?envType=list&envId=55ac4kuc)  
7. [https://leetcode.com/problems/ones-and-zeroes/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/ones-and-zeroes/?envType=list&envId=55ac4kuc)  
8. [https://leetcode.com/problems/2-keys-keyboard/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/2-keys-keyboard/?envType=list&envId=55ac4kuc)  
9. [https://leetcode.com/problems/min-cost-climbing-stairs/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/min-cost-climbing-stairs/?envType=list&envId=55ac4kuc)  
10. [https://leetcode.com/problems/minimum-number-of-refueling-stops/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/minimum-number-of-refueling-stops/?envType=list&envId=55ac4kuc)  
11. [https://leetcode.com/problems/minimum-falling-path-sum/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/minimum-falling-path-sum/?envType=list&envId=55ac4kuc)  
12. [https://leetcode.com/problems/minimum-cost-for-tickets/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/minimum-cost-for-tickets/?envType=list&envId=55ac4kuc)  
13. [https://leetcode.com/problems/last-stone-weight-ii/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/last-stone-weight-ii/?envType=list&envId=55ac4kuc)  
14. [https://leetcode.com/problems/tiling-a-rectangle-with-the-fewest-squares/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/tiling-a-rectangle-with-the-fewest-squares/?envType=list&envId=55ac4kuc)
