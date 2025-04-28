
# Mastering Two Pointers and Sliding Window Techniques for LeetCode Problems

In the world of algorithmic problem-solving, particularly in coding interviews, two techniques stand out for their versatility and efficiency: Two Pointers and Sliding Window. These patterns can transform solutions from brute force approaches with O(n²) or O(n³) time complexity to elegant O(n) implementations. This comprehensive guide will take you through both techniques using a progressive approach, starting with foundational problems and gradually introducing variations that test your understanding with new twists.

## Two Pointers Technique: Fundamentals and Applications

The Two Pointers technique involves using two reference points to traverse data structures, typically arrays or linked lists. This approach eliminates the need for nested loops, significantly improving time complexity.

### Understanding the Basic Two Pointers Pattern

At its core, the Two Pointers technique can be classified into three primary types:

1. **Meet in the Middle (Opposite Directional)**: Pointers start at opposite ends and move toward each other
2. **Fast and Slow (Hare and Tortoise)**: Both pointers start at the same position but move at different speeds
3. **Expand Around Center**: Pointers start at the middle and expand outward

Let's begin with the most common implementation: the "meet in the middle" approach.

### Core Problem: Two Sum (LeetCode 1)

**Problem Statement:** Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

*Input:* ` nums =[^2][^7][^11][^15], target = 9`
*Output:* `[^1]`
**Explanation:** Because nums + nums[^1] == 9, we return[^1]

#### Two Pointers Approach: O(n)

For the two pointers approach to work efficiently, the array must be sorted. However, since we need to return original indices, we'll need to keep track of them before sorting:

```python
def twoSum(nums, target):
    # Create a list of (value, index) pairs
    indexed_nums = [(nums[i], i) for i in range(len(nums))]
    
    # Sort by value
    indexed_nums.sort()
    
    left, right = 0, len(nums) - 1
    
    while left &lt; right:
        current_sum = indexed_nums[left][^0] + indexed_nums[right][^0]
        
        if current_sum == target:
            return [indexed_nums[left][^1], indexed_nums[right][^1]]
        elif current_sum &lt; target:
            left += 1
        else:  # current_sum &gt; target
            right -= 1
            
    return []
```

This solution demonstrates the basic two pointers technique where we start from both ends of the array and move pointers based on comparison with the target sum[^1].

### Variation 1: Three Sum (LeetCode 15)

**Problem Statement:** Given an array `nums` of n integers, find all unique triplets in the array which give the sum of zero.

*Input:* ` nums = `[-1,0,1,2,-1,-4]``
*Output:* `[[-1,-1,2],[-1,0,1]]`

This problem adds complexity by:

1. Requiring three numbers instead of two
2. Needing to find all unique triplets (not just one solution)

#### Solution: Two Pointers with an Extra Loop

```python
def threeSum(nums):
    nums.sort()  # Sort the array
    result = []
    
    for i in range(len(nums) - 2):
        # Skip duplicates for the first element
        if i &gt; 0 and nums[i] == nums[i-1]:
            continue
            
        left, right = i + 1, len(nums) - 1
        
        while left &lt; right:
            total = nums[i] + nums[left] + nums[right]
            
            if total &lt; 0:
                left += 1
            elif total &gt; 0:
                right -= 1
            else:  # total == 0
                result.append([nums[i], nums[left], nums[right]])
                
                # Skip duplicates for the second and third elements
                while left &lt; right and nums[left] == nums[left + 1]:
                    left += 1
                while left &lt; right and nums[right] == nums[right - 1]:
                    right -= 1
                    
                left += 1
                right -= 1
                
    return result
```

This variation shows how the two pointers technique can be extended to find triplets by adding an outer loop. The time complexity becomes O(n²) because we have one loop to iterate through each number and the two pointers approach to find the pair[^2].

### Variation 2: Container With Most Water (LeetCode 11)

**Problem Statement:** Given n non-negative integers representing the height of each vertical line, find two lines that together with the x-axis form a container that would hold the most water.

*Input:* ` height =[^1][^8][^6][^2][^5][^4][^8][^3][^7]`
*Output:* ` 49`

This problem tests a different application of two pointers where:

1. The goal isn't to find a specific sum
2. We need to maximize an area calculation

#### Solution: Two Pointers to Maximize Area

```python
def maxArea(height):
    left, right = 0, len(height) - 1
    max_area = 0
    
    while left &lt; right:
        # Calculate the area
        width = right - left
        current_area = width * min(height[left], height[right])
        max_area = max(max_area, current_area)
        
        # Move the pointer with the smaller height
        if height[left] &lt; height[right]:
            left += 1
        else:
            right -= 1
            
    return max_area
```

This solution demonstrates how the two pointers technique can be applied to optimization problems. The key insight is that the area is limited by the shorter line, so we always move the pointer that points to the shorter line[^1].

### Variation 3: Valid Palindrome (LeetCode 125)

**Problem Statement:** Given a string `s`, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

*Input:* ` s = "A man, a plan, a canal: Panama"`
*Output:* ` true`

This problem introduces string manipulation with two pointers and adds the complexity of:

1. Filtering out non-alphanumeric characters
2. Case-insensitive comparison

#### Solution: Two Pointers from Ends to Middle

```python
def isPalindrome(s):
    # Convert to lowercase and remove non-alphanumeric characters
    filtered_chars = [c.lower() for c in s if c.isalnum()]
    
    # Check if it's a palindrome using two pointers
    left, right = 0, len(filtered_chars) - 1
    
    while left &lt; right:
        if filtered_chars[left] != filtered_chars[right]:
            return False
        left += 1
        right -= 1
        
    return True
```

This solution demonstrates a direct application of two pointers to check if a string is a palindrome[^14].

### Variation 4: Valid Palindrome II (LeetCode 680)

**Problem Statement:** Given a string `s`, return true if the string can be a palindrome after deleting at most one character from it.

*Input:* ` s = "abca"`
*Output:* ` true`
**Explanation:** You could delete the character 'c'.

This variation adds a twist to the palindrome problem:

1. We now have the option to delete one character
2. Need to determine if the resulting string can be a palindrome

#### Solution: Two Pointers with Recursive Check

