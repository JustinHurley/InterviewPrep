---
tags:
  - math
  - recursion
  - medium
---
# 50. Pow(x,n)
Link: [here](https://leetcode.com/problems/powx-n/description/)
## Problem
Implement [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/), which calculates `x` raised to the power `n` (i.e., `x^n`).
## Intuition
- Assume you can't do `x ** n`
- We could multiply `x` `n` times, but that wouldn't fix our issue
- We can use binary exponentiation to split `x` in half 
- $x^n = x^{2^{n/2}} = (x*x)^{n/2}$ 
- It's very easy to find `x*x` and we make the exponent half of it's original value 
## Approach
Besides `x ** n`, you can solve this problem efficiently using a property inherent to powers and bases. While we could iteratively multiply `x` a total of `n` times, we could also do "binary exponentiation", which lets you split the exponent in half.
```math
x^n = ?
x^n = (x^2)^(n/2) 
x^n = (x*x)^(n/2)
```
We use the fact that when a number raised to a power, is then raised to a power, you can multiply the powers together to get an answer. So instead of saying `2^10` , you could express it as `(2*2)^5` . We then just make recursive calls with the amounts, halving `n` each time until we are done.
## Edge Cases
- When `n == 0` we are raising a number to the 0th power, which is always 1.
- When `x == 0` we just return 0
- We need to handle when `n` is odd in a given recursive call, so we split into $x^{n/2} * x^{n/2} * x$ (note the extra x at the end)
## Solution
```python 
class Solution:
    def myPow(self, x: float, n: int) -> float:
        # 0 to any power is 0
        if x == 0: return 0
        # x to 0 is 1
        if n == 0: return 1
        # x to 1 is x
        elif n == 1:
            return x
        # If negative, return reciprocal of power
        elif n < 0:
            return 1 / self.myPow(x, -n)
        # Otherwise, split problem space in 2
        else:
            return self.myPow(x * x, n//2) * (x if n%2 == 1 else 1)
```
## Time Complexity
$O(log(n))$
## Space Complexity
$O(log(n))$ for the call stack.
