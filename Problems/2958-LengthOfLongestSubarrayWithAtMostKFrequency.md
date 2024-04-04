---
tags:
  - medium
  - sliding_window
  - dictionary
---
# 2958. Length of Longest Subarray With at Most K Frequency 
Link: [here](https://leetcode.com/problems/length-of-longest-subarray-with-at-most-k-frequency/)
## Problem
You are given an integer array `nums` and an integer `k`.
The **frequency** of an element `x` is the number of times it occurs in an array.
An array is called **good** if the frequency of each element in this array is **less than or equal** to `k`.
Return _the length of the **longest** **good** subarray of_ `nums`_._
A **subarray** is a contiguous non-empty sequence of elements within an array.
## Intuition
- Since we want a contiguous array, we can use a sliding window to keep track of it as we move through `nums` and a dictionary to keep track of frequency 
- If we ever have a frequency of a number that is greater than `k` we can just move the left pointer forward until the subarray is in a valid state again
## Approach
- Make a left pointer and a dictionary, also init the longest value to 0
- Go through `nums` with the right pointer
- Add each element to the hashmap
- If the hashmap has a frequency that's too large, we call a while loop that moves up the left pointer and removes elements until it's valid again
## Edge Cases/Pitfalls
- We are returning the length of the subarray at the end so it's `right - left + 1`
## Solution
```python 
class Solution:
    def maxSubarrayLength(self, nums: List[int], k: int) -> int:
        l = 0
        freq = defaultdict(int)
        best = 0
        for r in range(len(nums)):
            # Add right element
            freq[nums[r]] += 1
            # Check if valid
            while freq[nums[r]] > k:
                freq[nums[l]] -= 1
                l += 1
            # Grab subarray len
            best = max(best, r - l + 1)
        return best
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
## Takeaways 
- When the question is asking for a contiguous subarray, it might be asking for a sliding window approach 
- We can use a dictionary to keep track of frequency as we encounter elements 