```python
def validPalindrome(s):
    def check_palindrome(left, right):
        while left &lt; right:
            if s[left] != s[right]:
                return False
            left += 1
            right -= 1
        return True
    
    left, right = 0, len(s) - 1
    
    while left &lt; right:
        if s[left] != s[right]:
            # Try removing character at left or right
            return check_palindrome(left + 1, right) or check_palindrome(left, right - 1)
        left += 1
        right -= 1
        
    return True
```

This solution shows how to handle the case where we're allowed to delete one character. When we find a mismatch, we check if removing either the left or right character results in a palindrome[^14].

## Sliding Window Technique: From Basic to Advanced Applications

The Sliding Window technique is a specialized form of the two pointers approach that focuses on a contiguous sequence of elements. It's particularly useful for problems involving subarrays or substrings.

### Understanding the Sliding Window Pattern

There are two main types of sliding windows:

1. **Fixed-Size Window**: The window size remains constant throughout the process
2. **Variable-Size Window**: The window size adjusts dynamically based on conditions

Let's explore both types through progressive problem-solving.

### Core Problem: Maximum Sum Subarray of Size K

**Problem Statement:** Given an array of integers and a number k, find the maximum sum of a subarray of size k.

*Input:* ` nums = [1, 3, 2, 6, -1, 4, 1, 8, 2], k = 3`
*Output:* ` 15`
**Explanation:** Subarray [6, -1, 4] yields the maximum sum (15)

#### Naive Approach: O(n*k)

```python
def maxSumSubarrayOfSizeK(nums, k):
    max_sum = float('-inf')
    
    for i in range(len(nums) - k + 1):
        current_sum = 0
        for j in range(i, i + k):
            current_sum += nums[j]
        max_sum = max(max_sum, current_sum)
    
    return max_sum
```


#### Sliding Window Approach: O(n)

```python
def maxSumSubarrayOfSizeK(nums, k):
    max_sum = 0
    window_sum = 0
    start = 0
    
    for end in range(len(nums)):
        window_sum += nums[end]  # Add the next element to the window
        
        # When we hit the window size k
        if end &gt;= k - 1:
            max_sum = max(max_sum, window_sum)  # Update max sum
            window_sum -= nums[start]  # Remove the starting element
            start += 1  # Slide the window ahead
    
    return max_sum
```

This implementation demonstrates the fixed-size sliding window technique. Instead of recalculating the sum for each window, we slide the window by adding the next element and removing the first element[^3][^8].

### Variation 1: Minimum Size Subarray Sum (LeetCode 209)

**Problem Statement:** Given an array of positive integers `nums` and a positive integer `target`, find the minimum length of a contiguous subarray whose sum is greater than or equal to the `target`.

*Input:* ` nums =[^2][^3][^1][^2][^4][^3], target = 7`
*Output:* ` 2`
**Explanation:** The subarray[^4][^3] has the minimal length under the problem constraint.

This problem introduces variable-size windows where:

1. We need to find the minimum length, not a fixed size
2. The window expands and contracts based on the sum condition

#### Solution: Variable-Size Sliding Window

```python
def minSubArrayLen(nums, target):
    min_length = float('inf')
    current_sum = 0
    start = 0
    
    for end in range(len(nums)):
        current_sum += nums[end]
        
        # Shrink the window as small as possible while maintaining the sum condition
        while current_sum &gt;= target:
            min_length = min(min_length, end - start + 1)
            current_sum -= nums[start]
            start += 1
    
    return 0 if min_length == float('inf') else min_length
```

This solution demonstrates how to implement a variable-size sliding window. We expand the window until the sum condition is met, then contract it as much as possible while still meeting the condition[^8].

### Variation 2: Longest Substring Without Repeating Characters (LeetCode 3)

**Problem Statement:** Given a string `s`, find the length of the longest substring without repeating characters.

*Input:* ` s = "abcabcbb"`
*Output:* ` 3`
**Explanation:** The answer is "abc", with the length of 3.

This problem adds the complexity of:

1. Working with characters instead of numbers
2. Tracking character uniqueness in the current window

#### Solution: Sliding Window with Character Tracking

```python
def lengthOfLongestSubstring(s):
    char_set = set()
    max_length = 0
    start = 0
    
    for end in range(len(s)):
        # If the character is already in our set, shrink the window
        while s[end] in char_set:
            char_set.remove(s[start])
            start += 1
        
        # Add the current character to our set
        char_set.add(s[end])
        
        # Update the maximum length
        max_length = max(max_length, end - start + 1)
    
    return max_length
```

This solution uses a set to track unique characters. When we encounter a repeating character, we contract the window from the left until the repeating character is removed, maintaining the uniqueness property[^9][^16].

### Variation 3: Minimum Window Substring (LeetCode 76)

**Problem Statement:** Given two strings `s` and `t`, return the minimum window in `s` which will contain all the characters in `t`.

*Input:* ` s = "ADOBECODEBANC", t = "ABC"`
*Output:* ` "BANC"`

This problem presents a complex application of sliding window with:

1. Character frequency counting for both strings
2. Multiple conditions for window validity
3. Finding the minimal window that satisfies all conditions

#### Solution: Sliding Window with Character Frequency Counting

```python
from collections import Counter

def minWindow(s, t):
    # Edge case: Empty target string
    if not t:
        return ""
    
    # Count characters in t
    t_count = Counter(t)
    required = len(t_count)
    
    # Sliding window pointers and counters
    start = 0
    formed = 0  # Counter for characters with matching frequency
    window_counts = Counter()
    
    # Answer variables
    min_len = float('inf')
    result_start = 0
    
    for end in range(len(s)):
        # Add current character to window count
        char = s[end]
        window_counts[char] += 1
        
        # Check if this character's frequency matches with t
        if char in t_count and window_counts[char] == t_count[char]:
            formed += 1
        
        # Try to minimize the window
        while formed == required:
            # Update result if current window is smaller
            if end - start + 1 &lt; min_len:
                min_len = end - start + 1
                result_start = start
            
            # Remove the leftmost character
            char = s[start]
            window_counts[char] -= 1
            
            # If this character is needed for t and removal makes it deficient
            if char in t_count and window_counts[char] &lt; t_count[char]:
                formed -= 1
            
            # Move left pointer
            start += 1
    
    return "" if min_len == float('inf') else s[result_start:result_start + min_len]
```

