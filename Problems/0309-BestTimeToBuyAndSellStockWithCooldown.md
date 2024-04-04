---
tags:
  - medium
  - dynamic_programming
  - memoization
---
# 309. Best Time to Buy and Sell Stock with Cooldown
Link: [here](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)
## Problem
You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.
Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:
- After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).
## Main Idea
- Use a `2*n` memo table to store results (or just a memo table that stores the data with tuples)
- Use a boolean to store if we are buying that round or selling that round
- Make the recurrence relations for buying, selling, and the cooldown decisions 
## Approach
- Make a memo dictionary to cache DFS values
- Now make the DFS method that takes in the index and the buying boolean
	- If the index is past the size of the input array, return 0 as there is no money to make here
	- If the value is present in the memo table from the input parameters, use that
	- If none of those options are true, we need to compute the answer by checking the recurrence relations
	- If we want to buy: `dfs(i+1, False) - prices[i]`
	- If we want to sell: `dfs(i+2, True) + prices[i]`
	- If we want to do nothing: `dfs(i+1, same_value)`
	- We take the max of the recurrence relations dependent on if we are buying or selling, and then save that value in the memo table.
- Finally, make a call with `dfs(0, True)` since we want to start at the 0th index and also we cannot sell so set the buying boolean to be true
## Edge Cases/Gotchas 
- The hard part of the problem is knowing to use a boolean to represent buying or selling
## Solution
```python 
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp = {} 
        def dfs(i, buying):
            # Base case for OOB
            if i >= len(prices):
                return 0
            # Check memo table
            if (i, buying) in dp:
                return dp[(i, buying)]
            # If buying, it means we want to buy this round
            if buying:
                # We can buy now or buy later
                buy = dfs(i+1, not buying) - prices[i]
                cooldown = dfs(i+1, buying)
                dp[(i, buying)] = max(buy, cooldown)
            else:
                # Sell requires cooldown 
                sell = dfs(i+2, not buying) + prices[i]
                cooldown = dfs(i+1, buying)
                dp[(i, buying)] = max(sell, cooldown)
            return dp[(i, buying)]
        return dfs(0, True)
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$ for the memo table 
