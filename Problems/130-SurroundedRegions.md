---
tags:
  - matrix
  - graph_traversal
  - depth_first_search
  - set
  - medium
---

### 130. Surrounded Regions
Link: [here](https://leetcode.com/problems/surrounded-regions/)

#### Problem
Given an m x n matrix board containing 'X' and 'O', capture all regions that are 4-directionally surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

#### Approach
For this problem, the trick is to not look for what cells need to be replaced, but instead we should look for what cells don't need to be replaced. 
We know that the only Os that won't be replaced are the ones that are on the border and the other Os connected to it, as all the other ones are surrounded by a border of Xs. 
We can navigate around the borders of the matrix, looking for Os. When we see one, we run DFS to look for more Os, replacing each O as we go along, so we can make differentiate between Os that need to be replaced and Os that shouldn't be replaced.
Then, before we submit, simply iterate over the array and change any O you see to an X, and any of the Os that can't be replaced (that were reassigned to differentiate from the other Os) and then return the matrix.

#### Solution
```python 
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        rows = len(board)
        cols = len(board[0])
        visited = set()
        
        def DFS(row, col):
            # We need to chcek the current cell to see if its valid and O
            if 0 <= row and row < rows and 0 <= col and col < cols and board[row][col] == 'O' and (row, col) not in visited:
                # Visit cell
                visited.add((row,col))
                # Change to differet char
                board[row][col] = 'Q'
                # Search adjacent cells
                DFS(row-1,col)
                DFS(row+1,col)
                DFS(row,col-1)
                DFS(row,col+1)
            return 
        
        # First we want to find the Os on the columns as this will show the non-surrounded items
        # Top and bottom row
        for col in range(cols):
            # If we have an O, we need to coordinate solutions 
            if board[0][col] == 'O':
                DFS(0,col)
            if board[rows-1][col] == 'O':
                DFS(rows-1,col)
        
        # Now we can look through the left and right cols (skip already visited)
        for row in range(1,rows-1):
            if board[row][0] == 'O':
                DFS(row,0)
            if board[row][cols-1] == 'O':
                DFS(row,cols-1)
        
        # Now we move through them all and then change Qs to Os and Os to Xs
        for r in range(rows):
            for c in range(cols):
                if board[r][c] == 'Q':
                    board[r][c] = 'O'
                elif board[r][c] == 'O':
                    board[r][c] = 'X'
```
