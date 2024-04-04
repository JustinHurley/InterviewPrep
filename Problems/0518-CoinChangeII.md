---
tags: [medium, dynamic_programming, depth_first_search, memoization, bottom_up]
---
# 518. Coin Change II
Link: [here](https://leetcode.com/problems/coin-change-ii/description/)
## Problem
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the number of combinations that make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.
The answer is **guaranteed** to fit into a signed **32-bit** integer.
## Main Idea
- To avoid double-counting coin sets (`1,1,2` vs. `1,2,1`) limit change making to the highest current coin value used or higher, for example if you made change with a `5`, all coins to make change in child calls should be at least `5` or higher, which ensures the only sum to 6 we would see is `1,5` and not `5,1`.
- Recurrence relation is the number of ways we can make change with the current coin, along with the number of ways we can make change with only larger coins
## Approaches
This problem will have multiple approaches for the different ways of solving this problem.
#### Memoization
- Make a memo table and a DFS method to recur over the decision tree
- The method takes 2 inputs, the current amount of change left to make and the index that corresponds to the smallest currency denomination we are working with 
	- If the current amount is 0, then we found an answer `return 1`
	- If the index is larger than the number of coins or the current smallest coin being used is too large to make change `return 0`
	- If we already made change using that smallest denomination and targeting that amount, just return the value from the memo table
	- If we actually need to get the value, we will use the recurrence relation like so:
	```python
memo[(curr_amount, i)] = dfs(amount-coins[i], i) + dfs(amount, i+1)
	```
	- Above we see to get the current amount, we take how many ways we make change using our current coin, but also how many ways we can make change only using larger coins
#### Tabulation
- Make the DP array that has a row for each coin and a cell for each amount, plus an extra cell since we want to have a cell for 0 but also a cell where the index equals the amount 
- Set the value for all cells with 0 to 1, since when there the target amount is 0, we are done and have found a valid solution 
- Now we actually fill in the array, start at the largest coin amount, and go from the amount being 1 all the way to the target amount
- A given cell's value is equal to the ways you can make change with larger amounts + if you can make change with the current amount + a given coin denomination 
## Edge Cases/Gotchas 
- Need to avoid double counting change when the permutation is different but the count of each coin is not
## Solution (Memoization)
```python 
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        memo = {}
        coins.sort()
        def dfs(curr_amount, i): 
            # If curr amount is 0 we found a soln
            if curr_amount == 0:
                return 1
            # If OOB or can't use any coins
            if i >= len(coins) or curr_amount < coins[i]:
                return 0
            # Check memo table
            if (curr_amount, i) in memo:
                return memo[(curr_amount, i)]
            # Otherwise we need to actually find value
            # We can either add the curr coin or try to make change with higher amount only
            memo[(curr_amount, i)] = dfs(curr_amount-coins[i], i) + dfs(curr_amount, i+1)
            return memo[(curr_amount, i)]
        return dfs(amount, 0)
```
## Solution (Tabular)
```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        memo = {}
        dp = [[0]*(amount+1) for _ in coins]
        # Set seed values
        for row in dp:
            row[0] = 1
        # c_i coin index, a_i amount index
        for c_i in range(len(dp)-1, -1, -1):
            coin = coins[c_i]
            for a_i in range(1,len(dp[0])):
                # Add calced value from lower amt
                if a_i - coin >= 0:
                    dp[c_i][a_i] += dp[c_i][a_i-coin]
                # Add calced value from larger coin
                if c_i < len(dp)-1:
                    dp[c_i][a_i] += dp[c_i+1][a_i]
        return dp[0][amount] 
```
## Time Complexity
$O(m*n)$ where `n` is the number of coins and `m` is the amount, both the tabular and memoization solution use this time complexity.
## Space Complexity
$O(m*n)$ but we can technically do it in `O(m)` time with the tabular approach if we just use 2 arrays.
