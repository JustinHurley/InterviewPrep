---
tags:
  - hard
  - memoization
  - two_pointer
  - depth_first_search
  - dynamic_programming
---
# 312. Burst Balloons
Link: [here](https://leetcode.com/problems/burst-balloons/description/)
## Problem
You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `ith` balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return _the maximum coins you can collect by bursting the balloons wisely_.
## Main Idea
- We can't just DFS the decision tree of choosing which balloons to pop, since popping a balloon leads to non-overlapping subproblems e.g. in `[1,2,3,4]` we can't reduce the problem to `[2,3,4]` after looking at the subproblems where we removed 1 and where we didn't remove 1, since having 1 be removed affects the subproblems. So basically, the decision affects the subproblem, and so we can't just traverse the decision tree.
- Instead, we need to traverse the decision tree of which balloon will be popped last, this allows us to go into the DFS tree and know for a fact, that one of the balloons will exist until it's the last one. This allows us to make a recurrence relation like this:
```python
ans = nums[l-1] * nums[i] * nums[r+1] # The last balloon to be popped
ans += dfs(l, i-1) # The left subproblem
ans += dfs(i+1, r) # The right subproblem 
```
- Once we know that `nums[i]` will be the last value present in the array (since we decided it will be the last value present) we can use that as the bounds for the subproblems, we don't have to worry if the border balloon was popped or not 
## Approach
- Extend the `nums` array to include the implied 1s on the borders 
- Make a memo table
- Make the DFS method that takes the left and right indicies as args
	- If `l > r` we are done processing the array
	- Check the memo table to save duplicate work
	- Otherwise, we need to go through all the items in the range from `l` to `r` and recursively choose which balloon of the subset will be the last one to be popped, allowing us to set the borders for the subproblems, and then see how many coins we can get from the subproblems, saving the best one
## Edge Cases/Gotchas 
- Adding the implied 1s to the edges of the input array makes the problem easier to deal with 
- When calling the DFS method, we want to start at `l = 1` and `r = len(nums)-2` since we added the 1s to the array
## Solution
```python 
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        nums = [1] + nums + [1]
        memo = {}

        def dfs(l, r):
            if l > r:
                return 0
            if (l, r) in memo:
                return memo[(l, r)]
            memo[(l, r)] = 0
            # Each cell will be the one removed last
            for i in range(l, r+1):
                coins = nums[l-1] * nums[i] * nums[r+1]
                coins += dfs(l, i-1) + dfs(i+1, r)
                memo[(l, r)] = max(memo[(l, r)], coins)
            return memo[(l, r)]
        
        return dfs(1, len(nums)-2)
```
## Time Complexity
$O(n^3)$ 
## Space Complexity
$O(n^2)$
