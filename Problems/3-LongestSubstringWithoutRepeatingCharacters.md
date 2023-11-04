---
tags: [sliding_window, string, Set, dictionary]
---

### 3. Longest Substring Without Repeating Characters

Link: [here](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

#### Problem
Given a string `s`, find the length of the longest 
substring without repeating characters.

#### Approach
For this problem we will use the [[Algorithms/SlidingWindow|sliding window]] approach. To do this, we will start with a left and right pointer, `L` and `R` respectively, and start them on the 0-index of the string. We then move the right pointer through the string, adding each character we see to a `set()`, checking for collisions each time. We also keep track of the length of the longest substring, which we can do by just getting the length of the `set()`. 
If we hit a character in the string that collides with an element in the set, we need to move `L` up until the character is removed from the set, and we are back to a substring with all unique characters again, starting the process from where we left off with `R`. 
This problem can also be solved using a HashSet/dict, but it isn't really necessary as we don't need to count the number of times we've seen every element, just if we've seen it more than once. We do need to keep in mind that we may end up trying to remove an element from the set that isn't there, so will need to have a check to prevent that from happening when we do use a set. 
#### Solution
```python 
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        length = len(s)
        l = 0
        r = 0

        seen = set()
        best = 0
        while r < length:
            # If current char is new, then add to set and keep going
            if s[r] not in seen:
                seen.add(s[r])
                best = max(len(seen), best)
                r += 1
            # Otherwise we need to move up the back
            else: 
                while s[r] in seen:
                    if l >= length:
                        return best
                    if s[l] in seen:
                        seen.remove(s[l])
                    l += 1
        return best
```

