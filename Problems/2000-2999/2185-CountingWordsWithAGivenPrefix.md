---
tags:
  - easy
  - string
  - array
---
### 2185. Counting Words With a Given Prefix

Link: [here](https://leetcode.com/problems/counting-words-with-a-given-prefix/description/)

#### Problem
You are given an array of strings `words` and a string `pref`.
Return _the number of strings in_ `words` _that contain_ `pref` _as a **prefix**_.
A **prefix** of a string `s` is any leading contiguous substring of `s`.

#### Main Idea
- Just look at only the front of each word

#### Approach
- See above

#### Edge Cases/Gotchas 
- Invalid input
- `len(word) < len(pref)`

#### Solution
```python 
class Solution:
    def prefixCount(self, words: List[str], pref: str) -> int:
        return sum(1 for word in words if word[:len(pref)] == pref)
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)` since we technically make the list in the list comprehension 

