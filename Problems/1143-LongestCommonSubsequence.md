---
tags: [dynamic_programming, array, string]
---

### 1143. Longest Common Subsequence

Link: [here](https://leetcode.com/problems/longest-common-subsequence/description/)

#### Problem
Given two strings `text1` and `text2`, return the length of their longest common subsequence. If there is no common subsequence, return 0.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, `"ace"` is a subsequence of `"abcde"`.
A common subsequence of two strings is a subsequence that is common to both strings.

#### Approach
So first off, it's important to read the question and actually understand what a longest common subsequence is. It is the longest subsequence that can be shared by two strings that is created only by removing chars from either string. So in other words, as long as the order is the same, we can ignore any characters that don't match.
So now comes to actually solving the problem, since we would need to check permutations of each possible substring, a brute force approach would take way too long, around `O(2^k)` where `k` is the length of the smaller string. 
So as normal, when we see permutations and no better solution, we think DP. In this case we need to consider how we want to build our DP array. In this case, it's actually a matrix, since we are dealing with 2 variables: either string subproblem, hence we will end up with an `len(text1)+1*len(text2)+1` array. The extra row and column is to capture what happens when we compare a string to a blank string (which will always yield 0). The empty string comparison will be our base case. 
We then need to go in and backfill all the information, starting from the end of the string to the front. The idea is we solve the smallest subproblem first (in this case comparing the last 2 strings), and the building from there. 
As we iterate backwards through the matrix, we consider 2 cases:
1. The current chars match. When this happens, we simply look to the bottom-right diagonal in the DP array, as that represents the LCS of the 2 strings starting after the current chars being evaluated, and adds 1 to that value. This approach ensures that we take the best value of the subproblem, and combine it with the current evaluation.
2. The other case to consider is when the current chars don't match. When this happens, we just want to update the current cell with the best LCS so far. To do this, we just look at the cell to the right and the cell under the current cell, and take the max of both values. This works because we are taking the best value of each subproblem when we ignore one of the current chars. We can't just look diagonally because we don't want to skip the cells under and to the right of the current cell where we are evaluating the cases without either current char.

#### Solution
```python 
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        dp = [[0 for _ in range(len(text2)+1)] for _ in range(len(text1)+1)]

        for i in range(len(text1)-1, -1, -1):
            for j in range(len(text2)-1, -1, -1):
                # Case where curr letters match
                if text1[i] == text2[j]:
                    # We look at the next letter pair to see if it also matches
                    dp[i][j] = 1 + dp[i+1][j+1]
                # If the current letters don't match
                else:
                    # The current val is just equal to the max substr possible from the subproblem
                    dp[i][j] = max(dp[i][j+1], dp[i+1][j])
        
        # Return the backfilled amount 
        return dp[0][0]
```

#### Time Complexity
`O(len(text1)*len(text2))`

#### Space Complexity
`O(len(text1)*len(text2))`
