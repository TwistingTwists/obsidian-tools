

### Knapsack variations

list - [Problem List - LeetCode](https://leetcode.com/problem-list/50vif4uc/)


```python

def knapsack( wt, val, capacity, n):
	if capacity == 0 or n == 0:
		return 0
	index = n - 1
	if wt[index] <= capacity:
		profit = max (val[index] + knapsack(wt,val, capacity - wt[index],index), knapsack(wt,val,capacity,index))
	else:
		profit = knapsack(wt, val, capacity, index)
	return profit 

```


Dynamic Programming Patterns  
[https://leetcode.com/discuss/study-guide/458695/Dynamic-Programming-Patterns](https://leetcode.com/discuss/study-guide/458695/Dynamic-Programming-Patterns)

  

Dynamic Programming Patterns 2  
[https://leetcode.com/discuss/study-guide/1437879/Dynamic-Programming-Patterns](https://leetcode.com/discuss/study-guide/1437879/Dynamic-Programming-Patterns)

## variations

**0/1 knapsack problems :**

- Subset sum
- Equal sum partition
- Count of subsets sum with a given sum
- Minimum subset sum difference
- Count the number of subset with a given difference
- Target sum

**Unbounded Knapsack**

- Integer Break
- Coin Change
- Coin Change 2
- Combination Sum IV
- Perfect Squares

**Longest Common Subsequence**

- Longest common substring
- Shortest common supersequence
- Minimum number of insertion and deletion to convert A to B
- Longest repeating subsequence
- Length of longest subsequence of A which is substring of B
- Subsequence pattern matching
- Count how many times A appears as subsequence in B
- Longest palindromic subsequence
- Count of palindromic substrings
- Minimum number of deletion in a string to make it palindrome
- Minimum number of insertion in a string to make it palindrome
- Uncrossed Lines

**Matrix Chain Multiplication**

- Burst Balloons
- Evaluate expression to true / boolean parenthesization
- Minimum or maximum value of a expression
- Palindrome partitioning
- Scramble string
- Super Egg Drop

**Fibonacci**

- Fibonacci number
- Climbing stairs
- Minimum jumps to reach the end
- Friends pairing problem
- Maximum subsequence sum such that no three are consecutive

**Longest Increasing Subsequence**

- Print longest increasing subsequence
- Number of longest increasing subsequences
- Longest non-decreasing subsequence
- Find the longest increasing subsequence in circular manner
- Longest bitonic subsequence
- Longest arithmetic subsequence
- Maximum sum increasing subsequence

**DP on Trees**

- Diameter of Binary Tree
- Binary Tree Maximum Path Sum
- Unique Binary Search Trees II
- House Robber III
- Sum of Distances in Tree

**DP on Grids**

- Unique Paths
- Unique Paths II
- Minimum Path Sum
- Dungeon Game
- Cherry Pickup


## Links only  with categories
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
  
9. [https://lnkd.in/dEiTg5yB](https://lnkd.in/dEiTg5yB)  
10. [https://lnkd.in/dk3zMy3s](https://lnkd.in/dk3zMy3s)  
11. [https://lnkd.in/dKhAzfUa](https://lnkd.in/dKhAzfUa)  
12. [https://lnkd.in/diWt_CpT](https://lnkd.in/diWt_CpT)  
13. [https://lnkd.in/dF4U5ZsV](https://lnkd.in/dF4U5ZsV)  
14. [https://lnkd.in/dMst59zc](https://lnkd.in/dMst59zc)  
  
[3] DP on Merging Intervals  
  
7. [https://lnkd.in/dHx3CTdg](https://lnkd.in/dHx3CTdg)  
8. [https://lnkd.in/de4ZDJVh](https://lnkd.in/de4ZDJVh)  
9. [https://lnkd.in/dA_Nh7VC](https://lnkd.in/dA_Nh7VC)  
10. [https://lnkd.in/dqiJp2Xh](https://lnkd.in/dqiJp2Xh)  
11. [https://lnkd.in/dp3eXBVq](https://lnkd.in/dp3eXBVq)  
12. [https://lnkd.in/dy3eKPbv](https://lnkd.in/dy3eKPbv)  
13. [https://lnkd.in/dEsEBt_Q](https://lnkd.in/dEsEBt_Q)



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
11. [https://leetcode.com/problems/dungeon-game/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/dungeon-game/?envType=list&envId=55ac4kuc)  
12. [https://leetcode.com/problems/maximal-square/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/maximal-square/?envType=list&envId=55ac4kuc)  
13. [https://leetcode.com/problems/perfect-squares/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/perfect-squares/?envType=list&envId=55ac4kuc)  
14. [https://leetcode.com/problems/coin-change/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/coin-change/?envType=list&envId=55ac4kuc)  
15. [https://leetcode.com/problems/ones-and-zeroes/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/ones-and-zeroes/?envType=list&envId=55ac4kuc)  
16. [https://leetcode.com/problems/2-keys-keyboard/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/2-keys-keyboard/?envType=list&envId=55ac4kuc)  
17. [https://leetcode.com/problems/min-cost-climbing-stairs/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/min-cost-climbing-stairs/?envType=list&envId=55ac4kuc)  
18. [https://leetcode.com/problems/minimum-number-of-refueling-stops/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/minimum-number-of-refueling-stops/?envType=list&envId=55ac4kuc)  
19. [https://leetcode.com/problems/minimum-falling-path-sum/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/minimum-falling-path-sum/?envType=list&envId=55ac4kuc)  
20. [https://leetcode.com/problems/minimum-cost-for-tickets/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/minimum-cost-for-tickets/?envType=list&envId=55ac4kuc)  
21. [https://leetcode.com/problems/last-stone-weight-ii/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/last-stone-weight-ii/?envType=list&envId=55ac4kuc)  
22. [https://leetcode.com/problems/tiling-a-rectangle-with-the-fewest-squares/?envType=list&envId=55ac4kuc](https://leetcode.com/problems/tiling-a-rectangle-with-the-fewest-squares/?envType=list&envId=55ac4kuc)

