
```python
def productExceptSelf(nums):
    # Initialize the result array where we'll store the final answers
    n = len(nums)
    result = [1] * n
    
    # Compute the prefix product (product of all elements to the left of each index)
    prefix_product = 1
    for i in range(n):
        result[i] = prefix_product
        prefix_product *= nums[i]
    
    # Compute the suffix product (product of all elements to the right of each index)
    suffix_product = 1
    for i in range(n - 1, -1, -1):
        result[i] *= suffix_product
        suffix_product *= nums[i]
    
    return result

```
