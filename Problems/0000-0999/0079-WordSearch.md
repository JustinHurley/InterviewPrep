---
tags:
  - backtracking
  - recursion
  - depth_first_search
  - matrix
  - string
  - medium
---
# 79. Word Search

Link: [here](https://leetcode.com/problems/word-search/)
## Problem
Given an `m x n` grid of characters `board` and a string `word`, return true if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.
## Intuition
- We are given a grid and asked to search, we should immediately be thinking DFS or BFS, and due to the subproblem nature of DFS, it's probably the better choice (although BFS could work as well)
## Approach
The approach to this is pretty similar to most [[Topics/Backtracking|backtracking]] problems, with a slight change along the way.
So you run DFS at each element in the matrix, and then in each DFS algo, you:
- Check to see if the remaining word to build is empty, if so return True.
- Check to see if the cells are valid
- Check to see if the cell is visited
- Visit the cell
- If the current cell matches the next letter, remove that letter from the word and do a recursive call on all the neighbors.
The tricky part of this problem comes in knowing when to return. We simply cannot just return the recursive calls, as many of them will be false, and we will return that as the answer. However the general flow of the code should be to find a true result, and only after all other results have been explored, return false. That is why in the solution we don't do something like `return DFS(row,col,word)`, and instead use the result of the DFS function to return true if that function finds the whole word, otherwise just keep looking. Another reason we don't just return the result is if we were to return False, we would need to keep looking through the array that we didn't "clean up," as we returned the result before we had the chance to "un-visit" the node by putting the original value where the `#` currently is.
## Approach 2
- This approach is very similar to approach 1, the main difference is how we are tracking wether or not a node is visited. In the above example we use the board to track seen, by overwriting the value in the cell to `#`. Here we will use a `set` to keep track of visited values, but the approach is the same
- This approach isn't much better, but you do save some time from having to rebuild strings from the splitting in solution 1
## Solution
```python 
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        rows = len(board)
        cols = len(board[0])
        neighbors = [(-1,0),(1,0),(0,-1),(0,1)]
        # Helper function
        def dfs(row, col, currWord):
            # First check if we have the valid solution
            if len(currWord) == 0:
                return True
            # Then make sure we are on a valid square 
            if 0 <= row and row < rows and 0 <= col and col < cols:
                # Then make sure that we haven't visited the square
                currLetter = board[row][col]
                if currLetter != '#':
                    # If it's not visited, we should mark it as visited
                    # Now we can check if it's the letter we want
                    if currLetter == currWord[0]:
                        # Now we need to make recursive calls with neighbors
                        # Make the change for backtracking
                        board[row][col] = '#'
                        for rowDiff, colDiff in neighbors:
                            # Don't return all results as we only want it to return if it's found the word
                            if dfs(row+rowDiff,col+colDiff,currWord[1:]):
                                return True
                        board[row][col] = currLetter
            return False

        # Now loop through all starting points
        for r in range(rows):
            for c in range(cols):
                if dfs(r,c,word):
                    return True
        
        return False
```
## Solution 2
```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        dirs = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        seen = set()

        def dfs(row, col, i):
            # Check OOB
            if row < 0 or row >= len(board) or col < 0 or col >= len(board[0]):
                return False
            # Check to see if curr char is valid
            ans = False
            if board[row][col] == word[i]:
                if i == len(word)-1:
                    return True
                # Check surrounding chars
                for dx, dy in dirs:
                    # Only check those not already visited
                    if (row+dx, col+dy) not in seen:
                        seen.add((row+dx, col+dy))
                        ans = ans or dfs(row+dx, col+dy, i+1)
                        seen.remove((row+dx, col+dy))
            return ans
        
        for r in range(len(board)):
            for c in range(len(board[0])):
                seen.add((r, c))
                if dfs(r, c, 0):
                    return True
                seen.remove((r, c))
        return False
```