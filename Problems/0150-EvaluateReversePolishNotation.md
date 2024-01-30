---
tags: [stack, array, math, medium]
---
# 150. Evaluate Reverse Polish Notation

Link: [here](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

## Problem
You are given an array of strings `tokens` that represents an arithmetic expression in a [Reverse Polish Notation](http://en.wikipedia.org/wiki/Reverse_Polish_notation).
Evaluate the expression. Return _an integer that represents the value of the expression_.

**Note** that:
- The valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
- Each operand may be an integer or another expression.
- The division between two integers always **truncates toward zero**.
- There will not be any division by zero.
- The input represents a valid arithmetic expression in a reverse polish notation.
- The answer and all the intermediate calculations can be represented in a **32-bit** integer.

## Main Idea
- We can use a stack to evaluate
- When we see an operator, pop the last 2 elements from the TOS and evaluate
## Approach
The general approach is to use a stack to keep track of what we are doing work on, and to quickly access the values we want. To generally evaluate RPN, you go left to right and when you see an operator, evaluate the left 2 values and then keep going. So something like `4 5 6 + -` becomes `4 11 -` which becomes `-7`. This is a great opportunity to use a stack as we want to keep track of the last looked at elements, and when we do evaluate something it goes on the top of the stack.
So to solve this problem we iterate through the array, and look at each token. If the token is a number, we convert to an int and add to the stack. If the number is an operator, we pop the top of the stack twice to get `b` and `a` respectively, and then do `a opr b` to get the result. 
Some pitfalls for this problem involve remembering to convert to a string, and also ensuring that negative numbers round up instead of down. You can do this by doing `int(a/b)` where it will divide, then the int operator will round towards 0. `a//b` would not work in this scenario.

## Edge Cases/Gotchas 
- We need to be very careful when thinking about how we are going to divide. 
- In Python3 `a//b` rounds down always e.g. `-13/4 = -4.0` since it evals to `-3.2...` which then rounds down to `-4`

## Solution
```python 
lass Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        opr = {
            '+': lambda a, b: a + b, 
            '-': lambda a, b: a - b, 
            '*': lambda a, b: a * b, 
            '/': lambda a, b: int(a / b)
        }
        for token in tokens:
            if token not in opr:
                stack.append(int(token))
            else:
                b = stack.pop()
                a = stack.pop()
                stack.append(opr[token](a,b))
        return int(stack[0])
```

## Time Complexity
Overall time complexity is `O(n)`, but since elements can be added back to the stack once they are evaluated, worst case would be `O(2n)` effectively, as each element could be re-added at most once to the stack. Consider `1 0 0 0 0 0 0 + + + + + +`, where we add `n/2` items back to the stack, which isn't exactly `n`, but in the context of only numbers it is.

## Space Complexity
`O(n)` for the stack 
