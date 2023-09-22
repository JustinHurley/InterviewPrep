---
tags:
- two_pointer
- array
- greedy
---
### 392. Is Subsequence 

Link: [here](https://leetcode.com/problems/is-subsequence/description/)

#### Problem
Given two strings `s` and `t`, return `true` _if_ `s` _is a **subsequence** of_ `t`_, or_ `false` _otherwise_.

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

#### Main Idea
The main idea is we can use [[two pointers]] to keep track of which char we are looking for in `s` as we move through `t`. When `s` and `t` match, we can move the `s` pointer forward, to start looking for the next char.

#### Approach
Pretty much the main idea, we can *greedily* advance the smaller string pointer whenever we match chars. We know this always works because either the char is continuing the substring, or we can't find the next char and return false.
In terms of choosing data structures, we can get away with just using two pointers. Since order matters with the substrings, we can't just count all the chars and add the freqs. to a dictionary.

#### Edge Cases
- `len(s) > len(r)`
- Either strings being null
- Not getting string input

#### Solution
```python 
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        i = 0
        for c in t:
            if i < len(s) and s[i] == c:
                i += 1
        return i == len(s)
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)`

