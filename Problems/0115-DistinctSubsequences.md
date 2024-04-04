---
tags:
  - hard
  - string
  - dynamic_programming
  - memoization
  - depth_first_search
---
# 115. Distinct Subsequences
Link: [here](https://leetcode.com/problems/distinct-subsequences/description/)
## Problem
Given two strings s and t, return _the number of distinct_ **_subsequences_** _of_ s _which equals_ t.
The test cases are generated so that the answer fits on a 32-bit signed integer.
## Main Idea
- We can solve this using DFS on the decision tree by using the current index of `s` and `t` to determine the subproblem we are trying to solve
## Approach
- Solution is all in the DFS method
- If we are at the end of the target string, we make a substring so should return 1
- If we are at the end of `s` but not `t`, it means we couldn't make `t` from `s` so should return 0
- If the base cases aren't true, we should check the memo table to see if the value was already computed
- If we need to calculate the value, we should first look at the current pointers for each char, if the current chars are equal, it means we can either advance both pointers (i.e. match the chars and try to solve the subproblem) or we can choose to not include the current `s` char in our current substring and keep looking
- If the characters don't match, we only need to skip the current char in `s` and keep looking
- Call DFS starting at the 0 index of both items
## Edge Cases/Gotchas 
- We stop when we processed all of `t`, we don't care where the `s` index is because if the `t` string is finished we could just not include all strings in `s`
- On the other hand, if `s` is done and `t` isn't we couldn't make a valid substring
## Solution
```python 
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        memo = {}
        def dfs(s_i, t_i):
            # Base case
            if t_i == len(t):
                return 1
            elif s_i == len(s):
                return 0
            # Check cache 
            if (s_i, t_i) in memo:
                return memo[(s_i, t_i)]
            # Do work
            ans = 0
            if s[s_i] == t[t_i]:
                ans = dfs(s_i+1, t_i+1) + dfs(s_i+1, t_i)
            else:
                ans = dfs(s_i+1, t_i)
            memo[(s_i, t_i)] = ans
            return memo[(s_i, t_i)]
        return dfs(0, 0)
```
## Time Complexity
$O(m*n)$ where m and n are the lengths of each string 
## Space Complexity
$O(m*n)$
