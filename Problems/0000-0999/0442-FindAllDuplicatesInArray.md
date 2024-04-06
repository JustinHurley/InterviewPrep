---
tags:
  - array
  - medium
---
# 442. Find All Duplicates in Array
Link: [here](https://leetcode.com/problems/find-all-duplicates-in-an-array/)
## Problem
Given an integer array `nums` of length `n` where all the integers of `nums` are in the range `[1, n]` and each integer appears **once** or **twice**, return _an array of all the integers that appears **twice**_.

You must write an algorithm that runs in `O(n)` time and uses only constant extra space.
## Main Idea
- We can't use any other DS to help with the problem, and we need `O(n)` time complexity so we can't sort
- We can use the input array as extra space by using each index as a boolean to store if that index has been visited yet, negative means it has been seen already, positive means it hasn't
## Approach
- Make an answer array
- Iterate through each element of the array
	- Convert the value to `abs()` of the value in case it was changed to negative
	- Look at the index that corresponds to the current value, if it has been seen before, add the current value to the answer array
	- If it hasn't been seen before, change the value of that index to negative
- Return the answer array
## Edge Cases/Gotchas 
- `nums` is in the range `[1, n]` so when going from `num -> i` you need to subtract 1
## Solution
```python 
class Solution:
    def findDuplicates(self, nums: List[int]) -> List[int]:
        ans = []

        for i, num in enumerate(nums):
            # Pull out value
            num = abs(num)
            # Check index for num
            if nums[num-1] < 0:
                ans.append(num)
            else:
                nums[num-1] = -nums[num-1]
        
        return ans
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
