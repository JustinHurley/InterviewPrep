---
tags: [easy, string_array, math]
---
### 1903. Largest Odd Number in String

Link: [here](https://leetcode.com/problems/largest-odd-number-in-string/description)

#### Problem
You are given a string `num`, representing a large integer. Return _the **largest-valued odd** integer (as a string) that is a **non-empty substring** of_ `num`_, or an empty string_ `""` _if no odd integer exists_.

A **substring** is a contiguous sequence of characters within a string.

#### Main Idea
- Largest odd number is based on the rightmost odd digit

#### Approach
- See aboce

#### Edge Cases
- `num` contains non digit chars

#### Solution
```python 
class Solution:
    def largestOddNumber(self, num: str) -> str:
        for i in range(len(num)-1, -1, -1):
            if int(num[i])%2 == 1:
              return num[:i+1]
        return ""
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)`
