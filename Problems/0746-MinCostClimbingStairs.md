---
tags:
  - dynamic_programming
  - array
  - easy
  - bottom_up
---
# 746. Min Cost Climbing Stairs
Link: [here](https://leetcode.com/problems/min-cost-climbing-stairs/)
## Problem
You are given an integer array `cost` where `cost[i]` is the cost of `ith` step on a staircase. Once you pay the cost, you can either climb one or two steps.
You can either start from the step with index `0`, or the step with index `1`.
## Main Idea
- Build the recurrence relation, which looks at the two previous steps, and takes the minimum cost one to get to the previous step
- Move front to back as you decrease the subproblem size
## Approach
- Build a DP array with an extra space to hold the answer cost
- Iterate through the DP array to fill it in
- Start at index 2, since you can start at either the 0 or 1 index for free (thus the cost is 0)
- `dp[i] = min(dp[i-2]+cost[i-2], dp[i-1]+cost[i-1])`
- Return the last element (cost of getting to the `n` index)
## Edge Cases/Gotchas 
- Set the cost of going to `dp[0]` and `dp[1]` to 0. This is because there is no cost to start from either of those items
- Set all values in the DP array to 0, this just allows for the min cost for the first 2 elements to be 0
## Solution
```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        n = len(cost)
        dp = [0]*(n+1)
        for i in range(2, n+1):
            dp[i] = min(dp[i-2]+cost[i-2], dp[i-1]+cost[i-1])
        return dp[n]
```
## Time Complexity 
$O(n)$ since the DP array is based on the size of the cost input array
## Space Complexity 
$O(n)$ but can be done in constant space using pointers to keep the previous 2 values saved instead of keeping everything in the `dp` array.
