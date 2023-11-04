---
tags: [stack, array, string]
---

### 20. Valid Parentheses

Link: [here](https://leetcode.com/problems/valid-parentheses/description/)

#### Problem
Given a string s containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.

#### Approach
We can use a [[Stack]] to trivialize this problem. We know that every opening bracket needs to correspond with the appropriate closing bracket, and that they need to be in the right order in terms of the open ones and closing ones and also correspond, meaning `({)}` would not count as a valid solution.
By adding each open bracket to the stack, we set up the order that each parenthesis was opened in. When we see a closing stack, we know that we need to find the corresponding open parenthesis, so we can pop the stack and then compare the values, and if they are the right ones and match we are good, but if they aren't then we have an issue. 
There are also a few edge cases that are good to consider on this problem. We can match all the values up in the stack fine, but if there are still leftover values in the stack, it means that not all of the open parentheses were accounted for so this is invalid. Likewise, if we try to pop the stack and see that it is empty, it means we have a closing parenthesis with no possible opening one and are also invalid. Having both these checks ensures that we aren't over-matching or leaving parentheses unaccounted for. 

#### Solution
```python 
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        for i in range(len(s)):
            # If open, add to stack
            if s[i] == '{' or s[i] == '[' or s[i] == '(':
                stack.append(s[i])
            # Otherwise, see if we can match with TOS
            else:
                # If stack is empty, then no match and is invalid
                if len(stack) == 0:
                    return False
                tos = stack.pop()
                if s[i] == '}' and tos != '{':
                    return False
                elif s[i] == ']' and tos != '[':
                    return False
                elif s[i] == ')' and tos != '(':
                    return False
        # If stack is not empty, open parens were unaccounted for and is invalid
        if len(stack) > 0:
            return False
        else:
            return True
```

#### Time Complexity
We look at each element once (and might not even do that necessarily but it's possible). This means there is a time complexity of `O(n)`.

