---
tags: [medium, prefix_sum, dictionary]
---
# 560. Subarray Sum Equals K
Link: [here](https://leetcode.com/problems/subarray-sum-equals-k/description/)
## Problem
Given an array of integers `nums` and an integer `k`, return _the total number of subarrays whose sum equals to_ `k`.

A subarray is a contiguous **non-empty** sequence of elements within an array.
## Main Idea
- We can use prefix sums to keep track of the subarray values as we go through `nums`
- Each sum is added to a dictionary to keep track of values
- If we see that the current prefix sum minus `k` already exists in our dictionary, it means that we can remove the earlier prefix sum, to be left with a prefix sum that totals `k`, e.g. for `[1,2,3]` `k = 3`, when we reach `i = 2`, we have a prefix sum of 6, so 6 - 3 is 3, which we already saw earlier in `nums` when we had 1 + 2, which means we can get to 6 by doing 6 - (1 + 2) = 3.
- We use a hashmap instead of a hashset since we want to count the frequency of each sum (since there are negative numbers), if a prefix sum happens multiple times, we want to capture all those times
## Approach
- Loop through `nums`, update the prefix sum, and see if `pref - k` exists in the dictionary
## Edge Cases/Gotchas 
- We need to consider the case when the prefix sum equals `k`. We can do this by adding a subarray sum of 0 to the hashmap, this way if `pref == k`, we will already have `0` in the hashmap
## Solution
```python 
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        # Add 0 for when pref == k
        freqs = {0: 1}
        pref = 0
        total = 0
        # Build prefix sums
        for i in range(len(nums)):
            # Update sum
            pref += nums[i]
            # Check if we can remove an earlier sum
            if pref - k in freqs:
                total += freqs[pref - k]
            # Update the freqs dict
            freqs[pref] = freqs.get(pref, 0) + 1
        return total
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
