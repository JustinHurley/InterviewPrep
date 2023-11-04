---
tags: [backtracking, recursion, depth_first_search, matrix, string]
---

### 79. Word Search

Link: [here](https://leetcode.com/problems/word-search/)

#### Problem
Given an `m x n` grid of characters `board` and a string `word`, return true if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

#### Approach
The approach to this is pretty similar to most [[Categories/Backtracking|backtracking]] problems, with a slight change along the way.
So you run DFS at each element in the matrix, and then in each DFS algo, you:
- Check to see if the remaining word to build is empty, if so return True.
- Check to see if the cells are valid
- Check to see if the cell is visited
- Visit the cell
- If the current cell matches the next letter, remove that letter from the word and do a recursive call on all the neighbors.
The tricky part of this problem comes in knowing when to return. We simply cannot just return the recursive calls, as many of them will be false, and we will return that as the answer. However the general flow of the code should be to find a true result, and only after all other results have been explored, return false. That is why in the solution we don't do something like `return DFS(row,col,word)`, and instead use the result of the DFS function to return true if that function finds the whole word, otherwise just keep looking. Another reason we don't just return the result is if we were to return False, we would need to keep looking through the array that we didn't "clean up," as we returned the result before we had the chance to "un-visit" the node by putting the original value where the `#` currently is.

#### Solution
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