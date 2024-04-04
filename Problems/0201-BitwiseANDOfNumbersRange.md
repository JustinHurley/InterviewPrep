---
tags: [medium, bit_manipulation, math]
---
# 201. Bitwise AND of Numbers Range
Link: [here](https://leetcode.com/problems/bitwise-and-of-numbers-range/description/)
## Problem
Given two integers `left` and `right` that represent the range `[left, right]`, return _the bitwise AND of all numbers in this range, inclusive_.
## Main Idea
- Shift bits left until the numbers are equal
- Looking to find matching binary prefix 
## Approach
- While the numbers are unequal, shift bits right
- Once equal, can return the common number built by shifting left
## Edge Cases/Gotchas 
- Not in problem but could get an invalid range
## Solution
```python 
class Solution:
    def rangeBitwiseAnd(self, left: int, right: int) -> int:
        shift = 0
        # While left isn't equal to right
        while left < right:
            # Bit shift least significant bit
            left = left >> 1
            right = right >> 1
            shift += 1
        # Return smallest num and shift values back
        return left << shift
```
## Time Complexity
$O(1)$ we do loop through all bits in the integer, but an integer does have a set number of bits
## Space Complexity
$O(1)$
