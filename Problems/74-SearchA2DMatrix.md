---
tags:
- matrix
- binary_search
- array
---

### 74. Search a 2D Matrix

Link: [here](https://leetcode.com/problems/search-a-2d-matrix/description/)

#### Problem
You are given an `m x n` integer matrix `matrix` with the following two properties:

Each row is sorted in non-decreasing order.
The first integer of each row is greater than the last integer of the previous row.
Given an integer `target`, return `true` if `target` is in `matrix` or `false` otherwise.

You must write a solution in `O(log(m * n))` time complexity.

#### Approach
Given that the whole matrix is sorted, we can simply run binary search twice to determine if the target element is in the matrix. The first search we do is to determine the potential target row of the matrix, this is done by looking at the last value in the current row relative to the target element and also looking at the first element in the current row relative to the target element. This finds us the row, then once we have the row, we just need to do binary search on that row to determine if the target element is in the matrix.

#### Solution
```python 
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        rows = len(matrix)
        cols = len(matrix[0])

        # First we want to find the right row
        l, r = 0, rows-1

        targetRow = -1
        while l <= r:
            mid = (l+r)//2
            if matrix[mid][0] <= target and matrix[mid][-1] >= target:
                targetRow = mid
                break
            elif matrix[mid][-1] < target:
                l = mid+1
            else:
                r = mid-1
        
        # Now do binary search on target row 
        l, r = 0, cols-1

        while l <= r:
            mid = (l+r)//2
            if matrix[targetRow][mid] == target:
                return True
            elif matrix[targetRow][mid] < target:
                l = mid+1
            else:
                r = mid-1
        
        return False
```

#### Time Complexity
`O(log(m) + log(n)) = O(log(m*n))`

