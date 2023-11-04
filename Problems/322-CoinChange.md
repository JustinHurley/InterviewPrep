---
tags: [dynamic_programming, array, depth_first_search, breadth_first_search]
---

### 322. Coin Change

Link: [here](https://leetcode.com/problems/coin-change/description/)

#### Problem
You are given an integer array `coins` representing coins of different denominations and an integer amount representing a total `amount` of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

#### Approach
Since we are looking at the best solution from a set of many solutions, and there is no greedy choice approach to solve this problem, we will need to follow a DP approach. This problem is asking us the best solution for making change with the given coin types and the given amount. 
Instead of permuting this problem starting at the amount, instead we will start with a base case and build our way to higher starting amounts until we solve for the desired amount. So we will start with a DP array, that holds `n` values, where the index of each value is the number of coins that can be used to make the amount. 
To start, the index of 0 will have a value of 0, since there are 0 ways to denominate 0 coins with the given amounts. Every other cell in the array will have a value of `amount + 1`, which we are using in lieu of `Math.MAX_INTEGER` or some other sufficiently large amount that a `min` comparison will always return a lower value if present. 
So now that we have established our array, we will need to iterate through it from 1 to `amount+1`. We go to `amount+1` just because we want to capture the value at `dp[amount]`, and the for loop is exclusive of the last value.
In each iteration, we will iterate through every coin type and do a comparison:
```
if i - coin >= 0:
    dp[i] = min(dp[i], dp[i-coin]+1)
```
The above line of code is asking what takes fewer coins to make: the current value (which may be uncalculated and thus `amount+1`) or the number of coins it takes to make up the `amount - coin` where `coin` is the value of the given coin. We can do this check because we know that the earlier values in the `dp` array have already been filled, since we started with the `dp[0]` seed value. The if statement check is also important because we don't want to provide coin counts for values that are impossible to make with the given coins, e.g. if you want to make 7 cents only using nickels, that is impossible. So we go through and calculate the lowest coin amount possible to make the desired amount at each amount up to `amount` and then we return the value of `dp[amount]`, the only catch is make sure it's not `amount + 1` because that means we can't calculate that amount with the given coin denominations.
#### Solution
```python 
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        # This is where we build the array, initally we set each val to above the max for min comparisons
        dp = [amount + 1] * (amount + 1)
        # We know the base case, so set it
        dp[0] = 0

        # Now we will iterate through each possible amount to build our soln
        for amt in range(1, amount+1):
            # For each amount we need to consider each coin
            for coin in coins:
                # Don't want negative cases
                if amt - coin >= 0:
                    # Here we compare the current amount relative to the amount that is already determined with fewer coins
                    # So if we can get from our value to an already calculated value, we should do that
                    dp[amt] = min(dp[amt], 1 + dp[amt-coin])

        # Now we return the amount iff the amount was updated, otherwise it's impossible to generate
        return dp[amount] if dp[amount] < amount+1 else -1
```

#### Time Complexity
`O(amount * len(coins))`

#### Space Complexity
`O(amount)`
