---
tags: ["#easy", "#math"]
---
### 1716. Calculate Money in Leetcode Bank

Link: [here](https://leetcode.com/problems/calculate-money-in-leetcode-bank/description/)

#### Problem
Hercy wants to save money for his first car. He puts money in the Leetcode bank **every day**.

He starts by putting in `$1` on Monday, the first day. Every day from Tuesday to Sunday, he will put in `$1` more than the day before. On every subsequent Monday, he will put in `$1` more than the **previous Monday**.

Given `n`, return _the total amount of money he will have in the Leetcode bank at the end of the_ `nth` _day._

#### Main Idea
- Do the math

#### Approach

#### Edge Cases
- Invalid `n`

#### Solution
```python 
class Solution:
    def totalMoney(self, n: int) -> int:
        return sum([(1 + i//7 + i%7) for i in range(n)])
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)`

