---
tags:
  - easy
  - cycle
  - math
---
# 202. Happy Number
Link: [here](https://leetcode.com/problems/happy-number/)
## Problem
Write an algorithm to determine if a number `n` is happy.
A **happy number** is a number defined by the following process:
- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
- Those numbers for which this process **ends in 1** are happy.
Return `true` _if_ `n` _is a happy number, and_ `false` _if not_.
## Main Idea
- This is really a cycle detection problem with some math sprinkled in
- Use a set to make sure you haven't hit the same number twice
## Approach
- Make a set
- While `n` is greater than 0 (meaning the number isn't done)
	- Check if we have already seen the number (if we have we have a cycle) and return if it's equal to 1 or not 
	- If not add the value to the set 
	- Then compute the next happy number in the problem
## Edge Cases/Gotchas 
- Need a set to track cycles
- Might be nice to break out the next happy number iteration into its own method 
## Solution
```python 
class Solution:
    def isHappy(self, n: int) -> bool:
        vals = set()
        # While number is not resolved
        while n > 0:
            if n in vals:
                return n == 1
            vals.add(n)
            sums = 0
            # Convert n to next number
            while n > 0:
                sums += (n%10) ** 2
                n //= 10
            n = sums
        return True
```
## Time Complexity
$O(log(n))$ since we process each number based on the number of digits i.e. $log_{10}{(n)}$
## Space Complexity
$O(n)$ but this can also be done in $O(1)$ using a fast and slow pointer ([[Floyd'sAlgorithm]])
