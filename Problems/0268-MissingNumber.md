---
tags: [easy, math]
---
# 268. Missing Number
Link: [here](https://leetcode.com/problems/missing-number/description/)
## Problem
Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return _the only number in the range that is missing from the array._
## Main Idea
- Use Gauss's Formula: $\frac{n*(n+1)}{2}$
- Take difference of expected and actual sum
## Approach
- See above
## Edge Cases/Gotchas 
- Know to handle even and odd cases
## Solution
```python 
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        return len(nums)*(len(nums)+1)//2 - sum(nums)
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
