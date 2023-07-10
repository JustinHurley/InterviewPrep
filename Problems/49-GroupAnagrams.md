---
tags:
- dictionary
- array
- string
---

### \#49. Group Anagrams

Link: [here](https://leetcode.com/problems/group-anagrams/description/)

#### Problem
Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

#### Approach
We can just sort the chars in each of the strings alphabetically, and then use that as the anagram key. To give the answer we can just return the values array of the dictionary.

The interesting part of this problem is the sorting of the strings, which in Python can be done with `sort = ''.join(sorted(s))`. This line converts the string to a sorted char array with `sorted()`, then joins the chars together back into a string with `''.join()`, by joining all the chars to a blank char.

#### Solution
```python 
# Given array of strings
# Group the anagrams together
# Could do this with a dict 
# Have value be a list of valid items
# Key could be sorted str
# Given that each word is less than 100 chars, can consider sorting 0(1) (kinda)

class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        ans = {}
        # Handle empty input case
        if len(strs) < 1:
            return []
        # Loop through strs and sort, then add to dict appropriately 
        for s in strs:
            sort = ''.join(sorted(s))
            ans[sort] = ans.get(sort, [])
            ans[sort].append(s)
        # Return the value set (list of lists)
        return ans.values()
```
