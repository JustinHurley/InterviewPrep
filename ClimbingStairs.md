### 70. Climbing Stairs

Link: [here](https://leetcode.com/problems/climbing-stairs/)

#### Topics
- Dynamic programming
- Arrays

#### Problem
You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

#### Approach
This is a classic DP problem, where working backwards can find a solution in `O(n)` time. First we find the base cases (a scenario where we know the result). In this case we can use `n = 1` and `n = 2`. When there is only 1 step, you only have 1 option, and when there are 2 steps, you only have 2 options. We can place these at the end of an array, and build up backwards by iterating backwards over the array we're using to store results.
`ans[i] = ans[i+1]+ans[i+2]`
The above is saying that at a certain step `i`, you can either go up 1 step or 2 steps. This accounts for both and the sum of the 2 options will give you the possibilities from that point.

#### Solution
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