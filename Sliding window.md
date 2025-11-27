

Level 1: Fixed-Size Window
This is the most basic form of the sliding window. The window size is always fixed (e.g., size k). You just slide this fixed-size window across the data, one step at a time.
 * LeetCode 643: Maximum Average Subarray I (Easy)
   * Goal: Find the maximum average of all contiguous subarrays of a fixed size k.
   * Why it's a good start: This is the canonical "fixed window" problem. You calculate the sum of the first k elements. Then, as you slide the window one position to the right, you add the new element (at the right) and subtract the old element (that just fell off the left). This O(1) update is the core concept.
 * LeetCode 1456: Maximum Number of Vowels in a Substring of Given Length (Medium)
   * Goal: Find the maximum number of vowel letters in any substring of size k.
   * Progression: This is the same pattern as the previous problem, but instead of tracking a sum, you track a count (of vowels). As you slide, you check if the new element (entering) is a vowel and if the old element (leaving) was a vowel, and update the count accordingly.


Level 2: Variable-Size Window (Dynamic Sizing)
This is the most common and powerful pattern. The window grows and shrinks based on a condition (e.g., "at most 2 distinct characters," "sum is less than target").
You'll typically use a loop to move the right pointer (expanding the window) and an inner while loop to move the left pointer (shrinking the window) whenever a condition is violated.
📈 Sub-level 2a: Simple Condition
 * LeetCode 209: Minimum Size Subarray Sum (Medium)
   * Goal: Find the minimal length of a contiguous subarray whose sum is greater than or equal to a target.
   * Progression: This is the perfect introduction to variable windows.
     * Expand (move right): Keep adding nums[right] to your current_sum.
     * Shrink (move left): While your current_sum >= target, you have a valid window. Record its length (right - left + 1) and try to make it smaller by shrinking from the left (subtracting nums[left] and incrementing left).
 * LeetCode 3: Longest Substring Without Repeating Characters (Medium)
   * Goal: Find the length of the longest substring without repeating characters.
   * Progression: This introduces the most common tool for sliding windows: a hash map (or set) to track character frequencies or presence within the current window.
     * Expand: Add char[right] to your map/set.
     * Shrink: While char[right] is already in your map/set (meaning you have a repeat), remove char[left] from the map/set and increment left.
🧩 Sub-level 2b: Complex Condition (Frequency Matching)
These problems require you to track the exact frequencies of multiple characters.
 * LeetCode 904: Fruit Into Baskets (Medium)
   * Goal: Find the longest subarray with at most two distinct types of fruit (characters).
   * Progression: This is a more specific version of the last problem. You use a hash map to store the counts of the characters in your window.
     * Expand: Add char[right] to the map.
     * Shrink: While the map.size() > 2 (you have more than 2 distinct fruits), remove char[left] from the map (decrementing its count, and removing it entirely if the count hits 0) and increment left.
 * LeetCode 567: Permutation in String (Medium)
   * Goal: Check if s2 contains a permutation of s1.
   * Progression: This is a brilliant problem that combines a fixed-size window (the size of s1) with the hash map frequency pattern. You create a frequency map for s1. Then, you slide a window of size s1.length() across s2, maintaining a frequency map for your current window. If the two maps are ever identical, you've found a permutation.


Level 3: Advanced & Complex Windows
These problems build on the previous patterns but add a new layer of complexity, often requiring an auxiliary data structure or a more complex shrinking/matching logic.
 * LeetCode 76: Minimum Window Substring (Hard)
   * Goal: Given s and t, find the smallest substring in s which contains all the characters of t.
   * Progression: This is the "boss" of hash map-based sliding windows. You create a frequency map for all characters in t. You then use a counter (e.g., required_matches) to track how many of t's characters you have matched in your current window in s.
     * Expand: Add char[right]. If it's a character from t and its count in your window now matches its required count in t, increment required_matches.
     * Shrink: While required_matches equals the total number of unique chars in t (meaning your window is valid), record the length. Then, try to shrink by removing char[left]. If char[left] was a required character and its count now drops below the required count, decrement required_matches.
 * LeetCode 239: Sliding Window Maximum (Hard)
   * Goal: Find the maximum value in each contiguous subarray of size k.
   * Progression: This problem is different. You can't just use a sum or a simple map. If you slide a window and the maximum element drops off the left, you have to re-scan the whole window to find the new maximum. This is too slow.
   * The Trick: The optimal solution uses a Deque (Double-Ended Queue). The deque is used to store indices of elements, and it's kept in decreasing order of their values. This way, the maximum element in the current window is always at the front of the dequeHere is a progression-based study list of LeetCode-style sliding-window problems.


---

1. Fixed-Length Window (warm-up)

#	Problem	Goal	
1	[Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)	Compute avg of every k elements	
2	[Substrings of Size Three with Distinct Characters](https://leetcode.com/problems/substrings-of-size-three-with-distinct-characters/)	Count 3-letter substrings with unique chars	
3	[Number of Sub-arrays of Size K and Average ≥ Threshold](https://leetcode.com/problems/number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold/)	Combine sum + avg condition	
4	[Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)	Max in every window (intro to deque)	

---

2. Variable-Length Window (classic 2-pointer)

#	Problem	Key Idea	
5	[Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)	Smallest subarray with sum ≥ target	
6	[Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)	Expand & shrink hash-set	
7	[Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets/)	At most 2 distinct integers	
8	[Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)	Max substring with ≤ k changes	
9	[Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)	Flip ≤ k zeroes	

---

3. Exact / At-Most k Tricks (subtraction trick)

#	Problem	Pattern	
10	[Subarrays with K Different Integers](https://leetcode.com/problems/subarrays-with-k-different-integers/)	`atMost(k) − atMost(k-1)`	
11	[Count Number of Nice Subarrays](https://leetcode.com/problems/count-number-of-nice-subarrays/)	Exactly k odd numbers	
12	[Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum/)	Exactly k ones	

---

4. String Permutation & Anagrams (hash-map counter)

#	Problem	Skill	
13	[Permutation in String](https://leetcode.com/problems/permutation-in-string/)	Anagram of s1 in s2	
14	[Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)	Return all start indices	
15	[Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)	Smallest window covering t	

---

5. Advanced / Expert (multiple constraints)

#	Problem	Why It’s Hard	
16	[Shortest Subarray with Sum at Least K](https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/)	Monotonic deque + prefix sum	
17	[Longest Continuous Subarray With Absolute Diff ≤ Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)	Two monotonic deques	
18	[Replace the Substring for Balanced String](https://leetcode.com/problems/replace-the-substring-for-balanced-string/)	Balance freq of chars outside window	
19	[Count Subarrays Where Max Element Appears at Least K Times](https://leetcode.com/problems/count-subarrays-where-max-element-appears-at-least-k-times/)	Track max & its frequency	
20	[Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)	Product instead of sum	

---

6. Optional “Boss” Tier (interview killers)
- [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/) (two multisets)  
- [Constrained Subsequence Sum](https://leetcode.com/problems/constrained-subsequence-sum/) (dp + deque)  
- [Number of Substrings Containing All Three Characters](https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/) (three-counter trick)  

---

How to use the list
1. Solve every problem in order inside a group before jumping to the next group.  
2. Code the template once:  
   
```python
   left = 0
   for right in range(n):
       window.add(s[right])
       while invalid(window):
           window.remove(s[left])
           left += 1
       update_answer()
   ```

3. For “exactly k” problems, always write `at_most(k)` first, then subtract.  
4. Track your runtime & space—aim for O(n) time & O(1) or O(k) space.

