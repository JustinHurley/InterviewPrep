---
tags: [medium, stack, string]
---
# 1249. Minimum Remove to Make Valid Parentheses
Link: [here](https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/description/)
## Problem
Given a string s of `'('` , `')'` and lowercase English characters.
Your task is to remove the minimum number of parentheses ( `'('` or `')'`, in any positions ) so that the resulting _parentheses string_ is valid and return **any** valid string.

Formally, a _parentheses string_ is valid if and only if:
- It is the empty string, contains only lowercase characters, or
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
- It can be written as `(A)`, where `A` is a valid string.
## Main Idea
- Ignore the chars
- If we see an open paren, increment the stack, add to the ans and keep going
- If we see a closed paren, check the stack if there are any open parens to match with, otherwise it needs to be removed
- Once we finish building the ans string, if there are any open parens left in the stack, they need to be removed
## Approach
- See above
## Edge Cases/Gotchas 
- Need to handle extra open and closed parentheses 
- Don't actually need a stack since we only ever add `(`
## Solution
```python 
class Solution:
    def minRemoveToMakeValid(self, s: str) -> str:
        stack = 0
        ans = []
        for c in s:
            # If open, add to stack and ans
            if c == '(':
                stack += 1
                ans.append(c)
            # If closed, only add to ans if valid else remove
            elif c == ')':
                if stack > 0:
                    stack -= 1
                    ans.append(c)
            # If not paren igonore
            else:
                ans.append(c)
        # If we have unresolved ( we need to remove last n (
        if stack > 0:
            i = len(ans)-1
            while stack > 0 and i >= 0:
                if ans[i] == '(':
                    ans[i] = ''
                    stack -= 1
                i -= 1
        return ''.join(ans)
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$ since we need to build the answer string, otherwise $O(1)$
