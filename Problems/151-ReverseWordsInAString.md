---
tags:
- string_array
---

### 151. Reverse words in a String

Link: [here](https://leetcode.com/problems/reverse-words-in-a-string/)

#### Problem
Given an input string `s`, reverse the order of the words.

A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

Note that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

#### Approach
The approach is to split the string up into an array, separating by spaces, reversing the array, and then converting the array back into a word.
The important part of this question is to note that you need to remove whitespace if there is extra so make sure to only append non-empty strings to the answer string.

#### Solution
```python 
class Solution:
    def reverseWords(self, s: str) -> str:
        # Split up word into array with " "
        words = str.split(s, " ")
        # Reverse the array
        words.reverse()
        ans = ""
        # Build the answer str from the array, need to ignore blank strings
        for word in words:
            if word != "":
                ans = ans + word + " "
        # Return without the last elt
        return ans[0:len(ans)-1]
```