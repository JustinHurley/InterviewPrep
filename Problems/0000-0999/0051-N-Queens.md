---
tags:
  - recursion
  - backtracking
  - hard
  - matrix
---
### 51. N-Queens

Link: [here](https://leetcode.com/problems/n-queens/description/)

#### Problem
The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return _all distinct solutions to the **n-queens puzzle**_. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

#### Main Idea
- Use backtracking to permute each possibility 
- Don't backtrack if it will make an invalid answer
- We rely on this statement to determine if a queen is in a valid spot
```python
if (col not in cols) and (row - col not in pos_diag) and (row + col not in neg_diag)
```

#### Approach
-  Make a helper method to build the string/row once we have decided where we want to add the queen
- We will also need 3 sets to track where queens have already been placed, since we are going row-by-row, we only need to track column, and both diagonals.
- To know if a value will be on the positive diagonal, i.e. increasing x-axis and y-axis, we can compare the values of `row - col`. Since both of those values increase by 1, each iteration, the difference between row and col will be constant.
- To know if a value will be on the negative diagonal, i.e increasing x-axis and decreasing y-axis, we know that the sum will always be constant, as if row increases by 1, col will decrease by 1, so we can use `row + col` for the set values
- If our current value is valid, we backtrack by updating all the sets and the current board
- Once we reach `n` rows, we can make a copy of the board and add it to the answer

#### Edge Cases
- `n < 1`

#### Solution
```python 
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        ans = []
        curr_board = []
        cols = set()
        pos_diag = set()
        neg_diag = set()
        
        def buildRow(col):
            ans = ""
            for i in range(n):
                if i == col:
                    ans += "Q"
                else:
                    ans += "."
            return ans
        
        def backtrack(row):
            # Base case if done with board
            if row >= n:
                ans.append(curr_board.copy())
                return
            # See where we can add a queen
            for col in range(n):
                # Check if valid
                if (col not in cols) and (row - col not in pos_diag) and (row + col not in neg_diag): 
                    # Adding elts before backtrack call
                    curr_board.append(buildRow(col))
                    cols.add(col)
                    pos_diag.add(row - col)
                    neg_diag.add(row + col)
                    # Recursive call
                    backtrack(row+1)
                    # Removing items after backtracking
                    curr_board.pop()
                    cols.remove(col)
                    pos_diag.remove(row - col)
                    neg_diag.remove(row + col)

        backtrack(0)
        return ans
```

#### Time Complexity
`O(n!)` since we have `n` options for the first queen, `n-1` options for the second, etc. This doesn't account for diagonals but is a good upper bound

#### Space Complexity
`O(n^2)`

