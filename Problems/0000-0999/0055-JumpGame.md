---
tags:
  - medium
  - dynamic_programming
  - greedy
---
# 55. Jump Game
Link: [here](https://leetcode.com/problems/jump-game/description/)
## Problem
You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` _if you can reach the last index, or_ `false` _otherwise_.
## Main Idea
- Use [[DynamicProgramming|DP]] technique of making subproblems 
- Work backwards to determine if each node can reach the edge of the subproblem 
## Approach
- Set the goal index `n-1`
- Now work backwards through the array
	- For each element, determine if it can reach the current goalpost
	- If it can, reset the goalpost to the current goal 
	- Otherwise keep looking
- If the goalpost isn't 0, it means we weren't able to find a valid way from it to the final goalpost 
## Edge Cases/Gotchas 
- Getting stuck on invalid paths (e.g. `[0,0,0,0]`)
## Solution
```python 
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        # What we want to reach
        goal = len(nums)-1
        for i in range(len(nums)-1, -1, -1):
            # Can we reach the goal from the current position?
            if i + nums[i] >= goal:
                # If so update goal to smaller valid index
                goal = i
        return goal == 0
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
