---
tags:
  - medium
  - dynamic_programming
  - memoization
---
# 494. Target Sum
Link: [here](https://leetcode.com/problems/target-sum/description/)
## Problem
You are given an integer array `nums` and an integer `target`.
You want to build an **expression** out of `nums` by adding one of the symbols `'+'` and `'-'` before each integer in `nums` and then concatenate all the integers.
- For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.
Return the number of different **expressions** that you can build, which evaluates to `target`.
## Main Idea
- Navigate the decision tree for either adding or subtracting a given value
- We can memoize the decision tree by index and remaining value
## Approach
- Make memo dictionary to cache values
- Make the DFS method that takes the current index and current sum
	- If the index is at the end of the array, we should check if we hit the target sum
	- If it is return 1 else 0
	- Now see if we already computed this value and is in the memo table
	- If we haven't we need to actually do the recurrence relation which sets the number of ways equal to the number of ways we can reach the target of the child decision tree nodes when we add the current value and when we subtract the current value
```python 
memo[(i, remainder)] = dfs(i+1, rem+nums[i]) + dfs(i+1, rem-nums[i])
```
## Edge Cases/Gotchas 
- Handle if DFS reaches the end of the `nums` array and didn't find the value
## Solution
```python 
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        memo = {}

        def dfs(i, remainder):
            # If at end of arr, check if we have valid soln
            if i == len(nums):
                return 1 if remainder == 0 else 0
            # Check if already did solution
            if (i, remainder) in memo:
                return memo[(i, remainder)]
            # Recurrence relation 
            memo[(i, remainder)] = dfs(i+1, remainder+nums[i]) + dfs(i+1, remainder-nums[i])
            return memo[(i, remainder)]
        
        return dfs(0, target)
```
## Time Complexity
$O(m*n)$ where $m$ is the size of target and $n$ is the length of `nums`
## Space Complexity
$O(m * n)$
