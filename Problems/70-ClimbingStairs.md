---
tags:
  - dynamic_programming
  - array
  - easy
---

### 70. Climbing Stairs

Link: [here](https://leetcode.com/problems/climbing-stairs/)

#### Problem
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

#### Approach
**First Solve**
This is a classic DP problem, where working backwards can find a solution in `O(n)` time. First we find the base cases (a scenario where we know the result). In this case we can use `n = 1` and `n = 2`. When there is only 1 step, you only have 1 option, and when there are 2 steps, you only have 2 options. We can place these at the end of an array, and build up backwards by iterating backwards over the array we're using to store results.
`ans[i] = ans[i+1]+ans[i+2]`
The above is saying that at a certain step `i`, you can either go up 1 step or 2 steps. This accounts for both and the sum of the 2 options will give you the possibilities from that point.
**Second Solve**
This DP problem is quickly solved by determining the rule to get the number of paths to take from step `k` when given steps `k+1` and `k+2`. So you can start with seed values for the goal step and the step before the goal step, and use those values to build your way to the solution. While DP typically calls for using an array, we actually don't need to in this scenario because we only care about the next two values at a time. Also you may have noticed that this pattern follows the Fibonacci sequence.
#### Solution
**First Solve**
```
class Solution:
    def climbStairs(self, n: int) -> int:
        # Make the DP array
        ans = [None for _ in range(n+1)]
        
        # When we're 1 away we have 1 option, and when we're 2 away we have 2
        ans[n-1], ans[n-2] = 1, 2
        
        # Work backwards
        for i in range(n-3, -1, -1):
            # We can either take 1 step forward or 2
            ans[i] = ans[i+1] + ans[i+2]
        
        return ans[0]
```
**Second Solve**
```
class Solution:
    def climbStairs(self, n: int) -> int:
        # This represents the last 2 positions on the stairs (base case seed values)
        # nth step or two: only one option, stop
        # n-1th step or one: only one option, take 1 step
        one, two = 1, 1

        # Now that we know the value for n and n-1, we need to fill in all the prev values
        for i in range(n-1):
            # Save one in a temp var
            temp = one
            # Update one to the previous value
            one = one + two
            # Shift two down by 1
            two = temp
        
        return one
```