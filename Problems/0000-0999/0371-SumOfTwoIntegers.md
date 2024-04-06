---
tags:
  - medium
  - bit_manipulation
  - math
---
# 371. Sum of Two Integers
Link: [here](https://leetcode.com/problems/sum-of-two-integers/description/)
## Problem
Given two integers `a` and `b`, return _the sum of the two integers without using the operators_ `+` _and_ `-`.
## Main Idea
- We can simulate adding by doing an XOR `^` and AND `&` of two numbers to give us the sum and the carried bits, which we then need to add so we repeat the iteration.
- We also need to ensure that the left value is larger so we order it that way
- Python has infinite size bits so we need to remove the negative and save it for later if that's the case
## Approach
- Set both values to be positive, and then make sure the one with greater magnitude (larger) is the first input param
- Extract the sign from the input param
- If both nums are positive or negative, we just need to add by doing XOR and AND, and then repeating that process until we have no carry bits
- If one num is negative (subtraction) we do the same thing but negate the negative bit (make it positive)
- Return the result after the loops are finished, and make sure to reintroduce the negative sign 
## Edge Cases/Gotchas 
- Python has infinite sized bits, so we need to ensure that the left number is larger as we reduce the problem 
- We must handle subtracting separately from adding 
## Solution
```python 
class Solution:
    def getSum(self, a: int, b: int) -> int:
        x, y = abs(a), abs(b)

        # x needs to be >= y
        if x < y:
            return self.getSum(b, a)

        sign = 1 if a > 0 else -1

        # If we are adding
        if a * b >= 0:
            while y:
                x, y = x ^ y, (x & y) << 1
        # If we are subtracting
        else:
            while y:
                x, y = x ^ y, ((~x) & y) << 1
        return x * sign
```
## Time Complexity
$O(1)$ due to 32 bit integer size
## Space Complexity
$O(1)$
