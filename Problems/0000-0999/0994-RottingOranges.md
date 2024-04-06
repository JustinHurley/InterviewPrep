---
tags: [graph, breadth_first_search, medium]
---
### 994. Rotting Oranges

Link: [here](https://leetcode.com/problems/rotting-oranges/description/)

#### Problem
You are given an `m x n` `grid` where each cell can have one of three values:

- `0` representing an empty cell,
- `1` representing a fresh orange, or
- `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.

#### Main Idea
- Use [[BreadthFirstSearch|BFS]] to determine if more oranges need to be added
- Count the number of fresh oranges beforehand to determine if some were missed

#### Approach
- Make a `deque` to be used as the queue for BFS
- Now navigate through the array, to count the number of fresh oranges, and to add the rotten oranges to the queue
- In BFS we want to iterate through every element in the queue in a `for` loop., which allows us to process the queue in bulk first, before moving onto the newly added queue elements
- If we find a fresh orange, update the counter and continue
- After the while loop, if we still have fresh oranges, return `-1` otherwise just return the time (number of while loop iterations)

#### Edge Cases
- len(rows) or len(cols) is 0
- grid contained invalid values

#### Solution
```python 
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        Q = deque()
        time, fresh = 0, 0
        rows, cols = len(grid), len(grid[0])

        directions = [[-1, 0], [0, -1], [1, 0], [0, 1]]

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == 1:
                    fresh += 1
                elif grid[r][c] == 2:
                    Q.appendleft((r, c))
        
        while Q and fresh > 0:
            for _ in range(len(Q)):
                r, c = Q.pop()
                for dr, dc in directions:
                    row, col = r+dr, c+dc
                    if row >= 0 and col >= 0 and row < rows and col < cols and grid[row][col] == 1:
                        grid[row][col] = 2
                        fresh -= 1
                        Q.appendleft((row,col))
            time += 1
        
        if fresh > 0:
            return -1
        return time
```

#### Time Complexity
`O(m*n)` since we just do traversals and aren't visiting nodes more than once

#### Space Complexity
`O(1)` since we only store some variables and the `directions` array

