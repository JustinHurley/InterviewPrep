---
tags:
  - easy
  - math
  - bit_manipulation
---
# 231. Power of Two
Link: [here](https://leetcode.com/problems/power-of-two/description/)
## Problem
Given an integer `n`, return _`true` if it is a power of two. Otherwise, return `false`_.
An integer `n` is a power of two, if there exists an integer `x` such that `n == 2x`.
## Main Idea
- Convert to binary, a power of two will only have 1 bit
## Approach
- Convert to binary from the int
- Count how many 1s and 0s are present
- If there is only one 1, and the number isn't negative, it's a power of two
## Edge Cases/Gotchas 
- Negative numbers
- Handling 0
- Not worried since only ints but we could hypothetically needed to handle square roots
## Solution
```python 
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
        if n < 0:
            return False
        binary = Counter(bin(n)[2:])
        return '1' in binary and binary['1'] == 1
```
## One-Liner
```python
class Solution:
    def isPowerOfTwo(self, n: int) -> bool:
       return bin(n).count('0')==len(bin(n))-2
```
## Time Complexity
$O(log(n))$
## Space Complexity
$O(n)$ but could do in $O(1)$
