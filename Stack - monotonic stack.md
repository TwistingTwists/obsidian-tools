# Monotonic Stack - Complete Guide

## The Core Concept

A **monotonic stack** is a stack where elements are always in monotonically increasing or monotonically decreasing order. The key idea is maintaining this property by **popping elements that violate the monotonic condition** before pushing new elements.

### Two Types

**1. Monotonic Increasing Stack**
- Elements from bottom to top are in increasing order
- When a new element comes, pop all elements greater than it
- Helps find: **previous/next smaller elements**

**2. Monotonic Decreasing Stack**
- Elements from bottom to top are in decreasing order  
- When a new element comes, pop all elements smaller than it
- Helps find: **previous/next greater elements**

### Why Is This Powerful?

The magic happens during popping:
- **When we pop an element**, we've just found its "boundary" (next greater/smaller)
- **What remains on stack top** is the previous greater/smaller element
- This transforms O(n²) problems into O(n) solutions

### Visual Example

Let's trace through `[3, 1, 4, 2, 5]` with a **monotonic increasing stack**:

```
Step 0: Push 3
Stack: [3]

Step 1: Push 1 (1 < 3, so pop 3 first)
Pop 3 → next smaller for 3 is 1
Stack: [1]

Step 2: Push 4 (4 > 1, just push)
Stack: [1, 4]

Step 3: Push 2 (2 < 4, pop 4)
Pop 4 → next smaller for 4 is 2
Stack: [1, 2]

Step 4: Push 5 (5 > 2, just push)
Stack: [1, 2, 5]

Result: Next smaller elements
3 → 1
4 → 2
Others → -1 (no next smaller)
```

---

## Example Problems with Detailed Solutions

### **Problem 1: Next Greater Element**

**Problem:** For each element, find the next element greater than it.

```
Input: [2, 1, 2, 4, 3]
Output: [4, 2, 4, -1, -1]
```

**Solution:** Use monotonic **decreasing** stack

```python
def nextGreaterElement(nums):
    n = len(nums)
    result = [-1] * n
    stack = []  # Stores indices
    
    for i in range(n):
        # Pop smaller elements - we found their next greater!
        while stack and nums[stack[-1]] < nums[i]:
            idx = stack.pop()
            result[idx] = nums[i]
        
        stack.append(i)
    
    return result

# Why it works:
# - Stack maintains decreasing order
# - When we find a larger element, all smaller ones on stack 
#   have found their "next greater"
# - Time: O(n), each element pushed/popped once
```

---

### **Problem 2: Daily Temperatures**

**Problem:** Given daily temperatures, return how many days until warmer temperature.

```
Input: [73, 74, 75, 71, 69, 72, 76, 73]
Output: [1,  1,  4,  2,  1,  1,  0,  0]
```

**Solution:** Monotonic decreasing stack (variant of next greater)

```python
def dailyTemperatures(temperatures):
    n = len(temperatures)
    answer = [0] * n
    stack = []
    
    for i in range(n):
        # Pop all days with lower temperature
        while stack and temperatures[stack[-1]] < temperatures[i]:
            prev_day = stack.pop()
            answer[prev_day] = i - prev_day  # Days difference
        
        stack.append(i)
    
    return answer

# Key insight: Instead of storing the value, store the INDEX
# This lets us calculate distance directly
```

---

### **Problem 3: Largest Rectangle in Histogram**

**Problem:** Find the largest rectangle area in a histogram.

```
Input: heights = [2, 1, 5, 6, 2, 3]
Output: 10  (rectangle with height 5, width 2)
```

**Solution:** Monotonic **increasing** stack — the crown jewel!

```python
def largestRectangleArea(heights):
    stack = []
    max_area = 0
    heights.append(0)  # Sentinel to flush stack
    
    for i, h in enumerate(heights):
        # When we find a shorter bar, calculate areas for taller bars
        while stack and heights[stack[-1]] > h:
            height_idx = stack.pop()
            height = heights[height_idx]
            
            # Width is between previous smaller and current smaller
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        
        stack.append(i)
    
    return max_area

# Why this works:
# - For each bar, we find: left boundary (prev smaller) and 
#   right boundary (next smaller)
# - When we pop, stack top gives left boundary, current i gives right
# - The popped bar can extend between these boundaries
```

