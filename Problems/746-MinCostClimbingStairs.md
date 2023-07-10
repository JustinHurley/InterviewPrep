---
tags:
- dynamic_programming
- array
---

### 746. Min Cost Climbing Stairs

Link: [here](https://leetcode.com/problems/min-cost-climbing-stairs/)

#### Problem
You are given an integer array `cost` where `cost[i]` is the cost of `ith` step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index `0`, or the step with index `1`.

#### Approach
This problem is very similar to the normal stair climbing problem, except now the steps are weighted and each has a cost. The premise is the same, we can either take 1 step or 2 steps, and either choice has its costs. We want to minimize the steps taken.
We first make an array of size `n+1`. This is because we want to account for the `n` step, which is the end position with no cost. Then we backfill the array, but this time, we move from the front of the array to the end of the array. We do this because we are summing up how many steps we have taken as we go along. 
We start from an index of 2, because we are using previous values to build our current value, and the user can start from either step 0 or step 1.
The crux of the problem can be addressed in this line:
`ans[i] = min(ans[i-1]+cost[i-1],ans[i-2]+cost[i-2])`.
In the above code, `ans[i-1]+cost[i-1]` represents if the user had taken 1 step from their previous step and `ans[i-2]+cost[i-2]`. Represents if they had taken 2 steps. 
We then continue to fill in the array until all `n+1` cells have been populated, and we return the last element in the list (using `ans[-1]`).

#### Solution
```python 
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        n = len(cost)

        # Add the extra one so there is an ending point
        ans = [0 for _ in range(n+1)]
        
        # Now we can backfill
        for i in range(2, n+1):
            ans[i] = min(ans[i-1]+cost[i-1],ans[i-2]+cost[i-2])
        
        # Want to return last element in the array 
        return ans[-1]
```