This solution demonstrates a sophisticated application of the sliding window technique with character frequency tracking. We use two counters: one for the target string and one for the current window[^7][^17].

### Variation 4: Longest Substring with At Most K Distinct Characters

**Problem Statement:** Given a string `s` and an integer `k`, return the length of the longest substring that contains at most `k` distinct characters.

*Input:* ` s = "eceba", k = 2`
*Output:* ` 3`
**Explanation:** The substring "ece" has length 3 and contains 2 distinct characters.

This problem adds the constraint of:

1. Limiting the number of distinct characters in the window
2. Finding the maximum length that satisfies this constraint

#### Solution: Sliding Window with Character Counting

```python
from collections import Counter

def lengthOfLongestSubstringKDistinct(s, k):
    char_count = Counter()
    max_length = 0
    start = 0
    
    for end in range(len(s)):
        char_count[s[end]] += 1
        
        # Shrink the window if we have more than k distinct characters
        while len(char_count) &gt; k:
            char_count[s[start]] -= 1
            if char_count[s[start]] == 0:
                del char_count[s[start]]
            start += 1
        
        # Update the maximum length
        max_length = max(max_length, end - start + 1)
    
    return max_length
```

This solution demonstrates using a sliding window with a Counter to track character frequencies. When we exceed k distinct characters, we shrink the window from the left until we're back to k distinct characters[^3][^10].

## Comparing Two Pointers and Sliding Window Techniques

While both techniques use two pointers, they have distinct applications:

1. **Two Pointers Technique:**
    - Often used on sorted arrays or when finding pairs/triplets that satisfy conditions
    - The pointers can move independently based on conditions
    - Can be applied in various patterns: meet in the middle, fast-slow, expand around center
    - Examples: Two Sum, Container With Most Water, Valid Palindrome
2. **Sliding Window Technique:**
    - Specializes in finding contiguous subarrays/substrings that satisfy conditions
    - Usually processes all elements between the pointers as a "window"
    - Can have fixed or variable-size windows
    - Examples: Maximum Sum Subarray, Longest Substring Without Repeating Characters, Minimum Window Substring

The key distinction: In the sliding window, we care about all elements within the window, whereas in the two pointers technique, we often only care about the elements at the pointer positions[^4].

## Advanced Problems Combining Both Techniques

### Problem: Trapping Rain Water (LeetCode 42)

**Problem Statement:** Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

*Input:* ` height =[^1][^2][^1][^1][^3][^2][^1][^2][^1]`
*Output:* ` 6`

This problem can be solved using the two pointers technique but requires a more intricate approach:

```python
def trap(height):
    if not height:
        return 0
    
    left, right = 0, len(height) - 1
    left_max = height[left]
    right_max = height[right]
    result = 0
    
    while left &lt; right:
        if left_max &lt; right_max:
            left += 1
            left_max = max(left_max, height[left])
            result += left_max - height[left]
        else:
            right -= 1
            right_max = max(right_max, height[right])
            result += right_max - height[right]
    
    return result
```

This solution demonstrates a sophisticated application of the two pointers technique. We track the maximum height from both sides and calculate the trapped water at each position[^5].

### Problem: Interval List Intersections (LeetCode 986)

**Problem Statement:** Given two lists of closed intervals, each list of intervals is pairwise disjoint and in sorted order. Return the intersection of these two interval lists.

This problem combines aspects of both techniques:

```python
def intervalIntersection(firstList, secondList):
    result = []
    i, j = 0, 0
    
    while i &lt; len(firstList) and j &lt; len(secondList):
        # Find the overlap
        low = max(firstList[i][^0], secondList[j][^0])
        high = min(firstList[i][^1], secondList[j][^1])
        
        # If there is an overlap, add it to result
        if low &lt;= high:
            result.append([low, high])
        
        # Move the pointer of the interval with the smaller end point
        if firstList[i][^1] &lt; secondList[j][^1]:
            i += 1
        else:
            j += 1
    
    return result
```

This solution uses two pointers to traverse two sorted lists simultaneously, combining aspects of both techniques to find interval intersections[^5].

## Progressive Learning Path: From Beginner to Advanced

Based on the problems covered, here's a suggested learning path to master these techniques:

### Beginner Level:

1. Two Sum (LeetCode 1) - Basic two pointers
2. Valid Palindrome (LeetCode 125) - Two pointers with strings
3. Maximum Sum Subarray of Size K - Fixed-size sliding window

### Intermediate Level:

1. Three Sum (LeetCode 15) - Two pointers with triplets
2. Valid Palindrome II (LeetCode 680) - Two pointers with deletion option
3. Minimum Size Subarray Sum (LeetCode 209) - Variable-size sliding window
4. Longest Substring Without Repeating Characters (LeetCode 3) - Sliding window with uniqueness

### Advanced Level:

1. Container With Most Water (LeetCode 11) - Two pointers with area calculation
2. Minimum Window Substring (LeetCode 76) - Complex sliding window
3. Trapping Rain Water (LeetCode 42) - Two pointers with cumulative calculations
4. Interval List Intersections (LeetCode 986) - Combined technique

## Conclusion: Mastering These Techniques

The Two Pointers and Sliding Window techniques are powerful tools in your algorithmic toolkit. By understanding these patterns and practicing their variations, you can significantly improve your problem-solving skills.

Key takeaways:

1. **Recognize the pattern:** Learn to identify when a problem is solvable using these techniques
2. **Start simple:** Begin with the basic patterns before tackling more complex variations
3. **Understand the variations:** Study how small changes in problem statements affect the solution approach
4. **Practice progressively:** Follow a structured learning path from simple to complex problems

With consistent practice and pattern recognition, you'll be able to apply these techniques to solve a wide range of problems efficiently.