**Visual Walkthrough:**
```
Heights: [2, 1, 5, 6, 2, 3, 0]
         
Index 0 (h=2): Push 0
Stack: [0]

Index 1 (h=1): Pop 0 (height=2)
  Width = 1 - (-1) - 1 = 1, Area = 2*1 = 2
  Push 1
Stack: [1]

Index 2 (h=5): Push 2
Stack: [1, 2]

Index 3 (h=6): Push 3
Stack: [1, 2, 3]

Index 4 (h=2): Pop 3 (height=6)
  Width = 4 - 2 - 1 = 1, Area = 6*1 = 6
  Pop 2 (height=5)
  Width = 4 - 1 - 1 = 2, Area = 5*2 = 10 ← Maximum!
  Push 4
Stack: [1, 4]
...
```

---

### **Problem 4: Trapping Rain Water**

**Problem:** Calculate water trapped between bars.

```
Input: [0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]
Output: 6
```

**Solution:** Monotonic **decreasing** stack (by tracking boundaries)

```python
def trap(height):
    stack = []
    water = 0
    
    for i, h in enumerate(height):
        # When we find a taller bar, water can be trapped
        while stack and height[stack[-1]] < h:
            bottom_idx = stack.pop()
            
            if not stack:
                break
            
            # Water is bounded by current bar and previous bar
            left_idx = stack[-1]
            bounded_height = min(height[left_idx], h) - height[bottom_idx]
            width = i - left_idx - 1
            water += bounded_height * width
        
        stack.append(i)
    
    return water

# Key insight: Water trapped forms "layers"
# Each pop represents finding the bottom of a water section
```

---

### **Problem 5: Sum of Subarray Minimums**

**Problem:** Find sum of minimum elements in all subarrays.

```
Input: [3, 1, 2, 4]
Output: 17
Explanation:
Subarrays: [3],[1],[2],[4],[3,1],[1,2],[2,4],[3,1,2],[1,2,4],[3,1,2,4]
Minimums:   3,  1,  2,  4,   1,    1,    2,     1,      1,       1
Sum = 17
```

**Solution:** Find contribution of each element as minimum

```python
def sumSubarrayMins(arr):
    n = len(arr)
    MOD = 10**9 + 7
    
    # For each element, find how many subarrays it's the minimum
    # Need: previous smaller (left) and next smaller (right)
    
    # Previous Less or Equal (to handle duplicates)
    left = [0] * n
    stack = []
    for i in range(n):
        while stack and arr[stack[-1]] > arr[i]:
            stack.pop()
        left[i] = i - stack[-1] if stack else i + 1
        stack.append(i)
    
    # Next Less (strict)
    right = [0] * n
    stack = []
    for i in range(n - 1, -1, -1):
        while stack and arr[stack[-1]] >= arr[i]:
            stack.pop()
        right[i] = stack[-1] - i if stack else n - i
        stack.append(i)
    
    # Calculate contribution
    result = 0
    for i in range(n):
        result += arr[i] * left[i] * right[i]
        result %= MOD
    
    return result

# Why: Element at index i is minimum in left[i] * right[i] subarrays
# Its contribution = arr[i] * (number of subarrays where it's min)
```

---

### **Problem 6: Remove K Digits**

**Problem:** Remove k digits to make the smallest number.

```
Input: num = "1432219", k = 3
Output: "1219"
```

**Solution:** Monotonic **increasing** stack (greedy removal)

```python
def removeKdigits(num, k):
    stack = []
    
    for digit in num:
        # Remove larger digits to make number smaller
        while k > 0 and stack and stack[-1] > digit:
            stack.pop()
            k -= 1
        stack.append(digit)
    
    # Remove remaining k digits from end
    stack = stack[:-k] if k > 0 else stack
    
    # Remove leading zeros and handle empty case
    result = ''.join(stack).lstrip('0')
    return result if result else '0'

# Greedy insight: Always remove larger digits that appear before smaller ones
# Monotonic stack naturally maintains increasing order
```

