

## Problem Statement 
You have an array of integers, where each integer represents the amount of money in a house. You want to maximize the total amount of money you rob, but you cannot rob two adjacent houses.

## House Robber Problem Input Examples

Here are some input examples for the House Robber problem on LeetCode, formatted in a Markdown table:

| Input                  | Explanation                                                       |
| ---------------------- | ----------------------------------------------------------------- |
| [1, 2, 3, 1]           | Rob house 1 (1) and house 3 (3) for a total of 4.                 |
| [2, 7, 9, 3, 1]        | Rob house 1 (2), house 3 (9), and house 5 (1) for a total of 12.  |
| [5, 5, 10, 40, 50, 35] | 80                                                                |
| [10, 2, 7, 3, 1, 8, 5] | Rob house 1 (10), house 3 (7), and house 6 (8) for a total of 25. |
| [10, 10, 10, 10]       | Rob house 1 (10) and house 3 (10) for a total of 20.              |
| [5]                    | Rob house 1 for a total of 5.                                     |
| []                     | No money can be robbed, total is 0.                               |
|                        |                                                                   |

```python
def rob(nums):
    if not nums:
        return 0
    if len(nums) == 1:
        return nums[0]
    
    prev2 = 0   # Represents dp[i-2]
    prev1 = 0   # Represents dp[i-1]
    
    for num in nums:
        current = max(prev1, prev2 + num)
        prev2 = prev1
        prev1 = current
    
    return prev1

```


## Dynamic Programming Problems


| Problem Name                                 | Description                                                                                                                                                                                         | Approach                                                                                                                                                                                                                                                      |
| -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **House Robber II**                          | Similar to the [[House Robber]] problem, but now the houses are arranged in a circle (i.e., the first and last houses are adjacent).                                                                | Solve the problem twice: <br/>1. Ignore the first house and consider only houses 2 to n. <br/>2. Ignore the last house and consider only houses 1 to n-1. <br/>Take the maximum of the two results.                                                           |
| **Paint House**                              | There are n houses, and you need to paint each house in one of three colors. Adjacent houses cannot have the same color. Each color has a different cost, and you want to minimize the total cost.  | Use a dp array where `dp[i][j]` represents the minimum cost to paint house `i` with color `j`. <br/>Transition: `dp[i][j] = cost[i][j] + min(dp[i-1][other_colors])`                                                                                          |
| **Paint Fence**                              | There are n fences, and you need to paint them using k colors such that no more than two adjacent fences have the same color. Calculate the total number of ways to paint the fence.                | Maintain two states: <br/>- same: Number of ways to paint so the last two fences are the same. <br/>- diff: Number of ways to paint so the last two fences are different. <br/>Transition: same[i] = diff[i-1] <br/>diff[i] = (same[i-1] + diff[i-1]) * (k-1) |
| **Maximum Sum of Non-Adjacent Elements**     | Given an array of integers, find the maximum sum you can obtain by selecting non-adjacent elements.                                                                                                 | Identical to the House Robber problem. <br/>Solution: Use the same recurrence relation: dp[i] = max(dp[i-1], nums[i] + dp[i-2])                                                                                                                               |
| **Delete and Earn**                          | Given an array, you can "delete" an element nums[i] and earn nums[i] points, but this also removes all occurrences of nums[i]-1 and nums[i]+1 from the array. Find the maximum points you can earn. | Convert the problem into a House Robber problem by grouping all identical elements and treating their sum as a "house". <br/>Example: Convert nums = [2, 2, 3, 3, 3, 4] into new_nums = [0, 0, 4, 9, 4].                                                      |
| **Partition Problem (Subset Sum Variation)** | Can you divide an array into two subsets with equal sums?                                                                                                                                           | Use dynamic programming to check if a subset exists with a sum equal to total_sum / 2. <br/>Transition: dp[i][j] = dp[i-1][j] or dp[i-1][j-nums[i]]                                                                                                           |
| **Weighted Job Scheduling**                  | You are given n jobs with start and end times and a profit for each job. Find the maximum profit you can earn such that no two jobs overlap.                                                        | Sort jobs by end time. <br/>Use a combination of binary search and dynamic programming to find the maximum profit by including or excluding a job.                                                                                                            |
| **Knapsack Problem**                         | Given weights and values of n items, determine the maximum value you can obtain by picking items such that their total weight does not exceed a given capacity.                                     | Use dynamic programming where dp[i][w] represents the maximum value achievable using the first i items with weight limit w. <br/>Transition: dp[i][w] = max(dp[i-1][w], dp[i-1][w-weights[i]] + values[i])                                                    |
| **Climbing Stairs**                          | You are climbing a staircase with n steps, and you can either take 1 step or 2 steps. Find the total number of distinct ways to reach the top.                                                      | Identical to Fibonacci: dp[i] = dp[i-1] + dp[i-2]                                                                                                                                                                                                             |
| **Burst Balloons**                           | You are given n balloons, each with a value. Popping a balloon earns you nums[i-1]*nums[i]*nums[i+1] points, and you remove it from the array. Maximize the points.                                 | Use DP to calculate maximum points for subarrays of balloons.                                                                                                                                                                                                 |

Alternate 1D array solution to House Robber:

```python
def rob(nums):
	if len(nums) == 0:
		return 0
	if len(nums) == 1:
		return nums[0]
	n = len(nums)
	
	dp = [0 for _ in range(n)]
	
	dp[0] = nums[0]
	# comes from recurrence relation
	dp[1] = max(nums[0],nums[1])

	for i in range(2,n):
		dp[i] = max(dp[i-1], dp[i-2] + nums[i])
	# last dp has the answer 
	return dp[-1]
```

Recursive solution: 

```python
def rob_recursive(nums):
    def helper(i):
        # Base cases
        if i < 0:  # No houses left to rob
            return 0
        if i == 0:  # Only one house
            return nums[0]
        
        # Recursive relation:
        # Either skip the current house or rob it
        return max(helper(i - 1), nums[i] + helper(i - 2))
    
    # Start solving from the last house
    return helper(len(nums) - 1)


```