## References[^1] "Two-Pointer Technique, an In-Depth Guide: Concepts Explained" - Reddit[^2] "sliding window is same as 2 pointers or is there difference btw 2 techniques?" - Reddit[^3] "Sliding Window Technique" - Reddit[^4] "Is two pointer problem same as sliding window" - Stack Overflow[^5] "Pattern: Two Pointers" - aman.ai[^6] "Several Coding Patterns for Solving Data Structures and Algorithms Problems" - GitHub[^7] "leetocode 76 minimum window substring - null pointer exception" - Reddit[^8] "Sliding Windows Explained" - Reddit[^9] "3. Longest Substring Without Repeating Characters" - GitHub[^10] "Sliding Window Algorithm - Working Explained With Code Examples"[^11] "Sliding Window and Two Pointer Technique" - LinkedIn[^12] "how to improve my answer for the two sum problem(leet code problem)" - Reddit[^13] "Two sum leetcode problem" - Reddit[^14] "Valid Palindrome - II" - Reddit[^15] "Sliding Window Algorithm" - Baeldung on Computer Science[^16] "3. Longest Substring Without Repeating Characters" - AlgoMonster[^17] "LeetCode 76: Minimum Window Substring - All Solutions Explained"

<div style="text-align: center">⁂</div>

[^1]: https://www.reddit.com/r/leetcode/comments/18g9383/twopointer_technique_an_indepth_guide_concepts/

[^2]: https://www.reddit.com/r/leetcode/comments/14tgv8h/sliding_window_is_same_as_2_pointers_or_is_there/

[^3]: https://www.reddit.com/r/leetcode/comments/15a7ezt/sliding_window_technique/

[^4]: https://stackoverflow.com/questions/64078162/is-two-pointer-problem-same-as-sliding-window

[^5]: https://aman.ai/code/two-pointers/

[^6]: https://github.com/Chanda-Abdul/Several-Coding-Patterns-for-Solving-Data-Structures-and-Algorithms-Problems-during-Interviews/blob/main/✅  Pattern 02: Two Pointers.md

[^7]: https://www.reddit.com/r/learnprogramming/comments/18xl2g6/leetocode_76_minimum_window_substring_null/

[^8]: https://www.reddit.com/r/leetcode/comments/1iic14f/sliding_windows_explained/

[^9]: https://github.com/doocs/leetcode/blob/main/solution/0000-0099/0003.Longest Substring Without Repeating Characters/README_EN.md

[^10]: https://unstop.com/blog/sliding-window-algorithm

[^11]: https://www.linkedin.com/pulse/sliding-window-two-pointer-technique-saiful-islam-rasel

[^12]: https://www.reddit.com/r/learnpython/comments/due1kg/how_to_improve_my_answer_for_the_two_sum/

[^13]: https://www.reddit.com/r/learnpython/comments/wt10hu/two_sum_leetcode_problem/

[^14]: https://www.reddit.com/r/leetcode/comments/19br7ir/valid_palindrome_ii/

[^15]: https://builtin.com/data-science/sliding-window-algorithm

[^16]: https://algo.monster/liteproblems/3

[^17]: https://www.sparkcodehub.com/leetcode-76-minimum-window-substring-explained

[^18]: https://www.reddit.com/r/leetcode/comments/1av0ey5/im_working_on_a_free_alternative_to_grokking_the/

[^19]: https://www.architectalgos.com/mastering-the-two-pointer-technique-a-guide-to-solving-array-problems-efficiently-71bd5a22bedc

[^20]: https://www.gaohongnan.com/dsa/two_pointers/two_pointers.html

[^21]: https://www.baeldung.com/cs/sliding-window-algorithm

[^22]: https://www.reddit.com/r/leetcode/comments/14sg9ux/im_not_capable_of_doing_leetcode_without_studying/

[^23]: https://www.reddit.com/r/leetcode/comments/16cwz67/why_are_leetcode_questions_so_hard_for_me/

[^24]: https://www.reddit.com/r/leetcode/comments/s1gxmc/help_with_two_pointer_techniques/

[^25]: https://www.reddit.com/r/leetcode/comments/1d0brjl/struggling_to_understand_two_pointer_solution_to/

[^26]: https://www.reddit.com/r/learnprogramming/comments/142mj6b/very_confused_about_two_pointer_technique/

[^27]: https://www.reddit.com/r/leetcode/comments/14pxl5i/sliding_window_is_kicking_my_ass_and_im_losing/

[^28]: https://www.reddit.com/r/leetcode/comments/1f3da2r/how_do_you_all_solve_two_pointer_problems/

[^29]: https://www.reddit.com/r/leetcode/comments/1k4n2tb/i_am_a_beginner_in_lc_i_wanted_to_ask_if_you_dont/

[^30]: https://www.reddit.com/r/algorithms/comments/oz9jk3/techniques_like_two_pointer_to_get_better_at/

[^31]: https://www.reddit.com/r/leetcode/comments/yo3646/little_bit_confused_in_sliding_window_and_prefix/

[^32]: https://www.reddit.com/r/leetcode/comments/16dp0dq/best_time_to_buy_and_sell_stock_why_is_it_sliding/

[^33]: https://www.reddit.com/r/leetcode/comments/l0s6qs/are_twopointer_and_sliding_window_problems_the/

[^34]: https://www.reddit.com/r/leetcode/comments/108qf58/question_about_sliding_window_questions/

[^35]: https://www.reddit.com/r/leetcode/comments/1i6mvu0/beyond_two_pointers_a_complete_guide/

[^36]: https://www.reddit.com/r/leetcode/comments/1d31ksp/leetcode_patternstechniques_cheat_sheet/

[^37]: https://www.reddit.com/r/csMajors/comments/v8sigf/two_pointers_pattern/

[^38]: https://www.reddit.com/r/leetcode/comments/1f84r6c/what_are_most_common_widely_asked_patterns/

[^39]: https://www.reddit.com/r/leetcode/comments/w5ui9x/for_sliding_window_problems_how_do_you_know_when/

[^40]: https://www.reddit.com/r/leetcode/comments/1br1ak8/how_did_you_guys_get_good_at_substring_and_2/

[^41]: https://leetcode.com/problem-list/two-pointers/

[^42]: https://leetcode.com/articles/two-pointer-technique/

[^43]: https://www.youtube.com/watch?v=QzZ7nmouLTI

[^44]: https://leetcode.com/discuss/study-guide/1688903/Solved-all-two-pointers-problems-in-100-days.

[^45]: https://www.youtube.com/watch?v=Mf9C6vDxhzk

