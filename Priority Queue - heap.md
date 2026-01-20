
[[Intuition for slot fitting]]

```python

from collections import Counter, deque
import heapq
from typing import List


class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        # Frequency of tasks
        freq = Counter(tasks)

        # Max-heap of available tasks by remaining count
        # heapq is a min-heap, so store negative counts
        max_heap = [-c for c in freq.values()]
        heapq.heapify(max_heap)

        # Queue for tasks in cooldown: (ready_time, remaining_count_neg)
        cooldown = deque()

        time = 0
        while max_heap or cooldown:
            # Release tasks whose cooldown finished
            while cooldown and cooldown[0][0] <= time:
                _, cnt_neg = cooldown.popleft()
                heapq.heappush(max_heap, cnt_neg)

            if max_heap:
                cnt_neg = heapq.heappop(max_heap)
                cnt_neg += 1  # used one instance (e.g., -3 -> -2). Toward zero.

                # If still remaining, put into cooldown
                if cnt_neg < 0:
                    cooldown.append((time + n + 1, cnt_neg))
            # else: idle

            time += 1

        return time
```



Common Leetcode Problems on Priority Queue

1. [[Kth Largest Element in an Array (Leetcode 215) ]]— Medium

Problem: Find the kth largest element in an unsorted array.

Approach: Min-Heap of size k. Keep adding elements, and pop the smallest when size > k.

Time: O(n log k)

Leetcode: Link



---

2. Merge K Sorted Lists (Leetcode 23) — Hard

Problem: Merge k sorted linked lists and return it as one sorted list.

Approach: Use Min-Heap to always extract the smallest head node.

Time: O(N log k), where N is total nodes.

Leetcode: Link



---

3. Top K Frequent Elements (Leetcode 347) — Medium

Problem: Find the k most frequent elements.

Approach: HashMap (frequency count) + Min-Heap of size k.

Time: O(n log k)

Leetcode: Link



---

4. Find Median from Data Stream (Leetcode 295) — Hard

Problem: Continuously find the median as numbers are added.

Approach: Two Heaps — Max-Heap (lower half), Min-Heap (upper half) to maintain balance.

Time: O(log n) for insertion, O(1) for median.

Leetcode: Link



---

5. Sliding Window Maximum (Leetcode 239) — Hard

Problem: Find the max in each sliding window of size k.

Approach: Max-Heap or Monotonic Deque (Heap needs lazy deletion).

Time: O(n log k) using heap.

Leetcode: Link



---

Tips for Priority Queue Problems:

1. Identify when you need real-time min/max selection.


2. Fixed-size k operations -> min-heap/max-heap of size k.


3. Dynamic streaming data -> two heaps (median problems).


4. Merging sorted streams -> min-heap for smallest next element.




---
