---
tags:
  - hard
  - depth_first_search
  - memoization
---
# 10. Regular Expression Matching 
Link: [here](https://leetcode.com/problems/regular-expression-matching/description/)
## Problem
Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:
- `'.'` Matches any single character.​​​​
- `'*'` Matches zero or more of the preceding element.
The matching should cover the **entire** input string (not partial).
## Main Idea
- DP using indicies for `s` and `p` as you process the string
- Handling cases without `*` is trivial, if they match you move both pointers, otherwise return false
- If there is a `*` you need to DFS on the decision tree of either skipping this wildcard, processing this wildcard and stopping at this char, or processing the wildcard and seeing if there are any other chars we can process.
- Hard part is making everything work well and handling edge cases
## Approach
- Make a memo table
- Make a DFS method with indicies for `s` and `p`
	- Check memo table if already computed
	- If we finished `s` and `p`, we did it and return T
	- If we only finished `p` , we didn't do it and return F
	- If we only finished `s` , we potentially have a true solution, iff the remaining `p` substring only includes wildcards since we can skip those
	- Now we actually need to do the work for the problem
	- First check if we are looking at a wildcard in `p`
		- If we are we make the DFS calls on the decision tree:
		- Process char and stop wildcard matching: `dfs(si + 1, pi + 2)`
		- Process char and keep wildcard matching: `dfs(si + 1, pi)`
		- Skip wildcard matching for this char: `dfs(si, pi+2)`
	- If we aren't looking at a wildcard it's much easier
		- If the chars match we can keep going: `dfs(si + 1, pi + 1)`
		- If they don't we return False
## Edge Cases/Gotchas 
- It's important in this problem that we know what it means when we finish processing the `s` string and the `p` string respectively
- Know that you can skip a wildcard match entirely 
## Solution
```python 
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        memo = {}

        def dfs(si, pi):
            # Check memo
            if (si, pi) in memo:
                return memo[(si, pi)]
            # If processed all chars with all regex
            if si >= len(s) and pi >= len(p):
                return True
            # If chars still rem or regex still rem
            elif pi >= len(p):
                return False
            # If at end of string but still have regex left
            elif si >= len(s):
                # We can skip wildcards
                if pi < len(p)-1 and p[pi+1] == '*':
                    return dfs(si, pi+2)
                else:
                    return False
            # If here we need to actually do work
            memo[(si, pi)] = False
            # Check for *
            if pi < len(p)-1 and p[pi+1] == '*':
                if s[si] == p[pi] or p[pi] == '.':
                    # process char and stop with *, process char, skip char
                    memo[(si, pi)] = (dfs(si+1, pi+2) 
                            or dfs(si+1, pi) 
                            or dfs(si, pi+2) 
                            or memo[(si, pi)])
                else:
                    # skip regex
                    memo[(si, pi)] = dfs(si, pi+2) or memo[(si, pi)]
            # If we don't have * it's much easier
            else:
                if s[si] == p[pi] or p[pi] == '.':
                    memo[(si, pi)] = dfs(si+1, pi+1)
            return memo[(si, pi)]
        return dfs(0, 0)
```
## Time Complexity
$O(m*n)$ where m and n are the lengths of the input strings 
## Space Complexity
$O(m*n)$
