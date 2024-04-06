---
tags: [string_array, easy]
---
### 1662. Check If Two String Arrays are Equivalent

Link: [here](https://leetcode.com/problems/check-if-two-string-arrays-are-equivalent)

#### Problem
Given two string arrays `word1` and `word2`, return `true` _if the two arrays **represent** the same string, and_ `false` _otherwise._

A string is **represented** by an array if the array elements concatenated **in order** forms the string.

#### Main Idea
- Use `"".join()`

#### Approach
- See above

#### Edge Cases
- None

#### Solution
```python 
class Solution:
    def arrayStringsAreEqual(self, word1: List[str], word2: List[str]) -> bool:
        return "".join(word1) == "".join(word2)
```

#### Time Complexity
`O(w1 + w2)`

#### Space Complexity
`O(w1 + w2)`

