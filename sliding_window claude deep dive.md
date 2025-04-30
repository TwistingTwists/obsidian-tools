---
tags:
  - leetcode
---
Let me continue with the sliding window technique section:

#### Time and Space Complexity:

- Time Complexity: O(n) where n is the length of the array
- Space Complexity: O(1)

#### Step-by-Step Execution:

For `target = 7` and `nums = [2, 3, 1, 2, 4, 3]`:

1. Start with `left = 0`, `right = 0`, `current_sum = 0`, `min_length = inf`
2. Iteration 1: Add `nums[0] = 2` to `current_sum = 2`, not >= target, continue
3. Iteration 2: Add `nums[1] = 3` to `current_sum = 5`, not >= target, continue
4. Iteration 3: Add `nums[2] = 1` to `current_sum = 6`, not >= target, continue
5. Iteration 4: Add `nums[3] = 2` to `current_sum = 8`, >= target:
    - Update `min_length = min(inf, 4) = 4`
    - Remove `nums[0] = 2` from `current_sum = 6`, still >= target
    - Update `min_length = min(4, 3) = 3`
    - Remove `nums[1] = 3` from `current_sum = 3`, not >= target, break shrinking loop
6. Iteration 5: Add `nums[4] = 4` to `current_sum = 7`, >= target:
    - Update `min_length = min(3, 3) = 3`
    - Remove `nums[2] = 1` from `current_sum = 6`, not >= target, break shrinking loop
7. Iteration 6: Add `nums[5] = 3` to `current_sum = 9`, >= target:
    - Update `min_length = min(3, 4) = 3`
    - Remove `nums[3] = 2` from `current_sum = 7`, still >= target
    - Update `min_length = min(3, 3) = 3`
    - Remove `nums[4] = 4` from `current_sum = 3`, not >= target, break shrinking loop
8. Return `min_length = 2`

### Variation 2: Longest Substring Without Repeating Characters

**Problem Statement (LeetCode 3)**: Given a string `s`, find the length of the longest substring without repeating characters.

**Example**:

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

#### Approach:

We'll use a sliding window with a hash map to track character positions:

1. Maintain a window that contains only unique characters
2. When a duplicate is found, shrink the window from the left until the duplicate is removed
3. Keep track of the maximum window size seen so far

```python
def lengthOfLongestSubstring(s):
    n = len(s)
    char_map = {}  # Map to store the most recent position of each character
    max_length = 0
    left = 0
    
    for right in range(n):
        # If character is already in the window, move left pointer
        if s[right] in char_map and char_map[s[right]] >= left:
            left = char_map[s[right]] + 1
        else:
            # Update max length
            max_length = max(max_length, right - left + 1)
        
        # Update character position
        char_map[s[right]] = right
    
    return max_length
```

#### Time and Space Complexity:

- Time Complexity: O(n) where n is the length of the string
- Space Complexity: O(min(m, n)) where m is the size of the character set

#### Step-by-Step Execution:

For `s = "abcabcbb"`:

1. Initialize `left = 0`, `char_map = {}`, `max_length = 0`
2. Iteration 1: `s[0] = 'a'` not in map, `max_length = 1`, `char_map = {'a': 0}`
3. Iteration 2: `s[1] = 'b'` not in map, `max_length = 2`, `char_map = {'a': 0, 'b': 1}`
4. Iteration 3: `s[2] = 'c'` not in map, `max_length = 3`, `char_map = {'a': 0, 'b': 1, 'c': 2}`
5. Iteration 4: `s[3] = 'a'` in map at position 0, move `left` to `0 + 1 = 1`, `max_length = 3`, `char_map = {'a': 3, 'b': 1, 'c': 2}`
6. Iteration 5: `s[4] = 'b'` in map at position 1, move `left` to `1 + 1 = 2`, `max_length = 3`, `char_map = {'a': 3, 'b': 4, 'c': 2}`
7. Continue this process...
8. Final result: `max_length = 3`

### Variation 3: Longest Repeating Character Replacement

**Problem Statement (LeetCode 424)**: Given a string `s` and an integer `k`, you can replace any character in the string with another character at most `k` times. Return the length of the longest substring containing the same letter after the replacements.

**Example**:

```
Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA". The substring "BBBB" has the longest repeating letters, which is 4.
```

#### Approach:

This problem requires tracking the frequency of characters in our window:

1. Maintain a sliding window and a frequency map of characters
2. Track the character with the maximum frequency in the current window
3. If (window size - max frequency) > k, the window needs to be shrunk
4. Keep track of the maximum valid window size

```python
def characterReplacement(s, k):
    n = len(s)
    char_count = {}
    max_length = 0
    max_count = 0
    left = 0
    
    for right in range(n):
        # Update character frequency
        char_count[s[right]] = char_count.get(s[right], 0) + 1
        
        # Update max frequency character count
        max_count = max(max_count, char_count[s[right]])
        
        # If window has more than k replacements needed, shrink it
        if (right - left + 1) - max_count > k:
            char_count[s[left]] -= 1
            left += 1
        
        # Update max length
        max_length = max(max_length, right - left + 1)
    
    return max_length
```

