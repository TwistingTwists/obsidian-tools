---
tags:
  - leetcode
---

[[Overview Patterns of Binary Search]]

[[Binary Search Matrix Problems]]

Here is a **comprehensive code walkthrough** of **binary search variations** using **LeetCode, Codeforces, and HackerRank examples**, covering all major **use cases** discussed in the uploaded PDF:

---

# 🔍 1. **Binary Search in Rotated Sorted Arrays**
### 📌 Problem: [LeetCode #33 - Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
> Given a rotated sorted array (sorted but rotated at an unknown pivot), find the index of a target value.

## ✅ Key Idea:
At each step, determine which half is sorted and check if the target lies within that half.

```python
def search(nums, target):
    left, right = 0, len(nums) - 1

    while left <= right:
        mid = (left + right) // 2

        if nums[mid] == target:
            return mid

        # Handle duplicate edge case where nums[left] == nums[mid] == nums[right]
        if nums[left] == nums[mid] == nums[right]:
            left += 1
            right -= 1
        elif nums[left] <= nums[mid]:  # Left part is sorted
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:  # Right part is sorted
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1

    return -1
```

### 🧠 Explanation:
- The rotation breaks full monotonicity.
- We compare `nums[mid]` with `nums[left]` to detect the sorted half.
- In the presence of duplicates, we shrink the bounds (`left += 1`, `right -= 1`) to avoid ambiguity.

---

# 🎯 2. **Finding Peak Element**
### 📌 Problem: [LeetCode #162 - Find Peak Element](https://leetcode.com/problems/find-peak-element/)
> A peak element is strictly greater than its neighbors. Return any peak's index.

```python
def findPeakElement(nums):
    left, right = 0, len(nums) - 1

    while left < right:
        mid = (left + right) // 2

        # Compare mid and mid+1 to decide direction
        if nums[mid] > nums[mid + 1]:
            right = mid  # peak might be on the left side
        else:
            left = mid + 1  # peak might be on the right side

    return left  # both pointers meet at a peak
```

### 🧠 Explanation:
- Binary search doesn't require a fully sorted array.
- Based on whether `nums[mid] > nums[mid+1]`, we know the peak must lie to the **left** or **right**.
- This approach works in **O(log n)** time even on unsorted arrays.

---

# 🔢 3. **First and Last Occurrence (LowerBound & UpperBound)**
### 📌 Problem: [LeetCode #34 - Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```python
def searchRange(nums, target):
    def lower_bound():
        left, right = 0, len(nums) - 1
        while left < len(nums) and nums[left] != target:
            left += 1
        return left

    def upper_bound():
        left, right = 0, len(nums) - 1
        while left < len(nums) and nums[left] <= target:
            left += 1
        return left

    first = lower_bound()
    last = upper_bound() - 1
    if first < len(nums) and nums[first] == target:
        return [first, last]
    return [-1, -1]
```

### 🧠 Explanation:
- `lower_bound()` finds the **first occurrence**.
- `upper_bound()` finds the **index after the last occurrence**.
- Combine them to get the result range.
- C++ STL equivalents: `std::lower_bound`, `std::upper_bound`.

---

# ⚖️ 4. **Optimization Using Binary Search (Minimize Max Value)**
### 📌 Problem: [LeetCode #410 - Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)

> Split array into `k` contiguous subarrays such that the largest sum among these subarrays is as small as possible.

```python
def splitArray(nums, k):
    def canSplit(max_sum):
        count = 1
        curr_sum = 0
        for num in nums:
            if curr_sum + num > max_sum:
                count += 1
                curr_sum = num
                if count > k:
                    return False
            else:
                curr_sum += num
        return True

    low, high = max(nums), sum(nums)
    ans = high

    while low <= high:
        mid = (low + high) // 2
        if canSplit(mid):
            ans = mid
            high = mid - 1
        else:
            low = mid + 1

    return ans
