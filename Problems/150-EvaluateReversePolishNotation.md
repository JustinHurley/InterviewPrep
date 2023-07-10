---
tags:
- stack
- array
- math
---

### 150. Evaluate Reverse Polish Notation

Link: [here](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

#### Problem
You are given an array of strings tokens that represents an arithmetic expression in a Reverse Polish Notation.

Evaluate the expression. Return an integer that represents the value of the expression.

Note that:

The valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
Each operand may be an integer or another expression.
The division between two integers always truncates toward zero.
There will not be any division by zero.
The input represents a valid arithmetic expression in a reverse polish notation.
The answer and all the intermediate calculations can be represented in a 32-bit integer.

#### Approach
The general approach is to use a stack to keep track of what we are doing work on, and to quickly access the values we want. To generally evaluate RPN, you go left to right and when you see an operator, evaluate the left 2 values and then keep going. So something like `4 5 6 + -` becomes `4 11 -` which becomes `-7`. This is a great opportunity to use a stack as we want to keep track of the last looked at elements, and when we do evaluate something it goes on the top of the stack.
So to solve this problem we iterate through the array, and look at each token. If the token is a number, we convert to an int and add to the stack. If the number is an operator, we pop the top of the stack twice to get `b` and `a` respectively, and then do `a opr b` to get the result. 
Some pitfalls for this problem involve remembering to convert to a string, and also ensuring that negative numbers round up instead of down. You can do this by doing `int(a/b)` where it will divide, then the int operator will round towards 0. `a//b` would not work in this scenario.

#### Solution
```python 
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for token in tokens:
            if token == '+':
                b = stack.pop()
                a = stack.pop()
                stack.append(a+b)
            elif token == '-':
                b = stack.pop()
                a = stack.pop()
                stack.append(a-b)
            elif token == '*':
                b = stack.pop()
                a = stack.pop()
                stack.append(a*b)
            elif token == '/':
                b = stack.pop()
                a = stack.pop()
                stack.append(int(a/b))
            else:
                stack.append(int(token))
        return stack.pop()
```

#### Time Complexity
Overall time complexity is `O(n)`, but since elements can be added back to the stack once they are evaluated, worst case would be `O(2n)` effectively, as each element could be re-added at most once to the stack. Consider `1 0 0 0 0 0 0 + + + + + +`, where we add `n/2` items back to the stack, which isn't exactly `n`, but in the context of only numbers it is.