[^46]: https://www.youtube.com/watch?v=6lX7x1RcLvg

[^47]: https://www.youtube.com/watch?v=NWPtdSiuAAs

[^48]: https://www.pluralsight.com/resources/blog/guides/algorithm-templates-two-pointers-part-3

[^49]: https://blog.algomaster.io/p/15-leetcode-patterns

[^50]: https://www.youtube.com/playlist?list=PLpIkg8OmuX-J2Ivo9YdY7bRDstPPTVGvN

[^51]: https://leetcode.com/discuss/study-guide/1905453/master-in-two-pointer

[^52]: https://www.youtube.com/watch?v=y2d0VHdvfdc

[^53]: https://leetcode.com/problems/minimum-window-substring/

[^54]: https://leetcode.com/problem-list/sliding-window/

[^55]: https://www.youtube.com/watch?v=9kdHxplyl5I

[^56]: https://www.reddit.com/r/leetcode/comments/1ftxpav/feeling_stuck_with_twopointer_problems_on_blind/

[^57]: https://www.reddit.com/r/leetcode/comments/1bov6fw/two_pointer_problems_how_do_we_know_whether_to/

[^58]: https://www.reddit.com/r/leetcode/comments/16f9w2x/beginner_to_150_leetcode_problems_in_2_weeks_best/

[^59]: https://www.reddit.com/r/leetcode/comments/xjbil5/twosum_like_question_with_2_arrays_need_to_beat/

[^60]: https://www.reddit.com/r/leetcode/comments/154d8jo/can_someone_explain_to_me_how_11_container_with/

[^61]: https://www.reddit.com/r/leetcode/comments/uj94f7/what_thinking_framework_do_you_use_to_identify_if/

[^62]: https://www.reddit.com/r/learnprogramming/comments/rxsclt/any_good_resources_for_learning_the_two_pointer/

[^63]: https://www.reddit.com/r/learnpython/comments/1cukn7w/leetcode_python_twosum_says_my_answer_is_invalid/

[^64]: https://www.reddit.com/r/learnprogramming/comments/krjmn0/could_someone_explain_the_algorithm_for_leetcode/

[^65]: https://www.reddit.com/r/leetcode/comments/1df0xe7/leetcode_says_2sum_was_asked_by_faang_alot_in/

[^66]: https://www.reddit.com/r/leetcode/comments/xaqzcx/did_you_study_data_structures_and_algorithms/

[^67]: https://www.reddit.com/r/leetcode/comments/tarelx/problem_with_twosum_leetcode_solution/

[^68]: https://www.reddit.com/r/leetcode/comments/1iyw6bl/container_with_most_water_edge_case/

[^69]: https://www.youtube.com/watch?v=r_mVgmc89_U

[^70]: https://www.youtube.com/watch?v=Fs2Ft2NSaT8

[^71]: https://www.stratascratch.com/blog/solving-leetcode-two-sum-problem-for-data-science-interviews/

[^72]: https://github.com/doocs/leetcode/blob/main/solution/0000-0099/0011.Container With Most Water/README_EN.md

[^73]: https://dorianhe.github.io/Two-pointer-in-Leetcode-Problems/

[^74]: https://stackoverflow.com/questions/72064986/mathematical-explanation-of-leetcode-question-container-with-most-water

[^75]: https://leetcode.com/problems/add-two-numbers/

[^76]: https://www.youtube.com/watch?v=UuiTKBwPgAo

[^77]: https://www.reddit.com/r/leetcode/comments/yuy5ap/sliding_window_guide/

[^78]: https://www.reddit.com/r/leetcode/comments/123f2ly/i_used_to_be_afraid_of_the_sliding_window/

[^79]: https://www.reddit.com/r/leetcode/comments/18jtovj/the_sliding_window_list_is_here/

[^80]: https://www.reddit.com/r/leetcode/comments/1ap5yk5/solution_for_return_longest_substring_without/

[^81]: https://www.reddit.com/r/leetcode/comments/17r82u7/genuinely_am_struggling_more_with_sliding_window/

[^82]: https://www.reddit.com/r/leetcode/comments/1fv93e1/has_anyone_solved_todays_leetcode_problem_i_was/

[^83]: https://www.reddit.com/r/leetcode/comments/1609c0c/testcase_wit_10_5_input_does_not_pass_minimum/

[^84]: https://www.reddit.com/r/cpp_questions/comments/9x9jlf/leetcode_longest_substring_without_repeating/

[^85]: https://www.reddit.com/r/leetcode/comments/1e6erkp/help_with_sliding_window_please/

[^86]: https://www.reddit.com/r/leetcode/comments/1ibb8o3/got_crushed_by_the_question_asked_in_recent/

[^87]: https://www.reddit.com/r/leetcode/comments/18xkxv3/leetcode76_minimum_window_substring_getting_a/

[^88]: https://www.reddit.com/r/leetcode/comments/1dgruyg/longest_substring_without_repeating_characters/

[^89]: https://www.reddit.com/r/learnprogramming/comments/13grncu/a_visual_introduction_to_the_sliding_window/

[^90]: https://www.reddit.com/r/leetcode/comments/1d15d1s/a_simpler_solution_to_an_essential_sliding_window/

[^91]: https://leetcode.com/problems/sliding-window-maximum/

[^92]: https://leetcode.com/discuss/study-guide/3722472/mastering-sliding-window-technique-a-comprehensive-guide

[^93]: https://algo.monster/liteproblems/76

[^94]: https://github.com/doocs/leetcode/blob/main/solution/0000-0099/0076.Minimum Window Substring/README_EN.md

[^95]: https://www.reddit.com/r/learnpython/comments/1dls8dn/medium_leetcode_question_longest_substring/

[^96]: https://www.youtube.com/watch?v=GaXwHThEgGk

[^97]: https://www.youtube.com/watch?v=fTIzYJvhsqg

[^98]: https://www.codeintuition.io/courses/array/tFob_VIXJQJ8bUN3hlSMB

[^99]: https://walkccc.me/LeetCode/problems/76/

[^100]: https://leetcode.com/problems/longest-substring-without-repeating-characters/

[^101]: https://www.reddit.com/r/leetcode/comments/wftiey/how_to_get_better_at_leetcode/