```

### 🧠 Explanation:
- Use binary search over possible answers: `[max(nums), sum(nums)]`.
- Helper function `canSplit(mid)` checks feasibility (whether it's possible to split array with maximum subarray sum ≤ `mid`).
- This is a **classic optimization via binary search** technique.
- Time complexity: O(n log s), where `s = sum(nums)`.

---

# ✔️ 5. **Feasibility Testing With Binary Search**
### 📌 Problem: [Codeforces #1606C - Banknotes](https://codeforces.com/problemset/problem/1606/C)

> Choose `k` banknotes such that every number from 1 up to some `x` can be formed. Maximize `x`.

```cpp
bool is_possible(long long x, int k, vector<long long>& b) {
    long long cnt = 0;
    for (int i = 0; i < b.size(); i++) {
        cnt += x / b[i];
        if (cnt >= k)
            return true;
    }
    return cnt >= k;
}

long long solve(int k, vector<long long>& b) {
    long long low = 1, high = LLONG_MAX, ans = 0;
    while (low <= high) {
        long long mid = (low + high) / 2;
        if (is_possible(mid, k, b)) {
            ans = mid;
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}
```

### 🧠 Explanation:
- Binary search over the answer space (possible values of `x`).
- Feasibility function checks how many unique values can be formed with `k` banknotes.
- Can also be used for interactive problems like those on Codeforces.

---

# 🪜 6. **Binary Search on Answer Space (Interactive Problems)**
### 📌 Problem: [Codeforces #2106G2 - Baudelaire (Hard Version)](https://codeforces.com/contest/2106/problem/G2)

> Interactive problem where you need to guess a hidden number using limited queries.

```cpp
int query(int l, int r) {
    cout << "? " << l << " " << r << endl;
    int res;
    cin >> res;
    return res;
}

int find_hidden_number(int n) {
    int left = 1, right = n;
    while (left < right) {
        int mid = (left + right) / 2;
        int res = query(left, mid);
        if (res == -1) return -1;

        if (res % 2 == 0) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

### 🧠 Explanation:
- This is a classic **interactive binary search** pattern.
- You make a decision based on the parity or response of the query.
- It’s widely used in Codeforces interactive problems.

---

# 📈 7. **Binary Search with Custom Conditions (e.g., Maximize/Minimize)**
### 📌 Problem: [HackerRank - Sherlock and Cost](https://www.hackerrank.com/challenges/sherlock-and-cost/problem)

> Maximize absolute differences between adjacent elements under constraints.

### ✨ Binary Search not directly applicable here, but similar logic can be applied when minimizing/maximizing under complex conditions.

For example, in greedy DP solutions:

```python
def cost(arr):
    max1 = 0  # Assume arr[i] = 1
    max2 = 0  # Assume arr[i] = current value
    for i in range(1, len(arr)):
        temp = max(max1, max2 + abs(arr[i] - 1))
        max2 = max(max1 + abs(arr[i] - arr[i-1]), max2)
        max1 = temp
    return max(max1, max2)
```

While this isn't binary search, the **concept of narrowing down optimal ranges** aligns with binary search-based optimization techniques.

---

# 📊 8. **Binary Search on Real Numbers (Floating Point Binary Search)**
### 📌 Problem: [LeetCode #786 - K-th Smallest in Lexicographical Order](https://leetcode.com/problems/k-th-smallest-in-lexicographical-order/)

> Binary search applied to real numbers or large integer spaces.

```python
def findKthNumber(n, k):
    def count(num1, num2):
        steps = 0
        while num1 <= n:
            steps += min(n + 1, num2) - num1
            num1 *= 10
            num2 *= 10
        return steps

    curr = 1
    k -= 1  # We start counting from the first node

    while k > 0:
        gap = count(curr, curr + 1)
        if gap <= k:
            k -= gap
            curr += 1
        else:
            k -= 1
            curr *= 10

    return curr
```

### 🧠 Explanation:
- While not traditional floating point BS, this uses **digit-wise traversal**, akin to binary search on digits.
- Used in lexicographical order, BFS trees, tries, etc.

---

# 🧩 9. **Binary Search Over Bit Manipulation**
### 📌 Problem: [Codeforces #1606E - Rank Reduction](https://codeforces.com/contest/1606/problem/E)

> Fix bits from MSB to LSB to satisfy a condition.

```cpp
int reduce_rank(vector<int>& basis, int target_rank) {
    int res = 0;
    for (int bit = 30; bit >= 0; --bit) {
        if ((target_rank >> bit) & 1) {
            res |= (1 << bit);
        }
    }
    return res;
}
```

### 🧠 Explanation:
- Binary search over bits instead of ranges.
- Common in bitmasking and interactive binary search problems.

---

# 🛠️ 10. **Binary Search + Greedy Strategy**
### 📌 Problem: [LeetCode #1011 - Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)

```python
def shipWithinDays(weights, days):
    def can_ship(capacity):
        days_needed = 1
        current_load = 0
        for weight in weights:
            if current_load + weight > capacity:
                days_needed += 1
                current_load = weight
            else:
                current_load += weight
            if days_needed > days:
                return False
        return True

    low, high = max(weights), sum(weights)
    while low < high:
        mid = (low + high) // 2
        if can_ship(mid):
            high = mid
        else:
            low = mid + 1

    return low
```

### 🧠 Explanation:
- Binary search over possible capacities.
- Greedy helper function checks if a given capacity allows shipping within required days.
- Widely used in scheduling, resource allocation, and logistics.

---

# 🧾 Summary Table of Binary Search Variations

| Use Case | Example Problem | Key Technique |
|----------|------------------|----------------|
| Rotated Sorted Array | LeetCode #33 | Identify sorted half |
| Peak Element | LeetCode #162 | Compare mid vs mid+1 |
| First/Last Occurrence | LeetCode #34 | LowerBound/UpperBound |
| Optimization | LeetCode #410 | Binary search on answer |
| Feasibility Testing | Codeforces #1606C | Feasible predicate |
| Interactive Binary Search | CF #2106G2 | Query responses guide search |
| Floating Point Binary Search | LeetCode #786 | Digit-level traversal |
| Bitwise Binary Search | CF #1606E | Fix bits iteratively |
| Binary Search + Greedy | LeetCode #1011 | Validate greedy condition |

---

# 🧠 Final Tips:
- Always define your **search space** clearly.
- Understand what defines a **feasible solution**.
- Handle **edge cases**: empty arrays, single elements, duplicates.
- Avoid off-by-one errors by carefully managing loop boundaries.


Here are **10 hard binary search problems** from **LeetCode, Codeforces, and HackerRank**, along with **detailed explanations**, **key insights**, and **code walkthroughs**. These problems represent advanced use cases of binary search including **optimization under constraints**, **interactive binary search**, **real number precision**, **bitwise binary search**, and **multi-dimensional feasibility testing**.

---

## 🔹 1. **[LeetCode #410 - Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)**
### 📌 Problem:
Split an array into `k` contiguous subarrays so that the **largest sum** among those subarrays is as small as possible.

### 💡 Strategy:
Use binary search on the answer space: `[max(nums), sum(nums)]`. For each candidate value, check if it's possible to split the array such that no subarray exceeds it.

### ✅ Solution:
```python
def splitArray(nums, k):
    def canSplit(max_sum):
        count = 1
        curr_sum = 0
        for num in nums:
            if curr_sum + num > max_sum:
                count += 1
                curr_sum = num
                if count > k:
                    return False
            else:
                curr_sum += num
        return True

    low, high = max(nums), sum(nums)
    ans = high

    while low <= high:
        mid = (low + high) // 2
        if canSplit(mid):
            ans = mid
            high = mid - 1
        else:
            low = mid + 1

    return ans
```

### 🧠 Insight:
- Binary search over possible answers.
- Greedy function to validate feasibility.

---

## 🔹 2. **[LeetCode #786 - K-th Smallest in Lexicographical Order](https://leetcode.com/problems/k-th-smallest-in-lexicographical-order/)**
### 📌 Problem:
Find the k-th smallest number in lexicographical order from 1 to n.

### 💡 Strategy:
Digit-wise traversal using a technique similar to binary search — count how many numbers lie between prefixes.

### ✅ Solution:
```python
def findKthNumber(n, k):
    def count_steps(a, b):
        steps = 0
        while a <= n:
            steps += min(n + 1, b) - a
            a *= 10
            b *= 10
        return steps

    curr = 1
    k -= 1
    while k > 0:
        gap = count_steps(curr, curr + 1)
        if gap <= k:
            k -= gap
            curr += 1
        else:
            k -= 1
            curr *= 10
    return curr
```

### 🧠 Insight:
- Instead of binary search on values, simulate binary search on digits or prefixes.
- Used in trie-based problems and DFS enumeration.

---

## 🔹 3. **[Codeforces #2106G2 - Baudelaire (Hard Version)](https://codeforces.com/contest/2106/problem/G2)**
### 📌 Problem:
Interactive problem where you need to guess a hidden number using limited queries.

### 💡 Strategy:
Binary search with real-time feedback.

### ✅ Solution:
```cpp
int query(int l, int r) {
    cout << "? " << l << " " << r << endl;
    int res; cin >> res;
    return res;
}

int find_hidden(int n) {
    int left = 1, right = n;
    while (left < right) {
        int mid = (left + right) / 2;
        int res = query(left, mid);
        if (res == -1) return -1;

        if (res % 2 == 0) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

### 🧠 Insight:
- Classic interactive binary search.
- Decision based on parity of response.

---

## 🔹 4. **[LeetCode #658 - Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements/)**
### 📌 Problem:
Given a sorted array and two integers `k` and `x`, return the `k` closest elements to `x`.

### 💡 Strategy:
Binary search to find the **best window start index**.

### ✅ Solution:
```python
def findClosestElements(arr, k, x):
    left, right = 0, len(arr) - k

    while left < right:
        mid = (left + right) // 2
        if x - arr[mid] > arr[mid + k] - x:
            left = mid + 1
        else:
            right = mid

    return arr[left:left + k]
```

### 🧠 Insight:
- Sliding window logic via binary search.
- Compare differences to determine direction.

---

## 🔹 5. **[LeetCode #1011 - Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)**
### 📌 Problem:
Determine the minimum capacity of a ship that will allow all packages to be shipped in exactly `days` days.

### ✅ Solution:
```python
def shipWithinDays(weights, days):
    def can_ship(capacity):
        day = 1
        total = 0
        for weight in weights:
            if total + weight > capacity:
                day += 1
                total = weight
                if day > days:
                    return False
            else:
                total += weight
        return True

    low, high = max(weights), sum(weights)
    while low < high:
        mid = (low + high) // 2
        if can_ship(mid):
            high = mid
        else:
            low = mid + 1
    return low
```

### 🧠 Insight:
- Binary search + greedy validation.
- Classic optimization pattern.

---

## 🔹 6. **[LeetCode #162 - Find Peak Element](https://leetcode.com/problems/find-peak-element/)**
### 📌 Problem:
A peak element is strictly greater than its neighbors. Return any peak’s index.

### ✅ Solution:
```python
def findPeakElement(nums):
    left, right = 0, len(nums) - 1
    while left < right:
        mid = (left + right) // 2
        if nums[mid] > nums[mid + 1]:
            right = mid
        else:
            left = mid + 1
    return left
```

### 🧠 Insight:
- Binary search applied to **unsorted arrays**.
- Based on trend detection (`mid vs mid+1`).

---

## 🔹 7. **[LeetCode #875 - Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)**
### 📌 Problem:
Koko can eat `h` hours worth of bananas. Find the minimum eating speed.

### ✅ Solution:
```python
def minEatingSpeed(piles, h):
    def time_required(speed):
        return sum((p + speed - 1) // speed for p in piles)

    low, high = 1, max(piles)
    answer = high

    while low <= high:
        mid = (low + high) // 2
        if time_required(mid) <= h:
            answer = mid
            high = mid - 1
        else:
            low = mid + 1

    return answer
```

### 🧠 Insight:
- Binary search on speed.
- Time calculation with ceiling division.

---

## 🔹 8. **[LeetCode #1283 - Find the Minimum Integer Such That Divided by All Array Elements Is at Most Threshold](https://leetcode.com/problems/find-the-minimum-number-of-frogs-croaking/)**
### 📌 Problem:
Find the smallest integer `x` such that the sum of `ceil(val/x)` for all elements ≤ threshold.

### ✅ Solution:
```python
def smallestDivisor(nums, threshold):
    def compute_sum(divisor):
        return sum((num + divisor - 1) // divisor for num in nums)

    low, high = 1, max(nums)
    ans = high

    while low <= high:
        mid = (low + high) // 2
        total = compute_sum(mid)
        if total <= threshold:
            ans = mid
            high = mid - 1
        else:
            low = mid + 1

    return ans
```

### 🧠 Insight:
- Same as Koko bananas, but generalized.

---

## 🔹 9. **[LeetCode #1891 - Cutting Ribbons](https://leetcode.com/problems/cutting-ribbons/)**
### 📌 Problem:
You are given an array of ribbons and want to cut them into `k` equal-length pieces.

### ✅ Solution:
```python
def maxLength(ribbons, k):
    def can_cut(length):
        return sum(r // length for r in ribbons) >= k

    low, high = 1, max(ribbons)
    ans = 0

    while low <= high:
        mid = (low + high) // 2
        if can_cut(mid):
            ans = mid
            low = mid + 1
        else:
            high = mid - 1

    return ans
```

### 🧠 Insight:
- Classic binary search on answer space.

---

## 🔹 10. **[LeetCode #1539 - Kth Missing Positive Number](https://leetcode.com/problems/kth-missing-positive-number/)**
### 📌 Problem:
Given a sorted array of unique integers, return the k-th missing positive number.

### ✅ Solution:
```python
def findKthPositive(nums, k):
    left, right = 0, len(nums) - 1

    while left <= right:
        mid = (left + right) // 2
        missing = nums[mid] - mid - 1
        if missing < k:
            left = mid + 1
        else:
            right = mid - 1

    return left + k
```

### 🧠 Insight:
- Use binary search to find first position where missing ≥ k.
- Linear formula used to calculate missing numbers.

---

# 🧾 Summary Table

| Problem | Type | Key Idea |
|--------|------|----------|
| [410](https://leetcode.com/problems/split-array-largest-sum/) | Optimization | Binary search on answer |
| [786](https://leetcode.com/problems/k-th-smallest-in-lexicographical-order/) | Digit-wise search | Prefix-based simulation |
| [2106G2](https://codeforces.com/contest/2106/problem/G2) | Interactive | Query-based decision |
| [658](https://leetcode.com/problems/find-k-closest-elements/) | Window selection | Compare distance |
| [1011](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) | Greedy + BS | Feasibility function |
| [162](https://leetcode.com/problems/find-peak-element/) | Unsorted array | Trend-based comparison |
| [875](https://leetcode.com/problems/koko-eating-bananas/) | Speed optimization | Ceiling division cost |
| [1283](https://leetcode.com/problems/find-the-minimum-integer-such-that-divided-by-all-array-elements-is-at-most-threshold/) | Generalized | Similar to banana problem |
| [1891](https://leetcode.com/problems/cutting-ribbons/) | Resource allocation | Count cuts per length |
| [1539](https://leetcode.com/problems/kth-missing-positive-number/) | Missing numbers | Math-based binary search |

---

Here’s a **comprehensive Binary Search Templates & Cheat Sheet** based on the detailed content from your uploaded PDF: *Variations of Binary Search in LeetCode Problems.pdf*.

---

# 🧠 Binary Search Templates & Cheat Sheet

## ⚠️ Common Pitfalls to Avoid
- ❌ Integer overflow when computing `mid = (low + high) // 2` → Use `mid = low + (high - low) // 2`
- ❌ Off-by-one errors in loop conditions and update logic
- ❌ Mishandling edge cases like empty arrays, single-element arrays, or duplicate elements
- ❌ Forgetting that binary search can be applied even if the array is not fully sorted (`Find Peak Element`)
- ❌ Incorrectly defining feasibility functions for optimization problems

---

# 🔍 1. Basic Binary Search Template (Sorted Array)

### Problem:
Search for a target in a **sorted array**

```python
def binary_search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

---

# 🔁 2. Binary Search in Rotated Sorted Array

### Problem:
[LeetCode #33 - Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)

```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid

        # Handle duplicates
        if nums[left] == nums[mid] == nums[right]:
            left += 1
            right -= 1
        elif nums[left] <= nums[mid]:  # Left side is sorted
            if nums[left] <= target < nums[mid]:
                right = mid - 1
            else:
                left = mid + 1
        else:  # Right side is sorted
            if nums[mid] < target <= nums[right]:
                left = mid + 1
            else:
                right = mid - 1
    return -1
```

---

# 🎯 3. Find Peak Element (Unsorted Array)

### Problem:
[LeetCode #162 - Find Peak Element](https://leetcode.com/problems/find-peak-element/)

```python
def findPeakElement(nums):
    left, right = 0, len(nums) - 1
    while left < right:
        mid = left + (right - left) // 2
        if nums[mid] > nums[mid + 1]:
            right = mid  # peak is on the left
        else:
            left = mid + 1  # peak is on the right
    return left
```

---

# 🔢 4. First and Last Occurrence (LowerBound & UpperBound)

### Problem:
[LeetCode #34 - Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```python
from bisect import bisect_left, bisect_right

def searchRange(nums, target):
    left = bisect_left(nums, target)
    if left >= len(nums) or nums[left] != target:
        return [-1, -1]
    right = bisect_right(nums, target) - 1
    return [left, right]
```

Or manually:

```python
def lower_bound(nums, target):
    left, right = 0, len(nums)
    while left < right:
        mid = left + (right - left) // 2
        if nums[mid] >= target:
            right = mid
        else:
            left = mid + 1
    return left

def upper_bound(nums, target):
    left, right = 0, len(nums)
    while left < right:
        mid = left + (right - left) // 2
        if nums[mid] > target:
            right = mid
        else:
            left = mid + 1
    return left
```

---

# ⚖️ 5. Optimization Using Binary Search (Minimize Max Value)

### Problem:
[LeetCode #410 - Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)

```python
def splitArray(nums, k):
    def canSplit(max_sum):
        count = 1
        curr_sum = 0
        for n in nums:
            if curr_sum + n > max_sum:
                count += 1
                curr_sum = n
                if count > k:
                    return False
            else:
                curr_sum += n
        return True

    low, high = max(nums), sum(nums)
    ans = high
    while low <= high:
        mid = low + (high - low) // 2
        if canSplit(mid):
            ans = mid
            high = mid - 1
        else:
            low = mid + 1
    return ans
```

---

# ✔️ 6. Feasibility Testing with Binary Search

### Problem:
[LeetCode #875 - Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

```python
def minEatingSpeed(piles, h):
    def time_required(speed):
        return sum((p + speed - 1) // speed for p in piles)

    low, high = 1, max(piles)
    answer = high
    while low <= high:
        mid = (low + high) // 2
        if time_required(mid) <= h:
            answer = mid
            high = mid - 1
        else:
            low = mid + 1
    return answer
```

---

# 🪜 7. Interactive Binary Search

### Problem:
[Codeforces #2106G2 - Baudelaire (Hard Version)](https://codeforces.com/contest/2106/problem/G2)

```cpp
int query(int l, int r) {
    cout << "? " << l << " " << r << endl;
    int res; cin >> res;
    return res;
}

int find_hidden_number(int n) {
    int left = 1, right = n;
    while (left < right) {
        int mid = (left + right) / 2;
        int res = query(left, mid);
        if (res % 2 == 0)
            right = mid;
        else
            left = mid + 1;
    }
    return left;
}
```

---

# 📈 8. Real Number Precision Binary Search

### Problem:
[LeetCode #786 - K-th Smallest in Lexicographical Order](https://leetcode.com/problems/k-th-smallest-in-lexicographical-order/)

```python
def findKthNumber(n, k):
    def count_steps(a, b):
        steps = 0
        while a <= n:
            steps += min(n + 1, b) - a
            a *= 10
            b *= 10
        return steps

    curr = 1
    k -= 1
    while k > 0:
        gap = count_steps(curr, curr + 1)
        if gap <= k:
            k -= gap
            curr += 1
        else:
            k -= 1
            curr *= 10
    return curr
```

---

# 🧮 9. Bitwise Binary Search

### Pattern:
Useful in interactive bit guessing problems, or when searching over binary representations.

```python
def guess_bitmask():
    result = 0
    for i in range(31):  # assuming 32-bit integer
        bit = 1 << (30 - i)
        # Query if result | bit is possible
        if is_possible(result | bit):
            result |= bit
    return result
```

Used in problems like:
- Codeforces: [Bitset Queries](https://codeforces.com/problemset/problem/1606/E)

---

# 📦 10. Binary Search + Greedy Strategy

### Problem:
[LeetCode #1011 - Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)

```python
def shipWithinDays(weights, days):
    def can_ship(capacity):
        day = 1
        total = 0
        for w in weights:
            if total + w > capacity:
                day += 1
                total = w
                if day > days:
                    return False
            else:
                total += w
        return True

    low, high = max(weights), sum(weights)
    while low < high:
        mid = (low + high) // 2
        if can_ship(mid):
            high = mid
        else:
            low = mid + 1
    return low
```

---

## ✅ Summary Table of Binary Search Variations

| Use Case | Example Problem | Template Type |
|---------|------------------|---------------|
| Basic Search | Standard Binary Search | Basic Template |
| Rotated Array | LeetCode 33 | Compare sides |
| Find Peak | LeetCode 162 | Trend-based comparison |
| First/Last Occurrence | LeetCode 34 | LowerBound / UpperBound |
| Minimize Max | LeetCode 410 | Feasibility function |
| Koko Bananas | LeetCode 875 | Threshold validation |
| Interactive BS | CF 2106G2 | Query feedback |
| Real Number BS | LeetCode 786 | Digit-wise traversal |
| Bitwise BS | CF 1606E | Fix bits iteratively |
| Greedy + BS | LeetCode 1011 | Validate greedy condition |

---

## 🛠️ Tips for Writing Robust Binary Search

- Always calculate midpoint using: `mid = low + (high - low) // 2`
- Use `while left < right` for peak finding
- Use `while left <= right` for value equality checks
- For real number precision, use fixed iterations: `for _ in range(100)`
- Use helper functions for complex feasibility checks
- Prefer built-in libraries like `bisect` in Python
- Understand monotonic properties before applying binary search

---

# 📄 Bonus: Printable Binary Search Cheat Sheet (PDF Format)

If you'd like me to generate a **ready-to-print PDF version** of this cheat sheet (great for interviews or revision), just say so and I'll format it for you!

---

Let me know if you want:
- A downloadable PDF version
- More templates for specific problem types (e.g., matrix binary search)
- Practice problems grouped by variation type
- Interview-style flashcards for quick revision