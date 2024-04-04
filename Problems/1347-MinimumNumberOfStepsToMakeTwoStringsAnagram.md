---
tags: [medium, dictionary]
---
# 1347. Minimum Number of Steps to Make Two Strings Anagram
Link: [here](https://leetcode.com/problems/minimum-number-of-steps-to-make-two-strings-anagram/description/)
## Problem
You are given two strings of the same length `s` and `t`. In one step you can choose **any character** of `t` and replace it with **another character**.
Return _the minimum number of steps_ to make `t` an anagram of `s`.
An **Anagram** of a string is a string that contains the same characters with a different (or the same) ordering.
## Main Idea
- Make 2 hash maps to get the frequencies 
- Compare frequencies 
- Divide answer by 2
## Approach
- See above
## Edge Cases/Gotchas 
- You need to divide the difference by 2 since `'a'` and `'b'` would have hash maps that differ by 1 each, making the total difference two, i.e. each swap is implied by 2 differing items.
## Solution
```python 
class Solution:
    def minSteps(self, s: str, t: str) -> int:
        s_freq = Counter(s)
        t_freq = Counter(t)
        diff = 0
        for a in string.ascii_lowercase:
            s_val = s_freq.get(a, 0)
            t_val = t_freq.get(a, 0)
            diff += abs(s_val - t_val)
        return diff//2
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
