---
tags:
  - two_pointer
  - regex
  - string
  - easy
---

### 125. Valid Palindrome

Link: [here](https://leetcode.com/problems/valid-palindrome/description/)

#### Problem
A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` if it is a palindrome, or `false` otherwise.

#### Approach
Pretty simple approach, run regex on the string to remove all non-alphanumeric characters, then convert all uppercase chars to lowercase. 
The just iterate on the string starting from the front and the back, which can be done via `s[i]` and `s[len(s)-i-1]`, remember the extra `-1` for the reverse or you'll have an out of bounds exception on the first iteration. You only have to go halfway through the string as you're checking the front and back simultaneously, and also can just divide by 2 and round down, since if the length is odd, the middle char does't matter (will not have a corresponding char).
#### Solution
```python 
import re

class Solution:
    def isPalindrome(self, s: str) -> bool:
        # Runs regex on s
        s = re.sub(r'[^a-zA-Z0-9]', '', s)
        # Converts to lowercase
        s = s.lower()
        l = len(s)
        # Iterates from front and back at the same time
        for i in range(l//2):
            if s[i] != s[l-i-1]:
                return False
        return True
```