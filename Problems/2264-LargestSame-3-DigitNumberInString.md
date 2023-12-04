---
tags: ["#easy", "#string"]
---
### 2264. Largest Same-3-Digit Number in String

Link: [here](https://leetcode.com/problems/largest-3-same-digit-number-in-string/)

#### Problem
You are given a string `num` representing a large integer. An integer is **good** if it meets the following conditions:
- It is a **substring** of `num` with length `3`.
- It consists of only one unique digit.

Return _the **maximum good** integer as a **string** or an empty string_ `""` _if no such integer exists_.

Note:
- A **substring** is a contiguous sequence of characters within a string.
- There may be **leading zeroes** in `num` or a good integer.

#### Main Idea
- Make a 3 long window and compare strings

#### Approach
- See above

#### Edge Cases
- None

#### Solution
```python 
class Solution:
    def largestGoodInteger(self, num: str) -> str:
        ans = ""
        for i in range(len(num)-2):
            if num[i] == num[i+1] and num[i+1] == num[i+2]:
                if ans == "":
                    ans = num[i]*3
                else:
                    if int(num[i]) > int(ans[0]):
                        ans = num[i]*3
        return ans
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)`

