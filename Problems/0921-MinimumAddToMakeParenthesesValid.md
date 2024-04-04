---
tags:
  - medium
  - string
  - greedy
---
# 921. Minimum Add to Make Parentheses Valid
Link: [here](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/description/)
## Problem
A parentheses string is valid if and only if:
- It is the empty string,
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
- It can be written as `(A)`, where `A` is a valid string.

You are given a parentheses string `s`. In one move, you can insert a parenthesis at any position of the string.
- For example, if `s = "()))"`, you can insert an opening parenthesis to be `"(**(**)))"` or a closing parenthesis to be `"())**)**)"`.

Return _the minimum number of moves required to make_ `s` _valid_.
## Main Idea
- If we see an open parentheses, add to the count
- If we see a closed parentheses, check the count to see if we can associate it with a `(` 
- If count is 0, we have no open paren, which means we have to insert an open paren to get a valid answer
- If we have a leftover count at the end, it means we have unclosed 
## Approach
- See above
## Edge Cases/Gotchas 
- Invalid input could mess up the problem
## Solution
```python 
class Solution:
    def minAddToMakeValid(self, s: str) -> int:
        count = 0
        add = 0
        for c in s:
            if c == '(':
                count += 1
            else:
                if count == 0:
                    add += 1
                else:
                    count -= 1
        return count + add
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