---

## Extensions and Advanced Patterns

### **Extension 1: Circular Arrays (Next Greater Element II)**

**Problem:** Find next greater in circular array.

```
Input: [1, 2, 1]
Output: [2, -1, 2]  (circular: 1→2, 2→1(wrapped), 1→2)
```

**Solution:** Traverse array twice

```python
def nextGreaterElements(nums):
    n = len(nums)
    result = [-1] * n
    stack = []
    
    # Process array twice (simulate circular)
    for i in range(2 * n):
        idx = i % n
        while stack and nums[stack[-1]] < nums[idx]:
            result[stack.pop()] = nums[idx]
        
        if i < n:  # Only push first iteration
            stack.append(idx)
    
    return result
```

---

### **Extension 2: 2D Extension (Maximal Rectangle)**

**Problem:** Find largest rectangle in binary matrix.

```python
def maximalRectangle(matrix):
    if not matrix:
        return 0
    
    max_area = 0
    heights = [0] * len(matrix[0])
    
    for row in matrix:
        # Update heights (treat as histogram)
        for i in range(len(row)):
            heights[i] = heights[i] + 1 if row[i] == '1' else 0
        
        # Use histogram algorithm
        max_area = max(max_area, largestRectangleArea(heights))
    
    return max_area

# Extension: Apply 1D histogram solution to each row
```

---

### **Extension 3: Range Queries (Sliding Window Maximum)**

**Problem:** Find maximum in each sliding window of size k.

```python
from collections import deque

def maxSlidingWindow(nums, k):
    result = []
    dq = deque()  # Monotonic decreasing deque
    
    for i, num in enumerate(nums):
        # Remove elements outside window
        while dq and dq[0] < i - k + 1:
            dq.popleft()
        
        # Maintain decreasing order
        while dq and nums[dq[-1]] < num:
            dq.pop()
        
        dq.append(i)
        
        if i >= k - 1:
            result.append(nums[dq[0]])
    
    return result

# Uses deque (double-ended queue) for window management
```

---

### **Extension 4: Contribution Counting**

Pattern: Count how many times each element contributes as min/max.

**Applications:**
- Sum of Subarray Minimums
- Sum of Subarray Ranges
- Number of Visible People in Queue

**Template:**
```python
def contribution_pattern(arr):
    # Find left boundary (previous smaller/greater)
    left = find_previous_boundary(arr)
    
    # Find right boundary (next smaller/greater)
    right = find_next_boundary(arr)
    
    # Calculate contribution
    result = 0
    for i in range(len(arr)):
        count = left[i] * right[i]  # Subarrays where arr[i] is min/max
        result += arr[i] * count
    
    return result
```

---

### **Extension 5: Online/Streaming Version**

**Stock Span (Online):**
```python
class StockSpanner:
    def __init__(self):
        self.stack = []  # (price, span)
    
    def next(self, price):
        span = 1
        
        while self.stack and self.stack[-1][0] <= price:
            span += self.stack.pop()[1]
        
        self.stack.append((price, span))
        return span

# Handles streaming data efficiently
```

---

## Common Patterns Summary

| Pattern | Stack Type | Use Case |
|---------|-----------|----------|
| Next Greater | Decreasing | Find next larger element |
| Next Smaller | Increasing | Find next smaller element |
| Prev Greater | Decreasing | Find previous larger |
| Prev Smaller | Increasing | Find previous smaller |
| Histogram | Increasing | Area calculations |
| Remove Duplicates | Increasing/Decreasing | Lexicographic optimization |
| Water Trapping | Decreasing | Boundary-based calculations |

## Key Insights

1. **Direction matters:** Process left-to-right for "next", right-to-left for "previous"

2. **Strict vs non-strict:** Use `<` vs `<=` to handle duplicates correctly

3. **Store indices, not values:** Enables distance calculations

4. **Sentinel values:** Adding 0 or infinity at end can simplify logic

5. **The moment of popping is powerful:** That's when we've found the boundary!

Would you like me to explore any specific extension in more detail, or work through another problem type?