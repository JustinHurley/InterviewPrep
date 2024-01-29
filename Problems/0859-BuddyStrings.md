---
tags:
  - easy
  - string
---
### 859. Buddy Strings

Link: [here](https://leetcode.com/problems/buddy-strings/description/)

#### Problem
Given two strings `s` and `goal`, return `true` _if you can swap two letters in_ `s` _so the result is equal to_ `goal`_, otherwise, return_ `false`_._

Swapping letters is defined as taking two indices `i` and `j` (0-indexed) such that `i != j` and swapping the characters at `s[i]` and `s[j]`.
- For example, swapping at indices `0` and `2` in `"abcd"` results in `"cbad"`.

#### Main Idea
- Only two cases where we can swap:
- When the strings differ by 2 chars are are swapped
- When the strings are the same and have an identical char

#### Approach
- Do edge case checks
- Add all differences to an list
- Check the list and process appropriately 

#### Edge Cases/Gotchas 
- Inputs could be unequal length
- Case when words are identical
- Case when chars are different

#### Solution
```python 
class Solution:
    def buddyStrings(self, s: str, goal: str) -> bool:
        if len(s) != len(goal):
            return False
        diffs = []
        # Find all diffs
        for i in range(len(s)):
            if s[i] != goal[i]:
                diffs.append([s[i], goal[i]])
        # Handle case when there are 2 to swap
        if len(diffs) == 2:
            return diffs[0][0] == diffs[1][1] and diffs[0][1] == diffs[1][0]
        # Handle case when items are identical
        elif len(diffs) == 0:
            # We can only swap if there are duplicate letters
            return len(s) != len(set(s))
        return False
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)` since I used a list that grows with the length of each string, but I could alternatively use a size-26 array and just track the letter counts using `ord`. 

