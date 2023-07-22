---
tags:
- dynamic_programming
- matrix
- depth_first_search
- recursion
---
### 688. Knight Probability in Chessboard

Link: [here](https://leetcode.com/problems/knight-probability-in-chessboard/description/)

#### Problem
On an `n x n` chessboard, a knight starts at the cell `(row, column)` and attempts to make exactly `k` moves. The rows and columns are **0-indexed**, so the top-left cell is `(0, 0)`, and the bottom-right cell is `(n - 1, n - 1)`.

A chess knight has eight possible moves it can make, as illustrated below. Each move is two cells in a cardinal direction, then one cell in an orthogonal direction.

![](https://assets.leetcode.com/uploads/2018/10/12/knight.png)

Each time the knight is to move, it chooses one of eight possible moves uniformly at random (even if the piece would go off the chessboard) and moves there.

The knight continues moving until it has made exactly `k` moves or has moved off the chessboard.

Return _the probability that the knight remains on the board after it has stopped moving_.

#### Approach
For this problem I did the Top-Down or DFS approach. In this approach we start at the desired location, and determine the probability of that space as the sum of the knight positions around it, divided by 8, since there are 8 possible moves. This continues until there are no moves left, and if the piece is in-bounds, return 1. If at any point the piece goes OOB, then return 0. So through this approach, we basically are traversing the decision tree of moves, that can be made at each point. So think of a tree with 8 children at each node, where each child represents one of the potential moves made.
So we would basically traverse the tree until we hit the leaves, where the value is either 0 or 1, and through the recursive-call-stack, these answers "bubble-up" back to the root node, where we can see the final probability for that space.

#### Solution
```python 
class Solution:
    def knightProbability(self, n: int, k: int, rowum: int, column: int) -> float:

        if k == 0:
            return 1

        moves = [[-2,-1], [-1,-2], [-2, 1], [-1, 2], [1, 2], [2, 1], [2, -1], [1, -2]]
        dp = [[[0 for _ in range(k)] for _ in range(n)] for _ in range(n)]

        def simulateMoves(m, row, col):
            # If out of bounds, return 0
            if row < 0 or col < 0 or row >= n or col >= n:
                return 0
            # If it was the last move, return 0 
            if m == 0:
                return 1
            # Otherwise we should see if it's been found before
            here = dp[row][col][m-1]
            if here != 0:
                return here
            
            # Now if we're here we need to check the next moves
            prob = 0
            for r, c in moves:
                prob += simulateMoves(m-1, row+r, col+c)
            prob /= 8
            # Update cell and return
            dp[row][col][m-1] = prob
            return prob
        
        simulateMoves(k, rowum, column)
        return dp[rowum][column][k-1]
```

#### Time Complexity
`O(k*n^2)` since the creation of the DP array takes that long.

#### Space Complexity
`O(k*n^2)` however you can optimize the space usage by using 2 `nxn` matrices where instead of having a 3rd dimension for move, you just swap between the 2 matrices, since there is never a point where you need to relay information over more than one move.