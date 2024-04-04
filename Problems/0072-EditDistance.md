---
tags:
  - medium
  - dynamic_programming
  - depth_first_search
  - memoization
---
# 72. Edit Distance 
Link: [here](https://leetcode.com/problems/edit-distance/)
## Problem
Given two strings `word1` and `word2`, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:
- Insert a character
- Delete a character
- Replace a character
## Main Idea
- Use the indicies of the words as the keys for the memo table
- If chars are equal, move both pointers and do 0 work
- Otherwise you will need to check and see:
	- Replace, move both pointers and do 1 work
	- Insert, move the pointer of the string that wasn't inserted and do 1 work
	- Remove, move the pointer of the string that was remove and do 1 work
## Approach
- First catch an easy edge case where one of the input strings is null
- Now make the memo table and the DFS method that takes in both pointers for each string as an arg
	- The base case is when we reach the end of either string, once that happens we will need to just do inserts for the "empty" string so that they are equal, so we can return the length of whatever remaining substring needed it 
	- Check to see if the value is in the memo table
	- If we have to do work, we consider 2 scenarios: 1 the current chars are equal and no work is needed so we move on, and 2 the current chars are not equal and so we need to either insert, delete, or replace to keep going
## Edge Cases/Gotchas 
- When one string is empty, we know we must insert all the chars that are on the non-empty string. 
## Solution
```python 
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        if not word1 or not word2:
            return max(len(word1), len(word2))

        memo = {}
        def dfs(i1, i2):
            # If end of word1, we add all chars in 2
            if i1 == len(word1):
                return len(word2) - i2
            elif i2 == len(word2):
                return len(word1) - i1
            # Check memo table
            if (i1, i2) in memo:
                return memo[(i1, i2)]
            # If equal, just move on
            min_dist = float('inf')
            if word1[i1] == word2[i2]:
                memo[(i1, i2)] = min(dfs(i1+1, i2+1), min_dist)
            else:
                # Replace, delete, add
                memo[(i1, i2)] = min(dfs(i1+1, i2+1)+1, dfs(i1+1, i2)+1, dfs(i1, i2+1)+1)
            return memo[(i1, i2)]
        return dfs(0,0)
```
## Time Complexity
$O(m*n)$ where they represent the lengths of the strings
## Space Complexity
$O(m*n)$
