---
tags:
  - dynamic_programming
  - array
  - depth_first_search
  - breadth_first_search
  - medium
---
# 322. Coin Change
Link: [here](https://leetcode.com/problems/coin-change/description/)
## Problem
You are given an integer array `coins` representing coins of different denominations and an integer amount representing a total `amount` of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.
## Intuition 
- You might want to try and solve this problem greedily, however you can't e.g. `[3,2]` for `4` wouldn't work if we just kept adding the largest value
- This means we need to do DP since we want to go through permutations, and also will be dealing with subproblems, (multiple ways to end up needing to make change for the same amount)
- We could do DFS, only searching if the current value is less than the current minimum sum found, however that's not ideal either, since it still doesn't stop us from exploring subproblems 
- This means we ideally want a bottom-up solution, where we can start with small subproblems and build our way up to the answer
- When we see a subproblem, we either can just return it or update it in the case we found a better value
## Approach
- Make our DP array that is length `amount+1` so we can get all values from `[0, amount]` inclusive, and set all values to `inf` since we are looking to see if we can find smaller ways to make change
- Set a seed value for the array, we set `dp[0] = 0` since you need 0 coins to make change for $0.
- Go through the array so we can try to calculate each amount value, starting from 1
- For each amount, we will try and use each coin denomination, if we get a value that is positive or 0, we can see if we either found a better value for our current amount, or should just use the already calculated one
- Finally return `dp[amount+1]` if it's not `inf`
## Edge Cases/Pitfalls
- Knowing the problem can't be solved greedily is important
- Knowing that DFS won't evaluate the subproblems and you need bottom-up is important
## Solution
```python
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [float('inf')] * (amount+1)
        dp[0] = 0

        for i in range(1, amount+1):
            for coin in coins:
                # If we already have subproblem solved
                if i - coin >= 0:
                    dp[i] = min(dp[i], dp[i-coin]+1)

        return dp[amount] if dp[amount] != float('inf') else -1
```
Note: you can also just use `amount+1` instead of `float('inf')` since they would function the same way
## Time Complexity
$O(k)$ where `k` is the amount
## Space Complexity 
$O(k)$
## Takeaways
- There are times where you're better off doing bottom-up programming
- When you see a greedy problem, try to find counterexamples
- Once you know it's DP, think of the recurrence relation 