#### Time and Space Complexity:

- Time Complexity: O(n) where n is the length of the string
- Space Complexity: O(1) since there are at most 26 English letters

#### Step-by-Step Execution:

For `s = "AABABBA"` and `k = 1`:

1. Initialize `left = 0`, `char_count = {}`, `max_length = 0`, `max_count = 0`
2. Iteration 1: `s[0] = 'A'`, `char_count = {'A': 1}`, `max_count = 1`, window valid, `max_length = 1`
3. Iteration 2: `s[1] = 'A'`, `char_count = {'A': 2}`, `max_count = 2`, window valid, `max_length = 2`
4. Iteration 3: `s[2] = 'B'`, `char_count = {'A': 2, 'B': 1}`, `max_count = 2`, window valid, `max_length = 3`
5. Iteration 4: `s[3] = 'A'`, `char_count = {'A': 3, 'B': 1}`, `max_count = 3`, window valid, `max_length = 4`
6. Iteration 5: `s[4] = 'B'`, `char_count = {'A': 3, 'B': 2}`, `max_count = 3`, (5-3) > 1, shrink window by incrementing `left` to 1, `char_count = {'A': 2, 'B': 2}`, `max_length = 4`
7. Continue this process...
8. Final result: `max_length = 4`

## Advanced Problems and Combined Techniques

Now let's tackle some advanced problems that may require combining multiple techniques.

### Problem 1: Minimum Window Substring

**Problem Statement (LeetCode 76)**: Given two strings `s` and `t`, return the minimum window in `s` which will contain all the characters in `t`. If there is no such window, return the empty string.

**Example**:

```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

#### Approach:

This is a challenging variable-size sliding window problem:

1. Create a frequency counter for characters in `t`
2. Use a sliding window to find substrings containing all characters in `t`
3. Keep track of how many characters from `t` are matched in the current window
4. When all characters are matched, try to shrink the window while maintaining the match

```python
def minWindow(s, t):
    if not s or not t:
        return ""
    
    # Character frequency in t
    target_counts = {}
    for char in t:
        target_counts[char] = target_counts.get(char, 0) + 1
    
    # Required matches
    required = len(target_counts)
    formed = 0
    
    # Window character counts
    window_counts = {}
    
    # Result variables
    min_length = float('inf')
    result_start = 0
    
    left = 0
    for right in range(len(s)):
        # Expand the window
        char = s[right]
        window_counts[char] = window_counts.get(char, 0) + 1
        
        # Check if this character helps form the required match
        if char in target_counts and window_counts[char] == target_counts[char]:
            formed += 1
        
        # Try to contract the window if all required characters are matched
        while left <= right and formed == required:
            char = s[left]
            
            # Update result if this window is smaller
            if right - left + 1 < min_length:
                min_length = right - left + 1
                result_start = left
            
            # Move left pointer
            window_counts[char] -= 1
            if char in target_counts and window_counts[char] < target_counts[char]:
                formed -= 1
                
            left += 1
    
    return "" if min_length == float('inf') else s[result_start:result_start + min_length]
```

#### Time and Space Complexity:

- Time Complexity: O(n + m) where n is the length of s and m is the length of t
- Space Complexity: O(n + m)

#### Step-by-Step Execution:

For `s = "ADOBECODEBANC"` and `t = "ABC"`:

1. Create `target_counts = {'A': 1, 'B': 1, 'C': 1}`, `required = 3`
2. Initialize window variables and result tracking
3. Expand window until we find all required characters ('A', 'B', 'C')
4. First valid window is "ADOBEC", then try to shrink
5. Continue expanding and shrinking until we find the minimum window "BANC"
6. Return "BANC"

### Problem 2: Subarrays with K Different Integers

**Problem Statement (LeetCode 992)**: Given an array `nums` of positive integers and an integer `k`, return the number of contiguous subarrays where the number of different integers in that subarray is exactly `k`.

**Example**:

```
Input: nums = [1,2,1,3,4], k = 3
Output: 3
Explanation: Subarrays with exactly 3 different integers: [1,2,1,3], [2,1,3], [1,3,4]
```

#### Approach:

This is a more complex sliding window problem. We'll solve it by using a clever trick: count of subarrays with exactly k different integers = count of subarrays with at most k different integers - count of subarrays with at most (k-1) different integers.

First, let's define a function to count subarrays with at most K different integers:

```python
def atMostK(nums, k):
    count = 0
    left = 0
    n = len(nums)
    distinct_count = {}
    
    for right in range(n):
        # Add the right element to the window
        distinct_count[nums[right]] = distinct_count.get(nums[right], 0) + 1
        
        # Shrink window while we have more than k distinct integers
        while len(distinct_count) > k:
            distinct_count[nums[left]] -= 1
            if distinct_count[nums[left]] == 0:
                del distinct_count[nums[left]]
            left += 1
        
        # For current right, all subarrays ending at right with at most k distinct
        # elements are counted
        count += right - left + 1
    
    return count