[^102]: https://www.reddit.com/r/learnprogramming/comments/t2gs8m/why_are_these_questions_labeled_as_two_pointers/

[^103]: https://www.reddit.com/r/cscareerquestions/comments/qq9itm/what_separates_leetcode_easy_medium_and_hard/

[^104]: https://www.reddit.com/r/cscareerquestions/comments/yd4rhm/plenty_of_easy_leet_code_problems_are_very/

[^105]: https://www.reddit.com/r/learnprogramming/comments/18x7xa8/why_are_c_pointers_portrayed_as_a_crazy_hard/

[^106]: https://www.reddit.com/r/leetcode/comments/1ew5ts8/what_are_the_easiest_hard_problems/

[^107]: https://www.reddit.com/r/leetcode/comments/13hozor/leetcode_hards_that_are_actually_easy/

[^108]: https://www.reddit.com/r/csharp/comments/1i9x4zi/c_as_first_language/

[^109]: https://www.reddit.com/r/learnprogramming/comments/uf81bw/can_someone_explains_why_the_2_pointers_technique/

[^110]: https://www.reddit.com/r/leetcode/comments/17pyxqy/hard_is_easy_and_easy_is_hard/

[^111]: https://www.reddit.com/r/cscareers/comments/u2u852/im_the_person_that_leetcode_is_designed_to_weed/

[^112]: http://leetcodethehardway.com/tutorials/basic-topics/sliding-window

[^113]: https://www.youtube.com/watch?v=8zBzUAQH5y8

[^114]: https://algo.monster/problems/two_pointers_intro

[^115]: https://www.interviewbit.com/courses/programming/two-pointers/

[^116]: https://www.codechef.com/practice/two-pointers

[^117]: https://www.reddit.com/r/leetcode/comments/12024za/i_cant_solve_twosum/

[^118]: https://www.reddit.com/r/leetcode/comments/1aqu5y2/stuck_on_two_sum_do_people_just_know_how_to_do/

[^119]: https://www.reddit.com/r/leetcode/comments/14eutbs/two_sum_take_away/

[^120]: https://www.reddit.com/r/leetcode/comments/11zsvqs/need_help_on_3sum_problem/

[^121]: https://www.reddit.com/r/leetcode/comments/y3i77k/please_explain_the_twosum_easy_problem_using/

[^122]: https://www.reddit.com/r/dailyprogrammer/comments/3kx6oh/20150914_challenge_232_easy_palindromes/

[^123]: https://www.reddit.com/r/developersIndia/comments/18e6yn9/help_me_with_this_3sum_solution/

[^124]: https://www.reddit.com/r/Cplusplus/comments/qvaw6l/remove_duplicate_point_in_a_vector/

[^125]: https://www.reddit.com/r/learnprogramming/comments/sn7qeu/confused_about_the_hashmap_solution_for_the/

[^126]: https://www.reddit.com/r/learnprogramming/comments/tubv62/palindrome_linked_list/

[^127]: https://www.reddit.com/r/theydidthemath/comments/19eoajn/request_some_youtube_vid_says_both_containers_of/

[^128]: https://www.reddit.com/r/leetcode/comments/1987ewg/3_sum/

[^129]: https://www.reddit.com/r/learnpython/comments/149o0v6/why_does_my_solution_goes_wrong/

[^130]: https://www.reddit.com/r/programming/comments/tfqwml/leetcode_two_sum_solution_explained_coding/

[^131]: https://interviewing.io/questions/two-sum

[^132]: https://builtin.com/articles/solve-two-sum-problem

[^133]: https://dev.to/shavonharrisdev/a-more-straightforward-guide-to-solving-two-sum-2gbi

[^134]: https://coderbyte.com/algorithm/two-sum-problem

[^135]: https://how.dev/answers/how-to-solve-the-two-sum-problem

[^136]: https://dzone.com/articles/the-two-pointers-technique

[^137]: https://interviewing.io/questions/container-with-most-water

[^138]: https://algo.monster/liteproblems/15

[^139]: https://algo.monster/liteproblems/26

[^140]: https://leetcode.com/problems/two-sum/

[^141]: https://algo.monster/liteproblems/680

[^142]: https://takeuforward.org/data-structure/container-with-most-water/

[^143]: https://takeuforward.org/data-structure/3-sum-find-triplets-that-add-up-to-a-zero/

[^144]: https://takeuforward.org/data-structure/remove-duplicates-in-place-from-sorted-array/

[^145]: https://www.youtube.com/watch?v=Ivyh3V4QolA

[^146]: https://stackoverflow.com/questions/76273122/two-pointers-pattern-to-solve-palindrome-problem

[^147]: https://algo.monster/liteproblems/11

[^148]: https://interviewing.io/questions/three-sum

[^149]: https://leetcode.com/problems/remove-duplicates-from-sorted-array/

[^150]: https://leetcode.com/problems/valid-palindrome-ii/

[^151]: https://www.reddit.com/r/programming/comments/kcpkbr/learn_how_the_sliding_window_algorithm_works_step/

[^152]: https://www.reddit.com/r/computerscience/comments/yyudo2/is_it_possible_to_use_a_divide_and_conquer_style/

[^153]: https://www.reddit.com/r/leetcode/comments/14ressl/76_minimum_window_substring_can_anyone_help_me/

[^154]: https://www.reddit.com/r/leetcode/comments/197s48p/here_are_13_interactive_animations_to_help_you/

[^155]: https://www.reddit.com/r/manim/comments/hehz7y/any_tips_on_the_most_efficient_way_to_show/

[^156]: https://www.reddit.com/r/leetcode/comments/zabpz1/utilising_a_stack_data_structure_in_minimum/

[^157]: https://www.reddit.com/r/learnpython/comments/sdgem3/hello_guys_i_need_help_to_understand_the_below/

[^158]: https://www.reddit.com/r/learnprogramming/comments/11v6j7y/effective_leetcode_understanding_the_sliding/

[^159]: https://www.reddit.com/r/leetcode/comments/1euufws/where_did_the_sliding_window_algorithm_come_from/

[^160]: https://www.reddit.com/r/learnprogramming/comments/1ap4yex/solution_for_return_substring_of_string_without/

