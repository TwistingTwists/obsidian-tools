

## Problem Statement

You are given an array `prices` where `prices[i]` represents the price of a given stock on the **i-th** day. You are permitted to complete **at most one** transaction (i.e., buy one and sell one share of the stock). Your goal is to design an algorithm to find the maximum profit you can achieve from this transaction. If no profit can be achieved, return `0`.

**Example:**



`Input: prices = [7,1,5,3,6,4] Output: 5 Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.`


## Example Inputs and Expected Outputs

#### **1. Simple Increasing Prices**

- **Input:** `[1, 2, 3, 4, 5]`
- **Explanation:** Buy on day 0 (price = 1) and sell on day 4 (price = 5).
- **Expected Output:** `4`

#### **2. Prices Fluctuating**

- **Input:** `[7, 1, 5, 3, 6, 4]`
- **Explanation:** Buy on day 1 (price = 1) and sell on day 4 (price = 6).
- **Expected Output:** `5`

#### **3. Prices Decreasing**

- **Input:** `[7, 6, 4, 3, 1]`
- **Explanation:** No profitable transactions possible.
- **Expected Output:** `0`

#### **4. Multiple Peaks and Troughs**

- **Input:** `[3, 2, 6, 5, 0, 3]`
- **Explanation:** Buy on day 1 (price = 2) and sell on day 2 (price = 6).
- **Expected Output:** `4`

#### **7. Same Prices Every Day**

- **Input:** `[5, 5, 5, 5]`
- **Explanation:** No profit can be made as prices do not change.
- **Expected Output:** `0`

#### **8. Large Prices with Early Peak**

- **Input:** `[100, 180, 260, 310, 40, 535, 695]`
- **Explanation:** Buy on day 0 (price = 100) and sell on day 6 (price = 695).
- **Expected Output:** `595`

#### **9. Buy Later for Maximum Profit**

<span class="reading"  >  ** </span>

- **Input:** `[3, 3, 5, 0, 0, 3, 1, 4]`
- **Explanation:** Buy on day 3 or 4 (price = 0) and sell on day 5 or 7 (price = 3 or 4).
- **Expected Output:** `4`

#### **10. Early High Price with Later Low and High**

- **Input:** `[10, 1, 2, 3, 4]`
- **Explanation:** Buy on day 1 (price = 1) and sell on day 4 (price = 4).
- **Expected Output:** `3`

#### **11. Peak at the End**

- **Input:** `[1, 2, 3, 4, 5, 10]`
- **Explanation:** Buy on day 0 (price = 1) and sell on day 5 (price = 10).
- **Expected Output:** `9`

#### **12. Multiple Equal Maximums**

- **Input:** `[2, 4, 1, 7, 5, 7]`
- **Explanation:** Buy on day 2 (price = 1) and sell on day 3 or day 5 (price = 7).
- **Expected Output:** `6`

#### **13. Immediate Profit Opportunity**

- **Input:** `[1, 4, 2, 5, 3, 6]`
- **Explanation:** Buy on day 0 (price = 1) and sell on day 5 (price = 6).
- **Expected Output:** `5`

#### **14. No Transaction Needed**

- **Input:** `[]` (Empty array)
- **Explanation:** No days available to perform transactions.
- **Expected Output:** `0`

#### **15. Negative Prices (Edge Case)**

- **Input:** `[-1, -2, -3, -4, -5]`
- **Explanation:** Even with negative prices, the best profit is `0` as buying and selling would result in a loss.
- **Expected Output:** `0`
- 

## Solution Approaches

### 1. Brute Force Approach


```python
def maxProfit(prices):
    max_profit = 0
    n = len(prices)
    for i in range(n):
        for j in range(i + 1, n):
            profit = prices[j] - prices[i]
            if profit > max_profit:
                max_profit = profit
    return max_profit
```



### 2. Optimized One-Pass Approach (Kadane’s Algorithm Inspired)

**Idea:**  
Traverse the array while keeping track of the minimum price encountered so far and the maximum profit that can be achieved by selling on the current day. This approach ensures that we find the optimal buy and sell days in a single pass.

```python
def maxProfit(prices):
    """
    Calculates the maximum profit that can be made by buying low and selling high.

    Args:
        prices: A list of integers representing the prices of a stock over time.

    Returns:
        The maximum profit that can be made, or 0 if the prices list is empty.
    """
    if not prices:
        return 0

    min_price = prices[0]
    max_profit = 0

    for price in prices:
        # Update the minimum price if the current price is lower
        if price < min_price:
            min_price = price
        else:
            # Calculate profit if selling at current price
            profit = price - min_price
            # Update max profit if this profit is higher
            if profit > max_profit:
                max_profit = profit

    return max_profit
```

