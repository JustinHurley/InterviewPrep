---
tags:
  - backtracking
  - string
  - recursion
  - medium
---
### 1239. Maximum Length of a Concatenated String with Unique Characters

Link: [here](https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/description/)

#### Problem
You are given an array of strings `arr`. A string `s` is formed by the **concatenation** of a **subsequence** of `arr` that has **unique characters**.

Return _the **maximum** possible length_ of `s`.

A **subsequence** is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

#### Main Idea
- Use backtracking to traverse every possible idea
- Skip any scenario that is invalid

#### Approach
- Build a helper method to DFS the decision tree substring builder
- First, check the current string to see if it is longer than the best
- Now, iterate through all potential strings we can add to the answer
- If the current string plus the substring makes a valid string, continue iterating and building that substring, otherwise ignore that substring 

#### Edge Cases
- Invalid strings
- When substrings are already invalid, e.g. `'aa'`

#### Solution
```python 
class Solution:
    def maxLength(self, arr: List[str]) -> int:
        def isStringValid(s):
            return len(s) == len(set(s))

        def helper(i, curr):
            # Now go through all possible starting points
            best[0] = max(best[0], len(curr))
            for j in range(i, len(arr)):
                # If freq != len then there is an issue
                if isStringValid(curr+arr[j]):
                    helper(j+1, curr+arr[j])
        best = [0]
        helper(0, "")
        return best[0]
```

#### Time Complexity
`O(2^n)` technically 

#### Space Complexity
`O(n)` if you consider the space required for the call stack

