---
tags:
  - dynamic_programming
  - array
  - medium
---

### 213. House Robber II

Link: [here](https://leetcode.com/problems/house-robber-ii/description/)

#### Problem
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

#### Approach
This problem is exactly the same as the first House Robber problem, the only difference is that the houses exist in a circle instead of in a row. This matters for the problem because it now means that if we rob the first house we cannot rob the last house and conversely, if we rob the last house we cannot rob the first house.
So all we have to do is apply the first House Robber solution to 2 cases: `nums[:n-1]`, which is the case where we can rob the first house but not the last house, and `nums[1:]`, where we can't rob the first house and can rob the first house. We apply the House Robber algorithm to both subsets of `nums` and then take the max between the two answers.

Now let's explain how the House Robber algorithm works. First we need to set our pointers that will track the two potential robber's choices in this problem, we will use `rob1` to represent the scenario where the robber robs the house 2 houses ago and the current house, and then we will use `rob2` to represent the scenario where the robber has robbed the previous adjacent house and will skip the current house. For each current house `curr`, we are basically looking to solve:
```
curr = max(rob1 + curr, rob2)
```
So we want the max since we want to maximize the amount of money we steal using the choice we are given. Once we calculate the values, it's time to increment the robber pointers to prepare for the next iteration (the house to the right of curr). We do this by setting `rob1 = rob2` since we have already determined those values and we want rob1 to be 2 houses away from `curr+1`, and we set `rob2 = curr`, since in the next iteration, that will represent the value of `curr` relative to `curr+1`. 

#### Solution
Note we could also reduce duplicated code by creating a helper method that actually runs the House Robber algorithm instead of making 2 for loops.
```
class Solution:
    def rob(self, nums: List[int]) -> int:
        rob1, rob2 = 0, 0
        n = len(nums)

        if n == 1:
            return nums[0]

        # Case where we start from the first elt
        for i in range(n-1):
            curr = max(rob1 + nums[i], rob2)
            rob1 = rob2
            rob2 = curr
        
        best = rob2

        rob1, rob2 = 0, 0
        for i in range(1,n):
            curr = max(rob1 + nums[i], rob2)
            rob1 = rob2
            rob2 = curr
        
        return max(best, rob2)
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)`