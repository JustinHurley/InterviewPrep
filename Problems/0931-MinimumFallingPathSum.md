---
tags:
  - dynamic_programming
  - "#bottom_up"
  - matrix
  - medium
---
### 931. Minimum Falling Path Sum

Link: [here](https://leetcode.com/problems/minimum-falling-path-sum/)

#### Problem
Given an `n x n` array of integers `matrix`, return _the **minimum sum** of any **falling path** through_ `matrix`.

A **falling path** starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position `(row, col)` will be `(row + 1, col - 1)`, `(row + 1, col)`, or `(row + 1, col + 1)`.

#### Main Idea
- A given cell's minimum sum value is equal to the value of itself plus the smallest possible next cell it could fall to
- `matrix[row][col] = matrix[row][col] + min(next_left, next_middle, next_right)`
- You can also solve this problem with a top-down/memoization approach 

#### Approach
- Navigate through the matrix, starting from the last row to the top row
- Skip the last row, since those values represent the minimum sum for each cell in the last row (no values in the next row to look at)
- For each cell, look at the potential left, right and middle values
- If on the leftmost or rightmost edge of the row, just set the left or right value to `float('inf')`.
- Set the cell equal to itself plus the smallest running minimum sum of the child nodes

#### Edge Cases
- Since we skip the last row, we need to handle when the input matrix only has one row given 

#### Solution
```python 
class Solution:
    def minFallingPathSum(self, matrix: List[List[int]]) -> int:
        rows, cols = len(matrix), len(matrix[0])
        # Edge case
        if cols == 1:
            return min(matrix[0])
        # Fill from bottom to top, row by row
        for row in range(rows-2, -1, -1):
            # For each row, eval each cell
            for col in range(cols):
                left = float('inf') if col == 0 else matrix[row+1][col-1]
                middle = matrix[row+1][col]
                right = float('inf') if col == cols-1 else matrix[row+1][col+1]
                matrix[row][col] += min(left, middle, right)
        return min(matrix[0])
```

#### Time Complexity
`O(n^2)`

#### Space Complexity
`O(n^2)` or `O(1)` depending on how you treat using the input array to store the DP values.

