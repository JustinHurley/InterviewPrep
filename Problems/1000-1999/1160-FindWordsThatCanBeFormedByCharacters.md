---
tags: [dictionary, string, easy]
---
### 1160. Find Words That Can Be Formed by Characters 

Link: [here](https://leetcode.com/problems/find-words-that-can-be-formed-by-characters)

#### Problem
You are given an array of strings `words` and a string `chars`.

A string is **good** if it can be formed by characters from chars (each character can only be used once).

Return _the sum of lengths of all good strings in words_.

#### Main Idea
- Use dictionaries to count the chars and compare the strings
- Since we are only using lowercase chars we could use a len 26 array

#### Approach
- See above

#### Edge Cases
- Invalid input

#### Solution
```python 
class Solution:
    def countCharacters(self, words: List[str], chars: str) -> int:
        count = Counter(chars)
        ans = 0
        for word in words:
            curr_count = Counter(word)
            is_valid = True
            for k in curr_count:
                if k not in count or curr_count[k] > count[k]:
                    is_valid = False
            if is_valid:
                ans += len(word)
        return ans
```

#### Time Complexity
`O(n+m*k)` where `n = len(chars), m = len(words), k = 26`

#### Space Complexity
`O(1)` since max 26 different kinds of chars

