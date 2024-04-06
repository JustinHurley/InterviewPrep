---
tags: [medium, array, sorting, greedy]
---
# 2966. Divide Array Into Arrays With Max Difference 
Link: [here](https://leetcode.com/problems/divide-array-into-arrays-with-max-difference/description/)
## Problem
You are given an integer array `nums` of size `n` and a positive integer `k`.
Divide the array into one or more arrays of size `3` satisfying the following conditions:
- **Each** element of `nums` should be in **exactly** one array.
- The difference between **any** two elements in one array is less than or equal to `k`.
Return _a_ **2D** _array containing all the arrays. If it is impossible to satisfy the conditions, return an empty array. And if there are multiple answers, return **any** of them._
## Main Idea
- Sort the array then split into groups of 3
- Greedy property that sorting and grouping by 3 minimizes the difference between grouped elements
- Check if the array is valid once done
## Approach
- See above
## Edge Cases/Gotchas 
- Input array could be not divisible by 3
- There could be negative numbers
- There could be invalid input 
## Solution
```python 
class Solution:
    def divideArray(self, nums: List[int], k: int) -> List[List[int]]:
        nums.sort()
        n = len(nums)
        ans = []
        for i in range(n):
            if (i % 3) == 0:
                ans.append([nums[i]])
            else:
                ans[i//3].append(nums[i])
        # Now check to see if valid
        for arr in ans:
            if abs(arr[2]-arr[0]) > k:
                return []
        return ans
```
## Time Complexity
`O(n)`
## Space Complexity
`O(n)`

