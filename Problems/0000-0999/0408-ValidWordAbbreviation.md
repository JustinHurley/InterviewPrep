---
tags: [easy, string, two_pointer]
---
# 408. Valid Word Abbreviation 
Link: [here](https://leetcode.com/problems/valid-word-abbreviation/)
## Problem
A string can be **abbreviated** by replacing any number of **non-adjacent**, **non-empty** substrings with their lengths. The lengths **should not** have leading zeros.

For example, a string such as `"substitution"` could be abbreviated as (but not limited to):
- `"s10n"` (`"s ubstitutio n"`)
- `"sub4u4"` (`"sub stit u tion"`)
- `"12"` (`"substitution"`)
- `"su3i1u2on"` (`"su bst i t u ti on"`)
- `"substitution"` (no substrings replaced)

The following are **not valid** abbreviations:
- `"s55n"` (`"s ubsti tutio n"`, the replaced substrings are adjacent)
- `"s010n"` (has leading zeros)
- `"s0ubstitution"` (replaces an empty substring)

Given a string `word` and an abbreviation `abbr`, return _whether the string **matches** the given abbreviation_.

A **substring** is a contiguous **non-empty** sequence of characters within a string.
## Main Idea
- Use two pointers to process each string at the same time
- If we see a number, capture the entire number and use it to increment the non-abbreviated word pointer
- If we made it to the end of our string and the abbr without returning false, we're good
## Approach
- See solution
## Edge Cases/Gotchas 
- Leading 0s are invalid
- No adjacent abbreviations 
## Solution
```python 
class Solution:
    def validWordAbbreviation(self, word: str, abbr: str) -> bool:
        i, j = 0, 0

        while i < len(word) and j < len(abbr):
            # If not the same and not word
            if word[i] != abbr[j] and not abbr[j].isdigit():
                return False
            # If leading 0
            elif abbr[j] == '0':
                return False
            # If we are dealing with a number
            elif abbr[j].isdigit():
                end = j
                # Capture the whole number
                while end < len(abbr) and abbr[end].isdigit():
                    end += 1
                i += int(abbr[j:end])
                j = end
            # Otherwise biz as usual
            else:
                i += 1
                j += 1
        # If we made it to the end it worked
        return i == len(word) and j == len(abbr)
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
