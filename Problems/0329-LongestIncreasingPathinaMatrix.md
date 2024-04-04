---
tags:
  - hard
  - dynamic_programming
  - memoization
  - matrix
  - depth_first_search
---
# 329. Longest Increasing Path in a Matrix
Link: [here](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/description/)
## Problem
Given an `m x n` integers `matrix`, return _the length of the longest increasing path in_ `matrix`.
From each cell, you can either move in four directions: left, right, up, or down. You **may not** move **diagonally** or move **outside the boundary** (i.e., wrap-around is not allowed).
## Main Idea
- DFS on every cell to find longest path
- Since we are strictly increasing (or decreasing when we actually DFS) we don't need to keep track of visited nodes since we can never revisit a node
## Approach
- Make a memo table
- Make a DFS method that takes in the row and column as arguments
	- If the value was already computed for that val, return it
	- Otherwise we should check all the neighbors, and if they are in bound and smaller than the current cell, we should DFS on that cell as well
- We call this DFS method on all cells, and return whatever the largest path is
## Edge Cases/Gotchas 
- You don't need to use a set to keep track of visited cells 
## Solution
```python 
class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        dirs = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        rows, cols = len(matrix), len(matrix[0])
        memo = {}
        def dfs(r, c):
            # Check in cache
            if (r,c) in memo:
                return memo[(r,c)]
            # Otherwise we need to do work
            best_len = 0
            for dr, dc in dirs:
                # If not OOB and val is lower 
                if r+dr >= 0 and r+dr < len(matrix) and c+dc >= 0 and c+dc < len(matrix[0]):
                    curr, nei = matrix[r][c], matrix[r+dr][c+dc]
                    if curr > nei:
                        best_len = max(best_len, dfs(r+dr, c+dc))
            memo[(r,c)] = best_len + 1
            return best_len + 1

        # Find max path len for each cell
        ans = 0
        for row in range(rows):
            for col in range(cols):
                ans = max(ans, dfs(row, col))
        return ans
```
## Time Complexity
$O(m*n)$ since we only need to compute each cell value once
## Space Complexity
$O(m*n)$ 
