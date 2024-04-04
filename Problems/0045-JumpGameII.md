---
tags: [medium, dynamic_programming, greedy]
---
# 45. Jump Game II
Link: [here](https://leetcode.com/problems/jump-game-ii/description/)
## Problem
You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.
Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:
- `0 <= j <= nums[i]` and
- `i + j < n`
Return _the minimum number of jumps to reach_ `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`.
## Main Idea
- We are basically doing a modified BFS to determine the max and min range of each step starting at index 0
- Iterating over the left and right range is faster than processing the queue 
- For index `i` we determine the range of cells that can be reached from `i`. We then take that range and look through each of those values, seeing if we can get further on this jump-step than on the previous level
## Approach
- Make a counter to keep how many jumps have been done (what jump step is currently being processed)
- Set the left and right pointers that will track the range that can be reached by that given jump (set to 0 initially as we don't know how far we can jump for the first node)
- Have a while loop run while the right pointer hasn't reached the end of the array i.e. before we reached the goal step
- For each iteration, we want to go through the range for that step, and get the furthest distance for that step
- Update the left pointer to one more than the right pointer, the idea is that if we know we can reach the rightmost pointer, we can certainly reach the leftmost
- Update the right pointer to the furthest distance
- Increment the number of jumps before processing the next step
## Edge Cases/Gotchas 
- This problem has no invalid cases, but it could in the future
- No negative numbers but something to consider
- Doing BFS with a range instead of actually keeping a queue makes the problem more efficient 
## Solution
```python 
class Solution:
    def jump(self, nums: List[int]) -> int:
        jumps = 0
        l = r = 0

        while r < len(nums)-1:
            farthest = 0
            # Iterate through range from l to r+1
            for i in range(l, r+1):
                # Update furthest to be curr or dist i+nums[i] can reach
                farthest = max(farthest, i + nums[i])
            # Once we have found range, update pointers and jump level
            l = r+1
            r = farthest
            jumps += 1
        return jumps
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
