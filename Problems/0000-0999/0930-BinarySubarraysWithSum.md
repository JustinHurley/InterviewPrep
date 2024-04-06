---
tags: [medium, prefix_sum, dictionary]
---
# 930. Binary Subarrays with Sum
Link: [here](https://leetcode.com/problems/binary-subarrays-with-sum/)
## Problem
Given a binary array `nums` and an integer `goal`, return _the number of non-empty **subarrays** with a sum_ `goal`.

A **subarray** is a contiguous part of the array.
## Main Idea
- We can use a prefix sum to know when we have reached the goal
- We also keep track of frequency for when we overshoot the goal, but know we can remove values from the left part of the subsequence to reach the goal (e.g. if target is 2 and we have 3, but we have already seen a subsequence of 1, we know that we can remove that 1 from the subsequence)
- When you see contiguous subsequence think prefix sum first
## Approach
- Make variables to track the answer, the current sum, and the frequency of each prefix sum
- Iterate through each num value
	- If our curr sum equals the target, incr. the answer
	- If our curr sum - target is in `freq` it means we can reach the subsequence by removing the left part of the existing subsequence, so we should check to see what `freq[curr_sum - goal]` is and add it to the answer if it exists.
## Edge Cases/Gotchas 
- We need to deal with a potentially infinite number of leading and trailing zeroes
## Solution
```python 
class Solution:
    def numSubarraysWithSum(self, nums: List[int], goal: int) -> int:
        total = 0
        curr_sum = 0
        freq = {}

        for num in nums:
            curr_sum += num
            # If at current goal, incr.
            if curr_sum == goal:
                total += 1
            # If we have pref sum, we can get a soln by removing it
            if curr_sum - goal in freq:
                total += freq[curr_sum - goal]
            freq[curr_sum] = freq.get(curr_sum, 0) + 1
        return total
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
