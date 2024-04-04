---
tags:
  - medium
  - string
  - array
---
# 8. String to Integer (atoi)
Link: [here](https://leetcode.com/problems/string-to-integer-atoi/description/)
## Problem
Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer (similar to C/C++'s `atoi` function).

The algorithm for `myAtoi(string s)` is as follows:
1. Read in and ignore any leading whitespace.
2. Check if the next character (if not already at the end of the string) is `'-'` or `'+'`. Read this character in if it is either. This determines if the final result is negative or positive respectively. Assume the result is positive if neither is present.
3. Read in next the characters until the next non-digit character or the end of the input is reached. The rest of the string is ignored.
4. Convert these digits into an integer (i.e. `"123" -> 123`, `"0032" -> 32`). If no digits were read, then the integer is `0`. Change the sign as necessary (from step 2).
5. If the integer is out of the 32-bit signed integer range `[-231, 231 - 1]`, then clamp the integer so that it remains in the range. Specifically, integers less than `-231` should be clamped to `-231`, and integers greater than `231 - 1` should be clamped to `231 - 1`.
6. Return the integer as the final result.

**Note:**
- Only the space character `' '` is considered a whitespace character.
- **Do not ignore** any characters other than the leading whitespace or the rest of the string after the digits.
## Main Idea
- Follow the instructions
## Approach
- Follow instructions
- Use `ord()` to get values of chars
- Treat the string like a list of chars
## Edge Cases/Gotchas 
- Pretty much the whole problem
## Solution
```python 
class Solution:
    def myAtoi(self, s: str) -> int:
        # Null check
        if not s:
            return 0
        # Remove leading whitespace
        i = 0
        while i < len(s) and s[i] == ' ':
            i += 1
        s = s[i:]
        if s == '':
            return 0
        # Check to see if sign is present and handle
        hasSign = False
        isNeg = False
        if s[0] == '+' or s[0] == '-':
            hasSign = True
            if s[0] == '-':
                isNeg = True
            s = s[1:]
        # Read in chars until non int char
        i = 0
        while i < len(s) and ord(s[i]) >= ord('0') and ord(s[i]) <= ord('9'):
            i += 1
        s = s[:i]
        # Handle empty string
        if s == '':
            return 0
        # Handle int overflow
        if int(s)-2**31 > 0 and isNeg:
            return -2**31
        if int(s) >= 2**31 - 1 and not isNeg:
            return 2**31-1

        return int(s) if not isNeg else -1 * int(s)
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
