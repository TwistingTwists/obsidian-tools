

## Problem Statement

You are given an array `prices` where `prices[i]` represents the price of a given stock on the **i-th** day. You are permitted to complete **at most one** transaction (i.e., buy one and sell one share of the stock). Your goal is to design an algorithm to find the maximum profit you can achieve from this transaction. If no profit can be achieved, return `0`.


| **Example**                                                            | **Input**                            | **Explanation**                                                                                 | **Expected Output** |
| ---------------------------------------------------------------------- | ------------------------------------ | ----------------------------------------------------------------------------------------------- | ------------------- |
| **1. Simple Increasing Prices**                                        | `[1, 2, 3, 4, 5]`                    | Buy on day 0 (price = 1) and sell on day 4 (price = 5).                                         | `4`                 |
| **2. Prices Fluctuating**                                              | `[7, 1, 5, 3, 6, 4]`                 | Buy on day 1 (price = 1) and sell on day 4 (price = 6).                                         | `5`                 |
| **3. Prices Decreasing**                                               | `[7, 6, 4, 3, 1]`                    | No profitable transactions possible.                                                            | `0`                 |
| **4. Multiple Peaks and Troughs**                                      | `[3, 2, 6, 5, 0, 3]`                 | Buy on day 1 (price = 2) and sell on day 2 (price = 6).                                         | `4`                 |
| **7. Same Prices Every Day**                                           | `[5, 5, 5, 5]`                       | No profit can be made as prices do not change.                                                  | `0`                 |
| **8. Large Prices with Early Peak**                                    | `[100, 180, 260, 310, 40, 535, 695]` | Buy on day 0 (price = 100) and sell on day 6 (price = 695).                                     | `595`               |
| <br><span class="reading" > **9. Buy Later for Maximum Profit**</span> | `[3, 3, 5, 0, 0, 3, 1, 4]`           | Buy on day 3 or 4 (price = 0) and sell on day 5 or 7 (price = 3 or 4).                          | `4`                 |
| **10. Early High Price with Later Low and High**                       | `[10, 1, 2, 3, 4]`                   | Buy on day 1 (price = 1) and sell on day 4 (price = 4).                                         | `3`                 |
| **11. Peak at the End**                                                | `[1, 2, 3, 4, 5, 10]`                | Buy on day 0 (price = 1) and sell on day 5 (price = 10).                                        | `9`                 |
| **12. Multiple Equal Maximums**                                        | `[2, 4, 1, 7, 5, 7]`                 | Buy on day 2 (price = 1) and sell on day 3 or day 5 (price = 7).                                | `6`                 |
| **13. Immediate Profit Opportunity**                                   | `[1, 4, 2, 5, 3, 6]`                 | Buy on day 0 (price = 1) and sell on day 5 (price = 6).                                         | `5`                 |
| **14. No Transaction Needed**                                          | `[]`                                 | No days available to perform transactions.                                                      | `0`                 |
| **15. Negative Prices (Edge Case)**                                    | `[-1, -2, -3, -4, -5]`               | Even with negative prices, the best profit is `0` as buying and selling would result in a loss. | `0`                 |

---

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



--- 

 **Variations** of the "Best Time to Buy Stocks" problem, including their descriptions, examples, expected outputs, and approaches.

| **#** | **Variation**                                                                        | **Description**                                                               | **Example Input**                                        | **Expected Output** | **Approach**                                                                                        |
| ----- | ------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------- | -------------------------------------------------------- | ------------------- | --------------------------------------------------------------------------------------------------- |
| 1     | **Single Transaction**                                                               | Buy once and sell once to maximize profit.                                    | Prices: `[7, 1, 5, 3, 6, 4]`                             | `5`                 | Track the minimum price and calculate maximum profit by iterating through prices.                   |
| 2     | **Multiple Transactions Allowed (Unlimited)**                                        | Execute as many buy-sell transactions as possible for maximum profit.         | Prices: `[7, 1, 5, 3, 6, 4]`                             | `7`                 | Sum all positive differences between consecutive increasing prices.                                 |
| 3     | <span class="reading"> **Limited Number of Transactions (`k` Transactions)** </span> | Perform at most `k` buy-sell transactions to maximize profit.                 | `k=2`, Prices: `[3, 2, 6, 5, 0, 3]`                      | `7`                 | Use dynamic programming to track profits with up to `k` transactions.                               |
| 4     | **With Transaction Fees**                                                            | Maximize profit accounting for a fixed transaction fee per trade.             | Prices: `[1, 3, 2, 8, 4, 9]`, Fee: `2`                   | `8`                 | Adjust profit calculations by subtracting fees during sell operations using DP.                     |
| 5     | **With Cooldown Period**                                                             | Introduce a cooldown period after each sell before the next buy.              | Prices: `[1, 2, 3, 0, 2]`                                | `3`                 | Utilize state variables to manage holding, not holding, and cooldown states.                        |
| 6     | **Short Selling Allowed**                                                            | Allow selling stocks not owned (short selling) to maximize profit.            | Prices: `[3, 2, 6, 5, 0, 3]`                             | Varies              | Incorporate strategies for both long and short positions, increasing algorithm complexity.          |
| 7     | **Fractional Shares Allowed**                                                        | Permit buying/selling fractional shares for flexibility in transactions.      | Prices: `[1.5, 3.0, 2.0, 4.5]`                           | `3.0`               | Optimize transactions considering fractional units, similar to single transaction logic.            |
| 8     | **Portfolio of Multiple Stocks**                                                     | Trade multiple different stocks to maximize total profit.                     | Stock1: `[1, 2, 3]`, Stock2: `[2, 1, 4]`                 | `4`                 | Handle each stock independently or use portfolio optimization techniques for interdependent assets. |


---
