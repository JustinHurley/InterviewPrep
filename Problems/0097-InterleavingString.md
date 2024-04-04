---
tags:
  - medium
  - dynamic_programming
  - memoization
---
# 97. Interleaving String
Link: [here](https://leetcode.com/problems/interleaving-string/)
## Problem
Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an **interleaving** of `s1` and `s2`.
An **interleaving** of two strings `s` and `t` is a configuration where `s` and `t` are divided into `n` and `m` substrings respectively, such that:
- `s = s1 + s2 + ... + sn`
- `t = t1 + t2 + ... + tm`
- `|n - m| <= 1`
- The **interleaving** is `s1 + t1 + s2 + t2 + s3 + t3 + ...` or `t1 + s1 + t2 + s2 + t3 + s3 + ...`
**Note:** `a + b` is the concatenation of strings `a` and `b`.
## Main Idea
- We can solve the problem by only tracking the current index for `s1` and `s2` since they correspond to the index of `s3`
- The recurrence relation has us try to make a substring from either string if the current chars match
## Approach
- Make a memo table
- Check for cases when `len(s1) + len(s2) != len(s3)` since it will be impossible to find an answer in those cases
- Make DFS method that takes the current `s1` and `s2` indicies 
	- If we are at the end of `s3` we found a valid answer 
	- If we already calculated the value from the memo table, return that 
	- Otherwise we need to actually calculate the value, this can be done by looking at the current `s3` char, and incrementing whatever char in `s1` or `s2` that can be used. In the case both chars match we make 2 calls
- Call the DFS method with both pointers set to 0
## Edge Cases/Gotchas 
- The hard part of the question is knowing that we only need to store `i1` and `i2` to keep track of values
- Handling the case where there is no answer based on the sum of the inputs definitely makes the problem space much smaller 
## Solution
```python 
class Solution:
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        memo = {}
        # Edge case len check 
        if len(s1) + len(s2) != len(s3):
            return False
        
        def dfs(i1, i2):
            # If we are at the end of s3
            if i1 + i2 == len(s3):
                return True
            # Check memo table 
            if (i1, i2) in memo:
                return memo[(i1, i2)]
            # Otherwise go and compute
            c3 = s3[i1+i2]
            ans = False
            # If both chars match explore both paths
            if i1 < len(s1) and i2 < len(s2) and c3 == s1[i1] and c3 == s2[i2]:
                ans = dfs(i1+1, i2) or dfs(i1, i2+1)
            elif i1 < len(s1) and c3 == s1[i1]:
                ans = dfs(i1+1, i2)
            elif i2 < len(s2) and c3 == s2[i2]:
                ans = dfs(i1, i2+1)
            memo[(i1, i2)] = ans
            return memo[(i1, i2)]
        return dfs(0, 0)
```
## Time Complexity
$O()$
## Space Complexity
$O()$