[^161]: https://www.reddit.com/r/godot/comments/10lp4vc/sliding_window_implementation/

[^162]: https://www.reddit.com/r/leetcode/comments/ve3128/what_are_the_most_common_patterns_in_interviews/

[^163]: https://takeuforward.org/data-structure/sliding-window-technique/

[^164]: https://www.youtube.com/watch?v=QGNAVBn1_bc

[^165]: https://www.freecodecamp.org/news/sliding-window-technique/

[^166]: https://prepinsta.com/leetcode-top-100-liked-questions-with-solution/minimum-window-substring/

[^167]: https://interviewing.io/sliding-window-interview-questions

[^168]: https://nan-archive.vercel.app/sliding-window

[^169]: https://interviewing.io/questions/longest-substring-without-repeating-characters

[^170]: https://www.linkedin.com/pulse/sliding-window-technique-simplified-c-rishabh-singh-1b3te

[^171]: https://dev.to/zzeroyzz/cracking-the-coding-interview-part-2-the-sliding-window-pattern-520d

[^172]: https://media.geeksforgeeks.org/wp-content/uploads/20240306112450/sliding-window-technique-2.webp?sa=X\&ved=2ahUKEwilvIm7zfmMAxVCQ2cHHdk1PQQQ_B16BAgDEAI

[^173]: https://takeuforward.org/data-structure/length-of-longest-substring-without-any-repeating-character/

[^174]: https://github.com/Chanda-Abdul/Several-Coding-Patterns-for-Solving-Data-Structures-and-Algorithms-Problems-during-Interviews/blob/main/✅  Pattern 01 : Sliding Window.md

[^175]: https://www.reddit.com/r/leetcode/comments/1ekpj8h/visualizing_the_5_most_important_leetcode/

[^176]: https://www.reddit.com/user/BluebirdAway5246/comments/1av0lb3/im_working_on_a_free_alternative_to_grokking_the/

[^177]: https://www.reddit.com/r/learnmath/comments/ksm94t/linear_algebra_visualization_tool/

[^178]: https://www.reddit.com/r/learnprogramming/comments/11x4k1z/which_python_library_is_best_to_create/

[^179]: https://www.reddit.com/r/leetcode/comments/1bjob5e/feeling_stuck_or_overwhelmed_not_sure_how_to_best/

[^180]: https://www.reddit.com/r/leetcode/comments/16u20vo/online_courses_that_teach_data_structures_and/

[^181]: https://www.reddit.com/r/software/comments/xq7612/looking_for_a_simple_animation_tool_to_help_me/

[^182]: https://www.reddit.com/r/leetcode/comments/z2jmf3/why_does_this_work/

[^183]: https://www.reddit.com/r/csMajors/comments/1emeu1e/visualizing_the_5_most_important_leetcode/

[^184]: https://www.reddit.com/r/algorithms/comments/p5o134/is_there_a_good_way_to_visualize_recursive/

[^185]: https://www.reddit.com/r/learnprogramming/comments/rliaga/how_can_i_create_a_gui_for_a_visualization_of_my/

[^186]: https://www.reddit.com/r/videos/comments/1jg05y/15_sorting_algorithms_in_6_minutesaudiovisual/

[^187]: https://www.reddit.com/r/leetcode/comments/1gnuj8l/data_structures_and_algorithms_is_it_really_tough/

[^188]: https://www.youtube.com/watch?v=On03HWe2tZM

[^189]: https://usaco.guide/silver/two-pointers

[^190]: https://www.youtube.com/watch?v=cxLhq-tYJsk

[^191]: https://labuladong.online/algo/en/essential-technique/sliding-window-framework/

[^192]: https://daily.dev/blog/algorithm-visualizer-vs-visualgo-comparison

[^193]: https://stackoverflow.com/questions/43722833/qt-step-by-step-algorithm-animation-demonstration

[^194]: https://www.pluralsight.com/resources/blog/guides/algorithm-templates-two-pointers-part-1

[^195]: https://www.14dsa.com/course/two-pointer

[^196]: https://algocademy.com/blog/exploring-algorithm-visualization-tools-enhancing-understanding-and-learning/

[^197]: https://www.restack.io/p/animated-algorithm-learning-tools-answer-creating-animated-tutorials-for-algorithms

[^198]: https://dev.to/liaowow/understanding-the-sliding-window-technique-in-algorithms-206o

[^199]: https://www.designgurus.io/course-play/grokking-the-coding-interview/doc/introduction-to-two-pointers-pattern

[^200]: https://visualgo.net

[^201]: https://www.cs.princeton.edu/~bwk/btl.mirror/anim.cstr132.pdf

[^202]: https://www.youtube.com/watch?v=jM2dhDPYMQM

[^203]: https://www.hellointerview.com/learn/code/two-pointers/overview

[^204]: https://algorithm-visualizer.org

[^205]: https://www.youtube.com/watch?v=JRwTCKjc37o

[^206]: https://www.reddit.com/r/programming/comments/191hbby/are_pointers_just_integers_some_interesting/

[^207]: https://www.reddit.com/r/cpp_questions/comments/1abhyfj/what_do_we_use_pointers_for_and_could_someone/

[^208]: https://www.reddit.com/r/cprogramming/comments/15ftqe2/pointers_pointerc_vs_pointerc_is_the_second/

[^209]: https://www.reddit.com/r/cpp_questions/comments/ttclox/when_learning_cc_i_feel_a_lot_of_time_is_spent_on/

[^210]: https://www.reddit.com/r/MachineLearning/comments/1jfmovl/r_analyzing_failure_modes_in_sliding_windowbased/

[^211]: https://www.reddit.com/r/learnprogramming/comments/11ogtmy/which_book_to_start_learning_data_structures_and/

[^212]: https://www.reddit.com/r/learnprogramming/comments/zh9brb/how_do_you_learn_data_structures_and_algorithms/

[^213]: https://www.reddit.com/r/bioinformatics/comments/1d5pgdw/sliding_window_trimming_criteria/

[^214]: https://www.reddit.com/r/learnprogramming/comments/102slt9/how_the_heck_did_you_guys_who_taught_yourselves/

[^215]: https://www.reddit.com/r/computerscience/comments/1gbxxxd/algorithms_and_data_structures_1_how_to_learn/

