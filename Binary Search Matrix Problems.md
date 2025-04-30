# 🧠 Matrix Binary Search and Related Problems  
Binary search extends beyond one-dimensional arrays to **two-dimensional matrices**, particularly when the matrix has sorted properties (e.g., row-wise or column-wise sorted). These variations introduce a new layer of complexity, requiring careful decision-making to reduce the search space effectively. Below is a comprehensive guide to **matrix binary search**, including key types, problem patterns, and curated practice problems from platforms like LeetCode.

---

## 🔍 Types of Matrix Binary Search

### 1. **Sorted Matrix Search**
**Property:** Each row and each column is sorted in non-decreasing order.

#### Strategy:
- Start from top-right or bottom-left corner.
- Move either left/down based on comparison with the target.
- Time Complexity: O(m + n)

#### Example Problem:
**LeetCode #74. Search a 2D Matrix I**  
https://leetcode.com/problems/search-a-2d-matrix/

```python
def searchMatrix(matrix, target):
    if not matrix or not matrix[0]:
        return False
    m, n = len(matrix), len(matrix[0])
    i, j = 0, n - 1  # Start from top-right

    while i < m and j >= 0:
        if matrix[i][j] == target:
            return True
        elif matrix[i][j] > target:
            j -= 1  # Move left
        else:
            i += 1  # Move down
    return False
```

---

### 2. **Row-Wise Sorted Matrix Search**
**Property:** Each row is sorted, and the first element of each row is greater than the last element of the previous row.

#### Strategy:
- Treat the matrix as a flattened 1D array.
- Use binary search with virtual indices.

#### Example Problem:
**LeetCode #74. Search a 2D Matrix I (Alternative Version)**  
Also applies here using this method.

```python
def searchMatrix(matrix, target):
    if not matrix or not matrix[0]:
        return False
    m, n = len(matrix), len(matrix[0])
    left, right = 0, m * n - 1

    while left <= right:
        mid = left + (right - left) // 2
        row, col = divmod(mid, n)
        if matrix[row][col] == target:
            return True
        elif matrix[row][col] < target:
            left = mid + 1
        else:
            right = mid - 1
    return False
```

---

### 3. **K-th Smallest Element in a Row-wise Sorted Matrix**
**Property:** Each row is sorted, and columns may also be sorted.

#### Strategy:
- Apply binary search over the value range (not index).
- Count elements less than or equal to mid using a helper function.

#### Example Problem:
**LeetCode #378. Kth Smallest Element in a Sorted Matrix**  
https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/

```python
def kthSmallest(matrix, k):
    def count_less_equal(mid):
        count = 0
        n = len(matrix)
        row, col = n - 1, 0
        while row >= 0 and col < n:
            if matrix[row][col] <= mid:
                count += row + 1
                col += 1
            else:
                row -= 1
        return count

    n = len(matrix)
    low, high = matrix[0][0], matrix[n - 1][n - 1]

    while low < high:
        mid = (low + high) // 2
        if count_less_equal(mid) < k:
            low = mid + 1
        else:
            high = mid
    return low
```

---

### 4. **Peak Element in a 2D Grid**
**Property:** A peak is an element strictly greater than all its neighbors (up, down, left, right).

#### Strategy:
- Use binary search on rows/columns.
- For each selected row, find the maximum value and compare with neighbors.

#### Example Problem:
**LeetCode #1901. Find a Peak Element II**  
https://leetcode.com/problems/find-a-peak-element-ii/

```python
def findPeakGrid(matrix):
    m, n = len(matrix), len(matrix[0])

    def get(r, c):
        return matrix[r][c] if 0 <= r < m and 0 <= c < n else -1

    def find_peak(r1, r2):
        if r1 > r2:
            return (-1, -1)
        r = (r1 + r2) // 2
        max_idx = 0
        for c in range(n):
            if matrix[r][c] > matrix[r][max_idx]:
                max_idx = c
        val = matrix[r][max_idx]
        if val > get(r - 1, max_idx) and val > get(r + 1, max_idx):
            return (r, max_idx)
        elif val < get(r - 1, max_idx):
            return find_peak(r1, r - 1)
        else:
            return find_peak(r + 1, r2)

    return find_peak(0, m - 1)
```

---

### 5. **Minimum in Rotated Sorted Matrix**
**Property:** A matrix where each row is rotated sorted, and each row's first element is greater than the previous row's last element.

#### Strategy:
- Use binary search similar to "Find Minimum in Rotated Sorted Array".
- Compare middle with edges to determine which half to search next.

#### Example Problem:
Custom variation inspired by standard binary search patterns.

```python
def findMin(matrix):
    m, n = len(matrix), len(matrix[0])
    left, right = 0, m * n - 1

    while left < right:
        mid = (left + right) // 2
        row, col = divmod(mid, n)
        if matrix[row][col] > matrix[-1][-1]:
            left = mid + 1
        else:
            right = mid
    row, col = divmod(left, n)
    return matrix[row][col]
```

---

## 📌 Practice Problems Grouped by Type

| Type | Problem | Link |
|------|--------|------|
| **Sorted Matrix Search** | [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/) | [LeetCode](https://leetcode.com/problems/search-a-2d-matrix/) |
| **Row-Wise Sorted Matrix Search** | [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/) | [LeetCode](https://leetcode.com/problems/search-a-2d-matrix/) |
| **K-th Smallest in Sorted Matrix** | [Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) | [LeetCode](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) |
| **Peak Element in 2D Grid** | [Find a Peak Element II](https://leetcode.com/problems/find-a-peak-element-ii/) | [LeetCode](https://leetcode.com/problems/find-a-peak-element-ii/) |
| **Minimum in Rotated Matrix** | Custom variation | N/A |
| **Binary Search on Value Range** | [Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements/) | [LeetCode](https://leetcode.com/problems/find-k-closest-elements/) |
| **Binary Search on Answer Space in Matrix** | [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) | [LeetCode](https://leetcode.com/problems/median-of-two-sorted-arrays/) |

---

## ✅ Tips for Solving Matrix Binary Search Problems

- Understand the **sorted structure**: row-wise, column-wise, or both.
- Decide whether to apply binary search on **indices** or **value ranges**.
- Use **virtual flattening** when the matrix simulates a 1D sorted array.
- Adjust your search direction based on comparisons (top-right or bottom-left starting points help).
- Be cautious with **indexing**, especially `row = mid // n`, `col = mid % n`.
- Handle edge cases: empty matrix, single-row, single-column.

---

## 🛠️ Bonus: Printable Matrix Binary Search Cheat Sheet

If you'd like, I can generate a **printable PDF cheat sheet** summarizing these patterns along with code templates and visual diagrams of matrix traversal strategies!

---

Let me know if you’d like:
- A downloadable PDF version of this summary
- More advanced problems involving matrix binary search
- Flashcards for interview prep
- Code walkthroughs of specific matrix binary search LeetCode problems