---
tags:
  - medium
  - stack
  - math
  - string
---
# 227. Basic Calculator II
Link: [here](https://leetcode.com/problems/basic-calculator-ii/)
## Problem
Given a string `s` which represents an expression, _evaluate this expression and return its value_. 
The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of `[-231, 231 - 1]`.

**Note:** You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.
## Main Idea
- If we see a number, keep building it 
- If we see a operator, we should act according to what the last operator was, and then keep going
- The idea of this problem is that we want to give `*` and `/` precedence over `+` and `-` . This can be done by using a stack to keep track of past operations, and ensuring that if we see a `*` or `/`, we process the last element in the stack
- The for loop will not process the last current number in the stack, so we can hackily force that by adding a `+` to the end of the input string
- This can be done without a stack as well for `O(1)` memory usage
## Approach
- Make a stack to use, along with a curr variable to hold the current value
- Set the operator to `+` since we are technically adding the first value
- For each char in the input string
	- If we see a digit, keep building the number that we are currently looking at 
	- If we see an operator, we need to look and see what the last operator we visited before this one was
		- If it was `+` we can just add the current value to the heap like normal
		- If it was `-` we add the negative of the current value since we are subtracting 
		- If it was `*` then we need to ensure that we do the multiplication operation before the adding, this can be done by popping the last element in the array, which will take the left number in the multiplication, and then it will add it back to the result
		- Same logic with `/` 
		- After we handle the operator we update it to the current operator we just saw, and also reset the current number
## Edge Cases/Gotchas 
- `s` is None
- Invalid inputs 
## Solution
#### With Stack
```python 
class Solution:
    def calculate(self, s: str) -> int:
        stack = []
        curr = 0
        oper = '+'

        for c in s+'+':
            if c.isdigit():
                curr = curr*10 + int(c)
            elif c != ' ':
                if oper == '-':
                    stack.append(-curr)
                elif oper == '+':
                    stack.append(curr)
                elif oper == '*':
                    stack.append(stack.pop() * curr)
                elif oper == '/':
                    stack.append(int(stack.pop() / curr))
                oper = c
                curr = 0
        return sum(stack)
```
#### Without Stack
```python
class Solution:
    def calculate(self, s: str) -> int:
        last = 0
        curr = 0
        ans = 0
        oper = '+'

        for c in s+'+':
            if c.isdigit():
                curr = curr*10 + int(c)
            elif c != ' ':
                if oper == '-' or oper == '+':
                    ans += last
                    last = curr if oper == '+' else -curr
                elif oper == '*':
                    last = last * curr
                elif oper == '/':
                    last = int(last / curr)
                oper = c
                curr = 0
        ans += last
        return ans
		```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$ or $O(1)$ if not using stack
