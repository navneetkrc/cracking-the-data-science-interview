

# 121. Best Time to Buy and Sell Stock

## Problem Description

> You are given an array `prices` where `prices[i]` is the price of a given stock on the i<sup>th</sup> day.
>
> You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.
>
> Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

---

## Examples

**Example 1:**

*   **Input:** `prices = [7,1,5,3,6,4]`
*   **Output:** `5`
*   **Explanation:** Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
    Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

**Example 2:**

*   **Input:** `prices = [7,6,4,3,1]`
*   **Output:** `0`
*   **Explanation:** In this case, no transactions are done and the max profit = 0.

---

## Constraints

*   `1 <= prices.length <= 10^5`
*   `0 <= prices[i] <= 10^4`

---

## Python Solution

```python
from typing import List

class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # If there are less than 2 days, no transaction can be made.
        if len(prices) <= 1:
            return 0
            
        # 'buy_day_index' will store the index of the day with the minimum price found so far
        # to consider as a potential buy day. Initialize to the first day.
        buy_day_index = 0 
        
        # 'max_profit_so_far' will store the maximum profit achievable.
        max_profit_so_far = 0
        
        # Iterate through the prices, considering each day as a potential 'sell_day'.
        # Start from the second day (index 1) as we need a prior day to buy.
        for sell_day_index in range(1, len(prices)):
            current_sell_price = prices[sell_day_index]
            current_buy_price = prices[buy_day_index]
            
            # Check if selling on the current day yields a profit
            if current_sell_price > current_buy_price:
                # Calculate profit if we sell on 'sell_day_index' and bought on 'buy_day_index'
                profit = current_sell_price - current_buy_price
                # Update max_profit_so_far if this profit is higher
                max_profit_so_far = max(max_profit_so_far, profit)
            else:
                # If the current price (prices[sell_day_index]) is lower than our 
                # current best buy price (prices[buy_day_index]),
                # it means we've found a new, better day to potentially buy.
                # So, update our 'buy_day_index' to the current day's index.
                buy_day_index = sell_day_index
                
        return max_profit_so_far

```

### Explanation of the Logic:

The code iterates through the `prices` array using a `sell_day_index` (representing the day we might sell). It maintains a `buy_day_index` that points to the day with the lowest price encountered *so far* which could serve as a buying day.

1.  **Initialization:**
    *   `buy_day_index` is set to `0` (the first day).
    *   `max_profit_so_far` is set to `0`.

2.  **Iteration:**
    *   For each `sell_day_index` (starting from the second day):
        *   It compares `prices[sell_day_index]` (potential sell price) with `prices[buy_day_index]` (best buy price found so far).
        *   **If `prices[sell_day_index] > prices[buy_day_index]`**: This means selling on `sell_day_index` would result in a profit. The code calculates this `profit` and updates `max_profit_so_far` if this new profit is greater.
        *   **Else (if `prices[sell_day_index] <= prices[buy_day_index]`)**: This means the price on `sell_day_index` is lower than or equal to our current best buy price. Therefore, `sell_day_index` becomes a better candidate for a `buy_day_index` for *future* potential sales. So, `buy_day_index` is updated to `sell_day_index`.

This approach efficiently finds the maximum profit in a single pass (O(N) time complexity) using constant extra space (O(1) space complexity). It's a variation of the common "track minimum price and max profit" strategy.
