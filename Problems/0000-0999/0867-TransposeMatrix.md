---
tags: ["#easy", matrix]
---
### 867. Transpose Matrix

Link: [here](https://leetcode.com/problems/transpose-matrix/description/)

#### Problem
Given a 2D integer array `matrix`, return _the **transpose** of_ `matrix`.

The **transpose** of a matrix is the matrix flipped over its main diagonal, switching the matrix's row and column indices.

#### Main Idea
- `matrix[row][col] -> answer[col][row]`

#### Approach
- See above

#### Edge Cases
- Can't do in place because `len(rows)` doesn't always equal `len(cols)`

#### Solution
```python 
class Solution:
    def transpose(self, matrix: List[List[int]]) -> List[List[int]]:
        rows, cols = len(matrix), len(matrix[0])
        ans = [[] for _ in range(cols)]

        for row in range(rows):
            for col in range(cols):
                ans[col].append(matrix[row][col])

        return ans
```

#### Time Complexity
`O(m*n)`

#### Space Complexity
`O(1)` if not including the answer matrix

