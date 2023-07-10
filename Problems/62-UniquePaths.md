---
tags:
- dynamic_programming
- array
- matrix
---

### 62. Unique Paths

Link: [here](https://leetcode.com/problems/unique-paths/description/)

#### Problem
There is a robot on an `m x n` grid. The robot is initially located at the top-left corner (i.e., `grid[0][0]`). The robot tries to move to the bottom-right corner (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to `2*10^9`.

#### Approach
This is a permutation problem and thus we are looking to use dynamic programming to solve. In this scenario we first want to think of what the base case is and build off of that. 
For this problem, our base cases are the bottom row and right-most row. Since you can only go down or right, any path in those cells will only have one potential path. 
The next step is to look at how we extrapolate these values, and for this we can fill the array backwards. To get a value, all we need to do is look at the values of the cells to the right and below the current cell, and set the current cell value to the sum of the left and right values, since we can choose either path and end up with either number of resulting paths.
There are a few optimizations that can be made to this problem to make non-exponential improvements to the speed. First we can init all the cells to 1, this saves us the time of having to go back and manually adding the 1s in for the bottom and right-most cells. 
We can also make a space improvement by reusing the DP matrix row instead of actually making a matrix and filling it in. As you can see in the solution, we work on one row at a time and don't really need to look at other rows so we don't need `m*n` space and instead just need `min(m,n)` space.

#### Solution
```python 
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # Build DP array, set vals to 1 as seed vals for bottom and right cells
        dp = [[1 for _ in range(n)] for _ in range(m)]
        
        # Start to fill array, from right->left bottom->top
        for row in range(m-2, -1, -1):
            for col in range(n-2, -1, -1):
                dp[row][col] = dp[row+1][col] + dp[row][col+1]
        
        return dp[0][0]
```

#### Time Complexity
`O(m*n)`

#### Space Complexity
`O(m*n)` but can get it down to `O(min(m,n))` if desired.

