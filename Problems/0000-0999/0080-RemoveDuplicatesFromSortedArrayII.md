---
tags:
  - medium
  - array
  - two_pointer
---
# 80. Remove Duplicates from Sorted Array II
Link: [here](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/description/)
## Problem
Given an integer array `nums` sorted in **non-decreasing order**, remove some duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each unique element appears **at most twice**. The **relative order** of the elements should be kept the **same**.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

Return `k` _after placing the final result in the first_ `k` _slots of_ `nums`.

Do **not** allocate extra space for another array. You must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.
## Main Idea
- We shift the array by `k` spaces when we don't have more than 2 duplicates, where `k` is the number of duplicates for the current number
- If we have over 2 duplicates, we skip the copy step and don't advance the `j` pointer, but since the `i` pointer keeps advancing it means the next swap will overwrite the duplicate pointer we are dealing with
- Basically, if we see a duplicate, we keep `j` still, so the next non-duplicate will overwrite the duplicate item 
## Approach
- See above
## Edge Cases/Gotchas 
- Start at 1 so you can check previous element for duplicates
- Reset the duplicate counter when you reach a new number 
## Solution
```python 
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        j = 1
        dupes = 0

        for i in range(1, len(nums)):
            # Check for duplicates
            if nums[i] == nums[i-1]:
                dupes += 1
            else:
                dupes = 0
            # If we have less than 2, overwrite the duplicate
            if dupes < 2:
                nums[j] = nums[i]
                j += 1
        return j
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