[^216]: https://www.reddit.com/r/learnprogramming/comments/r1vszt/almost_every_code_that_i_see_lately_uses_pointers/

[^217]: https://www.reddit.com/r/leetcode/comments/sv82tg/how_do_you_guys_get_good_at_dp/

[^218]: https://www.reddit.com/r/learnprogramming/comments/12dappg/roadmap_to_learn_data_structures_and_algorithms/

[^219]: https://www.reddit.com/r/C_Programming/comments/pk66g8/whats_the_highest_pointer_level_youve_ever_used/

[^220]: https://www.reddit.com/r/leetcode/comments/1bi2uqo/how_to_become_a_beast_at_leetcode_simple_strategy/

[^221]: https://www.reddit.com/r/learnprogramming/comments/mh9tda/math_knowledge_required_for_algorithms/

[^222]: https://www.reddit.com/r/compsci/comments/7mi6ov/what_are_the_most_importantfundamental_concepts/

[^223]: https://algodaily.com/lessons/using-the-two-pointer-technique

[^224]: https://www.youtube.com/watch?v=-gjxg6Pln50

[^225]: https://www.linkedin.com/pulse/two-pointers-design-approach-manas-rath-y5cdc

[^226]: https://www.youtube.com/watch?v=8hly31xKli0

[^227]: https://www.worldscientific.com/worldscibooks/10.1142/12298

[^228]: https://www.slideshare.net/slideshow/two-pointers-technique-mostafa-saad-pptx/268792884

[^229]: https://www.designgurus.io/answers/detail/how-do-we-write-an-algorithm

[^230]: https://schools.zenva.com/teaching-algorithms/

[^231]: https://www.youtube.com/watch?v=syTs9_w-pwA

[^232]: https://www.architectalgos.com/unlocking-the-power-of-sliding-window-pattern-c1d512981b5e

[^233]: https://blog.devgenius.io/step-by-step-guideline-to-write-an-algorithm-4bc4c08d7c65

[^234]: https://dbrau.ac.in/wp-content/uploads/2023/05/Basics-of-Alogrithm.pdf

[^235]: https://www.scribd.com/document/348121319/Two-Pointers

[^236]: https://quanticdev.com/algorithms/dynamic-programming/sliding-window/

[^237]: https://algocademy.com/blog/an-introduction-to-algorithm-design-patterns/

[^238]: https://www.khanacademy.org/computing/computer-science/algorithms

[^239]: https://www.reddit.com/r/vim/comments/3rvbd6/vim_daily_tips_tricks_newsletter_for_progressive/

[^240]: https://www.reddit.com/r/leetcode/comments/1ilemw0/please_help_me_decide_on_a_course_a_roadmap_for/

[^241]: https://www.reddit.com/r/mlscaling/comments/1cz3vsu/why_not_combine_curriculum_learning_and/

[^242]: https://www.reddit.com/r/learnmachinelearning/comments/1ciqvsb/problem_when_training_progressive_gan_for_cifar10/

[^243]: https://www.reddit.com/r/leetcode/comments/vw696l/from_complete_beginner_to_solving_500_questions/

[^244]: https://www.reddit.com/r/bioinformatics/comments/742he9/why_is_the_progressive_multiple_sequence/

[^245]: https://www.reddit.com/r/math/comments/18war9p/examples_of_simple_algorithms_with_ridiculously/

[^246]: https://www.reddit.com/r/roguelikedev/comments/lb5g09/a_simpler_pathfinding_algorithm/

[^247]: https://www.reddit.com/r/cscareerquestions/comments/18r139s/lc_progression_advice_from_a_slow_learner_who_was/

[^248]: https://www.reddit.com/r/computerscience/comments/1gs4vnn/pen_paper_algorithm_tutorials_for_youtube_would/

[^249]: https://www.reddit.com/r/explainlikeimfive/comments/5qsay3/eli5_algorithm_time_complexity/

[^250]: https://www.reddit.com/r/ChatGPT/comments/13qnl97/i_made_this_prompt_to_learn_about_new_things_in_a/

[^251]: https://www.reddit.com/r/leetcode/comments/1gm15ru/the_trick_to_leetcode/

[^252]: https://www.reddit.com/r/leetcode/comments/wlqwj3/how_much_time_to_spend_on_one_leetcode_problem/

[^253]: https://www.reddit.com/r/math/comments/21zdln/the_common_core_is_corrupting_school_mathematics/

[^254]: https://www.reddit.com/r/learnprogramming/comments/v6obt3/how_to_figure_out_the_time_complexity_of_my_code/

[^255]: https://www.reddit.com/r/learnprogramming/comments/ssrpgw/anyone_else_find_themselves_simply_memorizing/

[^256]: https://www.pranathiss.com/blog/ml-progressive-learning/

[^257]: https://www.sciencedirect.com/science/article/abs/pii/S0893608020301817

[^258]: https://github.com/antononcube/MathematicaVsR/blob/master/Projects/ProgressiveMachineLearning/Mathematica/Progressive-machine-learning-examples.md

[^259]: https://ietresearch.onlinelibrary.wiley.com/doi/full/10.1049/iet-ipr.2020.0166

[^260]: https://arxiv.org/pdf/1609.00085.pdf

[^261]: https://algo.monster/liteproblems/239

[^262]: https://www.pnas.org/doi/10.1073/pnas.0409137102

[^263]: https://www.simplilearn.com/tutorials/data-structure-tutorial/algorithm-complexity-in-data-structure

[^264]: https://cacm.acm.org/news/historic-algorithms-help-unlock-shortest-path-problem-breakthrough/

[^265]: https://www.mdpi.com/2071-1050/14/9/5191

[^266]: https://tau.edu.ng/assets/media/docs/algorithms-and-complexity-analysis-csc304_1716907183.pdf

[^267]: https://www.bu.edu/econ/files/2017/04/Ortner-Progressive.pdf

[^268]: https://www.sciencedirect.com/science/article/abs/pii/S1084804521002472

[^269]: https://daily.dev/blog/mastering-algorithm-complexity-time-and-space-optimization

[^270]: https://www.sciencedirect.com/science/article/abs/pii/S0309170822001233