def subarraysWithKDistinct(nums, k):
    return atMostK(nums, k) - atMostK(nums, k-1)
```

#### Time and Space Complexity:

- Time Complexity: O(n) where n is the length of the array
- Space Complexity: O(k)

#### Step-by-Step Execution:

For `nums = [1,2,1,3,4]` and `k = 3`:

1. Calculate `atMostK(nums, 3)`:
    - Window expands to include all elements (since there are exactly 4 distinct elements)
    - Shrink as needed to maintain at most 3 distinct elements
    - Count valid subarrays: 9
2. Calculate `atMostK(nums, 2)`:
    - Count valid subarrays with at most 2 distinct elements: 6
3. Return `9 - 6 = 3` subarrays with exactly 3 distinct integers

The key insight is that by subtracting the count of subarrays with at most (k-1) distinct integers from the count of subarrays with at most k distinct integers, we get exactly the count of subarrays with exactly k distinct integers.

## Practice Problems

Here are some additional practice problems categorized by technique:

### Two Pointers Problems:

1. [Valid Palindrome (LeetCode 125)](https://leetcode.com/problems/valid-palindrome/)
2. [3Sum Closest (LeetCode 16)](https://leetcode.com/problems/3sum-closest/)
3. [Sort Colors (LeetCode 75)](https://leetcode.com/problems/sort-colors/)
4. [Move Zeroes (LeetCode 283)](https://leetcode.com/problems/move-zeroes/)
5. [Remove Duplicates from Sorted Array (LeetCode 26)](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

### Fast and Slow Pointers Problems:

1. [Palindrome Linked List (LeetCode 234)](https://leetcode.com/problems/palindrome-linked-list/)
2. [Reorder List (LeetCode 143)](https://leetcode.com/problems/reorder-list/)
3. [Intersection of Two Linked Lists (LeetCode 160)](https://leetcode.com/problems/intersection-of-two-linked-lists/)
4. [Happy Number (LeetCode 202)](https://leetcode.com/problems/happy-number/)
5. [Find the Duplicate Number (LeetCode 287)](https://leetcode.com/problems/find-the-duplicate-number/)

### Sliding Window Problems:

1. [Fruit Into Baskets (LeetCode 904)](https://leetcode.com/problems/fruit-into-baskets/)
2. [Permutation in String (LeetCode 567)](https://leetcode.com/problems/permutation-in-string/)
3. [Longest Substring with At Most K Distinct Characters (LeetCode 340)](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)
4. [Max Consecutive Ones III (LeetCode 1004)](https://leetcode.com/problems/max-consecutive-ones-iii/)
5. [Sliding Window Maximum (LeetCode 239)](https://leetcode.com/problems/sliding-window-maximum/)

### Combined Techniques:

1. [Longest Mountain in Array (LeetCode 845)](https://leetcode.com/problems/longest-mountain-in-array/)
2. [Longest Substring with At Least K Repeating Characters (LeetCode 395)](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/)
3. [Shortest Unsorted Continuous Subarray (LeetCode 581)](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/)
4. [Merge Sorted Array (LeetCode 88)](https://leetcode.com/problems/merge-sorted-array/)

## Conclusion

The two pointers and sliding window techniques are powerful approaches for solving a wide variety of array, string, and linked list problems efficiently. They help reduce the time complexity from O(n²) to O(n) in many cases.

Key takeaways:

1. **Two Pointers Technique**:
    
    - Useful when searching for pairs, triplets, or subarrays
    - Common patterns include opposite ends, same direction, and fast-slow pointers
    - Particularly effective for sorted arrays and linked lists
2. **Sliding Window Technique**:
    
    - Ideal for problems involving subarrays or substrings
    - Can be fixed-size or variable-size depending on the problem
    - Often combined with hash maps for frequency counting
3. **Problem-Solving Strategy**:
    
    - Identify if the problem can be solved by examining elements in relation to each other
    - Determine if you need to examine every consecutive subarray/substring
    - Choose the appropriate variant of the two pointers or sliding window technique
    - Consider if you need additional data structures to track information
4. **Implementation Tips**:
    
    - Be careful with boundary conditions (empty arrays, single elements)
    - Pay attention to pointer movements and when to stop
    - For variable-size windows, clearly define when to expand and when to shrink
    - Use descriptive variable names to keep track of the window state

By mastering these techniques, you'll be equipped to tackle a significant number of algorithmic problems efficiently. Remember to practice regularly with variations of these patterns to build your intuition and problem-solving skills.