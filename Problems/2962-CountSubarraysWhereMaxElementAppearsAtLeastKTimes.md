---
tags:
  - medium
  - sliding_window
  - array
---
# 2962. Count Subarrays Where Max Element Appears at Least K Times
Link: [here](https://leetcode.com/problems/count-subarrays-where-max-element-appears-at-least-k-times/description/)
## Problem
You are given an integer array `nums` and a **positive** integer `k`.

Return _the number of subarrays where the **maximum** element of_ `nums` _appears **at least**_ `k` _times in that subarray._

A **subarray** is a contiguous sequence of elements within an array.
## Intuition
- Subarrays are contiguous so that would make me think sliding window
- The hard part of the problem is knowing how to count subarrays, we want to expand the window right until we have the max element k times, when we have that we then want to shrink the window until it's invalid. Every time we can move the left part of the window over and still be in a valid state means we have a valid subarray
- The idea is that for each valid subarray we find, we could also include the element on the left and it would still be valid, so the left pointer's index represents the count of all of those sliding windows that currently end at the right index of the window
## Approach
- Get the largest value in the list
- Set values to hold the answer, the left pointer, and the current frequency 
- Go through each element in a for loop
	- Update the current frequency if needed
	- While the current frequency is k, we should move up the left pointer if possible
	- Add the current left pointer position to the answer
## Edge Cases/Pitfalls
- The max element is the global max element, not the current max element of the subarray
## Solution
```python 
class Solution:
    def countSubarrays(self, nums: List[int], k: int) -> int:
        # get max
        largest = max(nums)
        l = ans = curr_freq = 0
        # go thorugh
        for r in range(len(nums)):
            # Count max num
            if nums[r] == largest:
                curr_freq += 1
            # Shrink window
            while curr_freq == k:
                if nums[l] == largest:
                    curr_freq -= 1
                l += 1
            # Get count of valid
            ans += l
        return ans
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
## Takeaways 
- We can use the left index of a sliding window as a marker of how many larger arrays that are also valid can be made up to that point, e.g. if `[0, 5, 5]` is valid then `[0, 0, 0, 5, 5]` is also valid
