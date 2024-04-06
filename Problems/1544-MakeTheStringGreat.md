---
tags:
  - easy
---
# 1544. Make the String Great
Link: [here](https://leetcode.com/problems/make-the-string-great/description/)
## Problem
Given a string `s` of lower and upper case English letters.

A good string is a string which doesn't have **two adjacent characters** `s[i]` and `s[i + 1]` where:
- `0 <= i <= s.length - 2`
- `s[i]` is a lower-case letter and `s[i + 1]` is the same letter but in upper-case or **vice-versa**.
To make the string good, you can choose **two adjacent** characters that make the string bad and remove them. You can keep doing this until the string becomes good.

Return _the string_ after making it good. The answer is guaranteed to be unique under the given constraints.

**Notice** that an empty string is also good.
## Intuition
- We can brute force solve this recursively, by scanning the string and removing the first invalid pair of chars we see, then calling the method on it again
- Brute force gives an $O(n^2)$ time complexity (but you might be able to do better by keeping track of the current index of the string)
- Since we want to keep track of not only the current chars, but also compare the chars when items have been removed, a stack makes more sense
## Approach
- Make a stack
- Move through elements in the stack
- Compare the top of the stack to the current element in the string
- If they are the same character but one is uppercase and one is lowercase, then we want to remove them. In this case, that looks like just not adding the element to the stack
- In the end, all non-popped elements will be combined into the answer string
## Edge Cases/Pitfalls
- If the stack is empty, you shouldn't check the TOS
## Solution
```python 
class Solution:
    def makeGood(self, s: str) -> str:
        stack = []
        for i in range(len(s)):
            if stack and stack[-1] != s[i] and stack[-1].lower() == s[i].lower():
                stack.pop()
            else:
                stack.append(s[i])
        return ''.join(stack)
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
## Takeaways 
- Stacks are good when you want to process elements but also preserve previous elements that may need to be changed based on what is currently being looked at
