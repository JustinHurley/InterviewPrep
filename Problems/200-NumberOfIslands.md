---
tags: [recursion, matrix, graph_traversal, depth_first_search]
---

### 200. Number of Islands
Link: [here](https://leetcode.com/problems/number-of-islands/)
  
#### Problem
Given an `m x n` 2D binary grid grid which represents a map of `'1's` (land) and `'0's` (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

#### Approach
The way to solve this problem is by using a graph traversal, and keeping track of 'visited' nodes. We need to go through every element of the matrix, and for each element we check if that element is '1' or not. 
If the element is '1', then we do Depth-First-Search on that element, searching all the adjacent cells, and marking any that are '1' as '0'. This prevents us from double counting an island.
Once the island has been fully visited, we keep moving through the matrix, skipping nodes that are 0 and nodes that are already visited.

Something to keep in mind is the fact that the 1s in the problem are not integers but strings, so make sure to clarify that if asked in an interview.

#### Solution
Note: You could also just put the DFS function in the `numIslands` function so that you don't need to pass in `grid` as a parameter and can just call it like a global variable since it's in the scope of the function.
```
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        totalRows = len(grid)
        totalCols = len(grid[0])
        
        islands = 0
        # We need to iterate through the array
        for r in range(totalRows):
            for c in range(totalCols):
                # If the cell is 1 we want to do search the whole island 
                if grid[r][c] == '1':
                    islands += 1
                    Solution.dfs(r, c, grid)
        return islands
    
    # DFS function 
    def dfs(r, c, grid):
        # First we check if we on an valid cell
        if r < len(grid) and r >= 0 and c < len(grid[0]) and c >= 0 and grid[r][c] == '1':
            # If we are, we need to "visit" this cell
            grid[r][c] = 0
            # Then we need to keep looking for adjacent land
            Solution.dfs(r-1,c,grid)
            Solution.dfs(r+1,c,grid)
            Solution.dfs(r,c-1,grid)
            Solution.dfs(r,c+1,grid)
        return 
```