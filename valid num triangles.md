```python
def triangleNumber(nums):
    nums.sort()
    n = len(nums)
    count = 0

    for k in range(n - 1, 1, -1):  # Fix the third side
        i, j = 0, k - 1
        while i < j:
            if nums[i] + nums[j] > nums[k]:
                count += j - i  # all elements between i and j are valid
                j -= 1
            else:
                i += 1
    return count

```