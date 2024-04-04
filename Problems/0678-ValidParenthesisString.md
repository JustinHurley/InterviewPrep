---
tags: [medium]
---
# 678. Valid Parenthesis String
Link: [here](https://leetcode.com/problems/valid-parenthesis-string/description/)
## Problem
Given a string `s` containing only three types of characters: `'('`, `')'` and `'*'`, return `true` _if_ `s` _is **valid**_.
The following rules define a **valid** string:
- Any left parenthesis `'('` must have a corresponding right parenthesis `')'`.
- Any right parenthesis `')'` must have a corresponding left parenthesis `'('`.
- Left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
- `'*'` could be treated as a single right parenthesis `')'` or a single left parenthesis `'('` or an empty string `""`.
## Main Idea
- We can count how many left parens are open at the moment, but treat it as a range, since the wildcard could be an open or closed paren
- If the number of maximum left parens ever goes negative, we are in a scenario where we had a right paren with no paired left paren to add to
## Approach
- Make to variables to track the max and min number of parentheses open
- Iterate through the input string 
	- If you see a left paren, incr. the min and max left vals
	- If you see a right paren, decr. the min and max right vals
	- If you see a wildcard, we need to treat it as both, so decrement the min counter and increment the max counter 
	- If the max number of left parens is ever negative, we know that even with the max amount of left parens available, we don't have a valid soln and return false
	- To ensure that we don't end up checking an impossible state, if the min number of parens ever goes below 0, we just reset it to 0, since we could end up with a negative min balance, and then have a right paren set the value to 0, making it valid
## Edge Cases/Gotchas 
- Understanding setting the minLeft value to 0 when it goes negative is an important edge case that is hard to understand 
## Solution
```python 
class Solution:
    def checkValidString(self, s: str) -> bool:
        minLeft, maxLeft = 0, 0

        for i, c in enumerate(s):
            if c == '(':
                minLeft += 1
                maxLeft += 1
            elif c == ')':
                minLeft -= 1
                maxLeft -= 1
            else:
                minLeft -= 1
                maxLeft += 1
            # For cases when more right than left
            if maxLeft < 0:
                return False
            # For cases like (*)(
            if minLeft < 0:
                minLeft = 0
        # If minLeft == 0 then all parens were paired 
        return minLeft == 0
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
