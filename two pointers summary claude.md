# Two Pointers and Sliding Window Techniques in LeetCode

## Introduction

Two pointers and sliding window are powerful algorithmic patterns that solve a wide range of problems efficiently. These patterns are especially common in array and string manipulation problems, and mastering them can significantly improve your problem-solving abilities on platforms like LeetCode.

This guide will take you through a progressive journey of understanding these techniques by:

1. Introducing the core concepts
2. Walking through base problems step-by-step
3. Exploring variations and extensions that build on the base problems
4. Providing implementation details with time and space complexity analysis

By the end of this guide, you'll have a deep understanding of when and how to apply these techniques to solve different types of problems.

## Table of Contents

1. [Two Pointers Technique](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#two-pointers-technique)
    
    1. [Base Problem: Two Sum II - Input Array Is Sorted](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#base-problem-two-sum-ii---input-array-is-sorted)
    2. [Variation 1: Three Sum](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#variation-1-three-sum)
    3. [Variation 2: Container With Most Water](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#variation-2-container-with-most-water)
    4. [Variation 3: Trapping Rain Water](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#variation-3-trapping-rain-water)
2. [Two Pointers with Fast and Slow Pointers](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#two-pointers-with-fast-and-slow-pointers)
    
    1. [Base Problem: Linked List Cycle](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#base-problem-linked-list-cycle)
    2. [Variation 1: Find the Start of Cycle](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#variation-1-find-the-start-of-cycle)
    3. [Variation 2: Middle of the Linked List](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#variation-2-middle-of-the-linked-list)
    4. [Variation 3: Remove Nth Node From End of List](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#variation-3-remove-nth-node-from-end-of-list)
3. [Sliding Window Technique](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#sliding-window-technique)
    
    1. [Base Problem: Maximum Sum Subarray of Size K](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#base-problem-maximum-sum-subarray-of-size-k)
    2. [Variation 1: Minimum Size Subarray Sum](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#variation-1-minimum-size-subarray-sum)
    3. [Variation 2: Longest Substring Without Repeating Characters](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#variation-2-longest-substring-without-repeating-characters)
    4. [Variation 3: Longest Repeating Character Replacement](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#variation-3-longest-repeating-character-replacement)
4. [Advanced Problems and Combined Techniques](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#advanced-problems-and-combined-techniques)
    
    1. [Problem 1: Minimum Window Substring](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#problem-1-minimum-window-substring)
    2. [Problem 2: Subarrays with K Different Integers](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#problem-2-subarrays-with-k-different-integers)
5. [Practice Problems](https://claude.ai/chat/eef1d399-ceb6-4208-9762-692cde5af958#practice-problems)
    

---

## Two Pointers Technique

The two pointers technique involves using two pointer variables to solve a problem in a single pass through the data structure, typically an array or linked list. This approach often helps reduce the time complexity from O(n²) to O(n).

### Common Patterns in Two Pointer Problems:

1. **Opposite Ends**: Pointers start at opposite ends of the array and move toward each other
2. **Same Direction**: Both pointers move in the same direction but at different speeds
3. **Fixed Distance**: Pointers maintain a fixed distance from each other

Let's dive into our first base problem.

### Base Problem: Two Sum II - Input Array Is Sorted

**Problem Statement (LeetCode 167)**: Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Return the indices of the two numbers, index1 and index2, added by one as an integer array [index1, index2] of length 2.

**Example**:

```
Input: numbers = [2, 7, 11, 15], target = 9
Output: [1, 2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2.
```

#### Two Pointers Approach:

Since the array is sorted, we can use two pointers starting from opposite ends:

1. Initialize left pointer at the beginning (index 0) and right pointer at the end (index n-1)
2. Calculate the sum of elements at left and right pointers
3. If sum equals target, return the indices
4. If sum is less than target, increment left pointer (to get a larger sum)
5. If sum is greater than target, decrement right pointer (to get a smaller sum)
6. Repeat until a solution is found or pointers meet

```python
def twoSum(numbers, target):
    left, right = 0, len(numbers) - 1
    
    while left < right:
        current_sum = numbers[left] + numbers[right]
        
        if current_sum == target:
            return [left + 1, right + 1]  # 1-indexed result
        elif current_sum < target:
            left += 1
        else:  # current_sum > target
            right -= 1
    
    return [-1, -1]  # No solution found
```

#### Time and Space Complexity:

- Time Complexity: O(n) where n is the length of the array
- Space Complexity: O(1) as we only use two pointers

#### Step-by-Step Execution:

For `numbers = [2, 7, 11, 15]` and `target = 9`:

1. Initialize `left = 0` and `right = 3`
2. Calculate `current_sum = numbers[0] + numbers[3] = 2 + 15 = 17`
3. Since `17 > 9`, we decrement `right` to `2`
4. Calculate `current_sum = numbers[0] + numbers[2] = 2 + 11 = 13`
5. Since `13 > 9`, we decrement `right` to `1`
6. Calculate `current_sum = numbers[0] + numbers[1] = 2 + 7 = 9`
7. Since `9 == 9`, we return `[1, 2]` (1-indexed)

#### Why This Works:

The sorted nature of the array allows us to systematically narrow down the solution space. When the sum is too large, we know we need to decrease it, so we move the right pointer left. When the sum is too small, we need to increase it, so we move the left pointer right.

### Variation 1: Three Sum

**Problem Statement (LeetCode 15)**: Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, `j != k`, and `nums[i] + nums[j] + nums[k] == 0`. The solution set must not contain duplicate triplets.

**Example**:

```
Input: nums = [-1, 0, 1, 2, -1, -4]
Output: [[-1, -1, 2], [-1, 0, 1]]
```

#### Approach:

We'll combine sorting with the two pointers technique:

1. Sort the array first
2. Iterate through the array, fixing the first element of the triplet
3. For each fixed element, use the two pointers technique to find pairs that sum to the target
4. Skip duplicate values to avoid duplicate triplets

```python
def threeSum(nums):
    result = []
    nums.sort()  # Sort the array
    n = len(nums)
    
    for i in range(n - 2):
        # Skip duplicates for the first element
        if i > 0 and nums[i] == nums[i - 1]:
            continue
        
        # Apply two pointers technique for the remaining array
        left, right = i + 1, n - 1
        target = -nums[i]  # We want three numbers to sum to 0
        
        while left < right:
            current_sum = nums[left] + nums[right]
            
            if current_sum == target:
                result.append([nums[i], nums[left], nums[right]])
                
                # Skip duplicates
                while left < right and nums[left] == nums[left + 1]:
                    left += 1
                while left < right and nums[right] == nums[right - 1]:
                    right -= 1
                
                left += 1
                right -= 1
            elif current_sum < target:
                left += 1
            else:  # current_sum > target
                right -= 1
    
    return result
```

#### Time and Space Complexity:

- Time Complexity: O(n²) because we have one loop O(n) and for each element we apply two pointers which is O(n)
- Space Complexity: O(1) excluding the output array

#### Step-by-Step Execution:

For `nums = [-1, 0, 1, 2, -1, -4]`:

1. After sorting: `nums = [-4, -1, -1, 0, 1, 2]`
2. Iteration 1: Fix `nums[0] = -4`
    - Target = 4, searching in [-1, -1, 0, 1, 2]
    - Find all pairs that sum to 4 using two pointers
    - No valid pairs found
3. Iteration 2: Fix `nums[1] = -1`
    - Target = 1, searching in [-1, 0, 1, 2]
    - Find pairs that sum to 1
    - Found: [-1, 0, 1] with sum 0
4. Iteration 3: Skip `nums[2] = -1` (duplicate)
5. Iteration 4: Fix `nums[3] = 0`
    - Target = 0, searching in [1, 2]
    - No valid pairs found
6. Output: [[-1, -1, 2], [-1, 0, 1]]

#### Key Insight:

Notice how we extended the two-sum problem to solve three-sum. This pattern can be generalized to solve k-sum problems. Each layer adds one more level of nested loops or recursion.

### Variation 2: Container With Most Water

**Problem Statement (LeetCode 11)**: Given an array `height` of length n, there are n vertical lines drawn such that the two endpoints of the ith line are `(i, 0)` and `(i, height[i])`. Find two lines that together with the x-axis form a container that contains the most water.

**Example**:

```
Input: height = [1, 8, 6, 2, 5, 4, 8, 3, 7]
Output: 49
Explanation: The max area is between heights 8 and 7 with width 7, area = min(8, 7) * 7 = 49
```

#### Approach:

This is a perfect example of the opposite-ends two-pointer technique:

1. Start with left pointer at the beginning and right pointer at the end
2. Calculate the area formed between these two lines
3. Move the pointer with the smaller height inward (as moving the taller one would only decrease area)
4. Continue this process until the pointers meet

```python
def maxArea(height):
    max_area = 0
    left, right = 0, len(height) - 1
    
    while left < right:
        # Calculate width and height
        width = right - left
        h = min(height[left], height[right])
        
        # Update max area
        max_area = max(max_area, width * h)
        
        # Move pointers
        if height[left] < height[right]:
            left += 1
        else:
            right -= 1
            
    return max_area
```

#### Time and Space Complexity:

- Time Complexity: O(n) where n is the length of the array
- Space Complexity: O(1)

#### Step-by-Step Execution:

For `height = [1, 8, 6, 2, 5, 4, 8, 3, 7]`:

1. Initialize `left = 0` and `right = 8`
2. Calculate area: `min(1, 7) * (8 - 0) = 1 * 8 = 8`
3. Since `height[left] < height[right]`, increment `left` to `1`
4. Calculate area: `min(8, 7) * (8 - 1) = 7 * 7 = 49`
5. Since `height[left] > height[right]`, decrement `right` to `7`
6. Calculate area: `min(8, 3) * (7 - 1) = 3 * 6 = 18`
7. Continue this process...
8. The final result is `49`

#### Key Insight:

The greedy approach of always moving the pointer with the smaller height works because:

- The width is decreasing with each step
- To maximize area, we need to find the best balance between width and height
- Moving the shorter line inward gives us a chance to find a taller line that might increase our area despite the reduced width

### Variation 3: Trapping Rain Water

**Problem Statement (LeetCode 42)**: Given an array `height` representing the elevation map where each element is the height of a bar, compute how much water it can trap after raining.

**Example**:

```
Input: height = [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
Output: 6
Explanation: The elevation map is shown above. The blue section represents the trapped water.
```

#### Approach:

This problem extends the two-pointer concept but requires tracking the maximum heights seen so far from both directions:

1. Use two pointers, left and right, starting from the edges
2. Keep track of the maximum height seen from both left and right sides
3. For each position, the water trapped is determined by the smaller of the two maximums minus the height at that position
4. Move the pointer from the side with the smaller maximum height

```python
def trap(height):
    if not height:
        return 0
    
    left, right = 0, len(height) - 1
    left_max = height[left]
    right_max = height[right]
    total_water = 0
    
    while left < right:
        if left_max <= right_max:
            left += 1
            left_max = max(left_max, height[left])
            total_water += left_max - height[left]
        else:
            right -= 1
            right_max = max(right_max, height[right])
            total_water += right_max - height[right]
    
    return total_water
```

#### Time and Space Complexity:

- Time Complexity: O(n) where n is the length of the array
- Space Complexity: O(1)

#### Step-by-Step Execution:

For `height = [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]`:

1. Initialize `left = 0`, `right = 11`, `left_max = 0`, `right_max = 1`, `total_water = 0`
2. `left_max (0) <= right_max (1)`, so move left pointer and update `left_max`
3. `left = 1`, `left_max = max(0, 1) = 1`, water at this position = `1 - 1 = 0`
4. Continue this process...
5. The final result is `6`

#### Alternative Approach:

We can also solve this using dynamic programming by precomputing the maximum heights from both directions:

```python
def trap(height):
    if not height:
        return 0
    
    n = len(height)
    left_max = [0] * n
    right_max = [0] * n
    
    # Calculate maximum heights from left to right
    left_max[0] = height[0]
    for i in range(1, n):
        left_max[i] = max(left_max[i-1], height[i])
    
    # Calculate maximum heights from right to left
    right_max[n-1] = height[n-1]
    for i in range(n-2, -1, -1):
        right_max[i] = max(right_max[i+1], height[i])
    
    # Calculate water trapped at each position
    total_water = 0
    for i in range(n):
        total_water += min(left_max[i], right_max[i]) - height[i]
    
    return total_water
```

#### Key Insight:

This problem shows how two pointers can be used in more complex ways. The key insight is that for any position, the water it can trap is determined by the minimum of the maximum heights on both sides minus its own height.

## Two Pointers with Fast and Slow Pointers

Another variant of the two pointers technique involves using fast and slow pointers that move at different speeds through a data structure. This pattern is particularly useful for linked list problems and cycle detection.

### Base Problem: Linked List Cycle

**Problem Statement (LeetCode 141)**: Given `head`, the head of a linked list, determine if the linked list has a cycle in it. There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer.

**Example**:

```
Input: head = [3,2,0,-4], where -4 points back to 2
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

#### Approach:

We'll use the Floyd's Tortoise and Hare algorithm (also known as the cycle-finding algorithm):

1. Use two pointers, slow and fast, both starting at the head
2. Move slow one step at a time and fast two steps at a time
3. If there's a cycle, the fast pointer will eventually meet the slow pointer
4. If fast reaches the end (null), there's no cycle

```python
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

def hasCycle(head):
    if not head or not head.next:
        return False
    
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next        # Move slow pointer one step
        fast = fast.next.next   # Move fast pointer two steps
        
        if slow == fast:        # If they meet, there's a cycle
            return True
    
    return False  # Fast reached end, no cycle
```

#### Time and Space Complexity:

- Time Complexity: O(n) where n is the number of nodes
- Space Complexity: O(1)

#### Step-by-Step Execution:

For a linked list `3 -> 2 -> 0 -> -4 -> 2` (cycle back to node with value 2):

1. Initialize `slow = head` and `fast = head`
2. Iteration 1:
    - Move `slow` to node `2`
    - Move `fast` to node `0`
3. Iteration 2:
    - Move `slow` to node `0`
    - Move `fast` to node `2` (after the cycle)
4. Iteration 3:
    - Move `slow` to node `-4`
    - Move `fast` to node `-4`
5. `slow == fast`, so we return `True`

#### Mathematical Proof:

If there's a cycle, why must the two pointers meet? Assume the distance from the head to the start of the cycle is `a`, and the cycle has length `b`. When the slow pointer enters the cycle, the fast pointer is already somewhere inside it. Let's say the fast pointer is `k` steps ahead of the slow pointer within the cycle.

The slow pointer moves 1 step per iteration, so after `b-k` more steps, it will reach the position where the fast pointer was when the slow pointer entered the cycle. However, during these `b-k` steps, the fast pointer moves `2(b-k)` steps, which is equivalent to going around the cycle `2(b-k)/b` times plus an additional `2(b-k) % b` steps.

This means that after the slow pointer moves `b-k` steps into the cycle, the fast pointer will be exactly `2(b-k) % b - (b-k) = (b-k) % b` steps ahead of the slow pointer. As both pointers continue to move, this gap will shrink by 1 each iteration (since fast moves 2 steps while slow moves 1). Eventually, the gap closes and they meet.

### Variation 1: Find the Start of Cycle

**Problem Statement (LeetCode 142)**: Given the head of a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

**Example**:

```
Input: head = [3,2,0,-4], where -4 points back to 2
Output: Node with value 2
Explanation: The cycle begins at the node with value 2.
```

#### Approach:

We'll extend the Floyd's algorithm to find the start of the cycle:

1. Use the same approach as before to detect if there's a cycle
2. Once the pointers meet, reset the fast pointer to the head
3. Move both pointers one step at a time
4. The point where they meet again is the start of the cycle

```python
def detectCycle(head):
    if not head or not head.next:
        return None
    
    slow = head
    fast = head
    
    # First phase: detect cycle
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:  # Cycle detected
            # Second phase: find the start
            fast = head
            while slow != fast:
                slow = slow.next
                fast = fast.next
            return slow
    
    return None  # No cycle
```

#### Time and Space Complexity:

- Time Complexity: O(n)
- Space Complexity: O(1)

#### Mathematical Explanation:

Let's denote:

- `a` as the distance from head to the start of the cycle
- `b` as the distance from the start of the cycle to the meeting point
- `c` as the remaining length of the cycle

When the slow and fast pointers meet, slow has traveled `a + b` distance, while fast has traveled `a + b + k(b + c)` where `k` is some integer representing full cycles.

Since fast moves twice as fast as slow: `a + b + k(b + c) = 2(a + b)` Simplifying: `k(b + c) = a + b` Rearranging: `a = k(b + c) - b = (k-1)(b + c) + c`

This means that the distance from the head to the cycle start (`a`) is equal to the distance from the meeting point to the cycle start traveling `c` steps plus some full cycles. This is why moving one pointer back to the head and advancing both at the same speed will make them meet at the cycle start.

### Variation 2: Middle of the Linked List

**Problem Statement (LeetCode 876)**: Given the head of a singly linked list, return the middle node of the linked list. If there are two middle nodes, return the second middle node.

**Example**:

```
Input: head = [1,2,3,4,5]
Output: Node with value 3
Explanation: The middle node of the list is Node 3.
```

#### Approach:

We'll use the fast and slow pointers technique:

1. Initialize slow and fast pointers at the head
2. Move slow one step at a time and fast two steps at a time
3. When fast reaches the end, slow will be at the middle

```python
def middleNode(head):
    slow = head
    fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    return slow
```

#### Time and Space Complexity:

- Time Complexity: O(n)
- Space Complexity: O(1)

#### Step-by-Step Execution:

For a linked list `1 -> 2 -> 3 -> 4 -> 5`:

1. Initialize `slow = head` and `fast = head`
2. Iteration 1:
    - Move `slow` to node `2`
    - Move `fast` to node `3`
3. Iteration 2:
    - Move `slow` to node `3`
    - Move `fast` to node `5`
4. Iteration 3:
    - `fast.next` is null, so we exit the loop
5. Return `slow`, which is at node `3`

This works because by the time the fast pointer reaches the end, the slow pointer has covered exactly half the distance.

### Variation 3: Remove Nth Node From End of List

**Problem Statement (LeetCode 19)**: Given the head of a linked list, remove the nth node from the end of the list and return its head.

**Example**:

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
Explanation: Removing the second node from the end, which is 4.
```

#### Approach:

We'll use fast and slow pointers with a gap of n between them:

1. Move fast pointer n steps ahead
2. If fast becomes null, it means we need to remove the head
3. Otherwise, move both pointers until fast reaches the last node
4. At this point, slow.next is the node to be removed

```python
def removeNthFromEnd(head, n):
    # Create a dummy node to handle edge cases
    dummy = ListNode(0)
    dummy.next = head
    
    fast = dummy
    slow = dummy
    
    # Advance fast by n+1 steps
    for _ in range(n + 1):
        if not fast:
            break
        fast = fast.next
    
    # Move both pointers until fast reaches the end
    while fast:
        slow = slow.next
        fast = fast.next
    
    # Remove the nth node from the end
    slow.next = slow.next.next
    
    return dummy.next
```

#### Time and Space Complexity:

- Time Complexity: O(n)
- Space Complexity: O(1)

#### Step-by-Step Execution:

For a linked list `1 -> 2 -> 3 -> 4 -> 5` and `n = 2`:

1. Create a dummy node: `dummy -> 1 -> 2 -> 3 -> 4 -> 5`
2. Initialize `slow = dummy` and `fast = dummy`
3. Move `fast` 3 steps ahead (n+1): `fast` is at node `3`
4. Move both pointers until `fast` reaches the end:
    - After 2 more iterations, `fast` is at node `5` and `slow` is at node `3`
5. Remove the node: `slow.next = slow.next.next`
    - This changes the list to `dummy -> 1 -> 2 -> 3 -> 5`
6. Return `dummy.next`, which is `1 -> 2 -> 3 -> 5`

The key insight is maintaining a gap of n nodes between the two pointers, so when the fast pointer reaches the end, the slow pointer is exactly at the position needed to remove the nth node from the end.

## Sliding Window Technique

The sliding window technique is a variation of the two pointers approach that maintains a "window" between two pointers and slides this window through the data structure. This technique is particularly useful for problems involving subarrays or substrings.

### Types of Sliding Windows:

1. **Fixed Size Window**: The window size remains constant throughout
2. **Variable Size Window**: The window size can grow or shrink based on certain conditions

### Base Problem: Maximum Sum Subarray of Size K

**Problem Statement**: Given an array of integers and a positive integer k, find the maximum sum of any contiguous subarray of size k.

**Example**:

```
Input: arr = [2, 1, 5, 1, 3, 2], k = 3
Output: 9
Explanation: Subarray with maximum sum is [5, 1, 3].
```

#### Naive Approach:

We could compute the sum of each k-sized subarray:

```python
def max_subarray_sum_naive(arr, k):
    max_sum = float('-inf')
    n = len(arr)
    
    for i in range(n - k + 1):
        current_sum = sum(arr[i:i+k])
        max_sum = max(max_sum, current_sum)
    
    return max_sum
```

This approach has O(n*k) time complexity.

#### Sliding Window Approach:

Instead of recomputing the sum for each window, we can slide the window by one element at a time:

1. Calculate the sum of the first k elements
2. For each subsequent position, subtract the element going out of the window and add the new element coming into the window
3. Keep track of the maximum sum seen so far

```python
def max_subarray_sum(arr, k):
    n = len(arr)
    if n < k:
        return None
    
    # Calculate sum of first window
    window_sum = sum(arr[:k])
    max_sum = window_sum
    
    # Slide the window
    for i in range(k, n):
        # Add new element and remove the first element of previous window
        window_sum = window_sum + arr[i] - arr[i - k]
        max_sum = max(max_sum, window_sum)
    
    return max_sum
```

#### Time and Space Complexity:

- Time Complexity: O(n)
- Space Complexity: O(1)

#### Step-by-Step Execution:

For `arr = [2, 1, 5, 1, 3, 2]` and `k = 3`:

1. Initial window sum = `2 + 1 + 5 = 8`, `max_sum = 8`
2. Slide window: add `1` and remove `2` -> `window_sum = 8 + 1 - 2 = 7`, `max_sum = 8`
3. Slide window: add `3` and remove `1` -> `window_sum = 7 + 3 - 1 = 9`, `max_sum = 9`
4. Slide window: add `2` and remove `5` -> `window_sum = 9 + 2 - 5 = 6`, `max_sum = 9`
5. Return `max_sum = 9`

### Variation 1: Minimum Size Subarray Sum

**Problem Statement (LeetCode 209)**: Given an array of positive integers `nums` and a positive integer `target`, find the minimal length of a contiguous subarray whose sum is greater than or equal to the `target`. If there is no such subarray, return 0.

**Example**:

```
Input: target = 7, nums = [2, 3, 1, 2, 4, 3]
Output: 2
Explanation: The subarray [4, 3] has the minimal length with a sum of 7.
```

#### Approach:

This problem requires a variable-size sliding window:

1. Start with an empty window (left and right both at 0)
2. Expand the window by moving the right pointer until the sum is >= target
3. Once we have a valid window, try to shrink it from the left
4. Keep track of the minimum length of valid windows

```python
def minSubArrayLen(target, nums):
    n = len(nums)
    left = 0
    current_sum = 0
    min_length = float('inf')
    
    for right in range(n):
        # Expand the window
        current_sum += nums[right]
        
        # Try to shrink the window
        while current_sum >= target:
            min_length = min(min_length, right - left + 1)
            current_sum -= nums[left]
            left += 1
    
    return min_length if min_length != float('inf') else 0
```

#### Time and Space Complexity:

- Time Complexity: O(n) where n is the length of the array
- Space Complexity: O(