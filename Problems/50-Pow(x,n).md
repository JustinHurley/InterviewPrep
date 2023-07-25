---
tags:
- math
- recursion
---
### 50. Pow(x,n)

Link: [here](https://leetcode.com/problems/powx-n/description/)

#### Problem
Implement [pow(x, n)](http://www.cplusplus.com/reference/valarray/pow/), which calculates `x` raised to the power `n` (i.e., `x^n`).

#### Approach
Besides `x ** n`, you can solve this problem efficiently using a property inherent to powers and bases. While we could iteratively multiply `x` a total of `n` times, we could also do "binary exponentiation", which lets you split the exponent in half.
```math
x^n = ?
x^n = (x^2)^(n/2) 
x^n = (x*x)^(n/2)
```
We use the fact that when a number raised to a power, is then raised to a power, you can multiply the powers together to get an answer. So instead of saying `2^10` , you could express it as `(2*2)^5` . We then just make recursive calls with the amounts, halving `n` each time until we are done.

#### Edge Cases
- When `n == 0` we are raising a number to the 0th power, which is always 1.
- Handling when `n` is odd, since you don't want to end up with an exponent power, or lose the extra 1 because of rounding.

#### Solution
```python 
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if n == 0:
            return 1
        if n < 0:
            return 1/self.myPow(x,-n)

        if n % 2 == 0:
            return self.myPow(x*x, n//2)
        else:
            return self.myPow(x*x, (n-1)//2) * x
```

#### Time Complexity
`O(log(n))`

#### Space Complexity
`O(log(n))` for the call stack.
