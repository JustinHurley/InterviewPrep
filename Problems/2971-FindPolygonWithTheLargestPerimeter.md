---
tags: [medium, math, sorting, greedy]
---
# 2971. Find Polygon With the Largest Perimeter
Link: [here](https://leetcode.com/problems/find-polygon-with-the-largest-perimeter/)
## Problem
You are given an array of **positive** integers `nums` of length `n`.
A **polygon** is a closed plane figure that has at least `3` sides. The **longest side** of a polygon is **smaller** than the sum of its other sides.
Conversely, if you have `k` (`k >= 3`) **positive** real numbers `a1`, `a2`, `a3`, ..., `ak` where `a1 <= a2 <= a3 <= ... <= ak` **and** `a1 + a2 + a3 + ... + ak-1 > ak`, then there **always** exists a polygon with `k` sides whose lengths are `a1`, `a2`, `a3`, ..., `ak`.
The **perimeter** of a polygon is the sum of lengths of its sides.
Return _the **largest** possible **perimeter** of a **polygon** whose sides can be formed from_ `nums`, _or_ `-1` _if it is not possible to create a polygon_.
## Main Idea
- Remove the largest element until the largest element in the array is small enough to make a valid polygon
## Approach
- See above
- Sort array
## Edge Cases/Gotchas 
- Handle the case when the array is less than 3 sides 
- Make sure to compare the largest value to the `total/2` and not `total//2` since we don't want to round
## Solution
```python 
class Solution:
    def largestPerimeter(self, nums: List[int]) -> int:
        total = sum(nums)
        nums.sort()
        n = len(nums)
        i = n-1
        # While the largest is too large
        while i > 0 and nums[i] >= total/2:
            # Remove total
            total -= nums.pop()
            # Dec. incr.
            i -= 1
        if len(nums) < 3:
            return -1
        return sum(nums)
```
## Time Complexity
$O(n*log(n))$
## Space Complexity
$O(n)$ if you count the input array otherwise it's $O(1)$
