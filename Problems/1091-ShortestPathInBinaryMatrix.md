---
tags: [matrix, breadth_first_search, queue, set, tuple]
---

### 1091. Shortest Path in Binary Matrix
Link: [here](https://leetcode.com/problems/shortest-path-in-binary-matrix/)

#### Problem
Given an n x n binary matrix grid, return the length of the shortest clear path in the matrix. If there is no clear path, return -1.

A clear path in a binary matrix is a path from the top-left cell (i.e., (0, 0)) to the bottom-right cell (i.e., (n - 1, n - 1)) such that:
- All the visited cells of the path are 0.
- All the adjacent cells of the path are 8-directionally connected (i.e., they are different and they share an edge or a corner).
  
The length of a clear path is the number of visited cells of this path.

#### Approach
Whenever you see shortest path problem, 99% of the time it is a BFS problem. This problem requires the traversal of the matrix (like a graph). You also need to keep track of the distance of each point from the origin. 
The general approach is to do breadth-first-search, and using tuples, keep track of the coordinates and distance from the starting point. 
It is also important to keep a set of visited nodes, that way you can quickly tell if a node has been visited or not in `O(1)` time. 
So basically, while there are still nodes in the queue, dequeue, check to see if we are at the end, if we aren't at the end, we should find the non-visited neighbors of the node and add them to the queue, incrementing the distance value for each of the neighbors by 1, relative to the current node.

##### Notes
There are a lot of Python-specific things to know that can help make this problem much easier.
- You can store coordinates in a tuple, and instead of indexing them, simply extract them into a variable like:
`x-coordinate, y-coordinate = coordinates`
- You can also extract values from a tuple using the `*` symbol. So if we wanted to return coordinates, but also add a z-coordinate, we could do that like this: `return(*coordinates, z-coordinate)`, where `*coordinates` will populate the returned tuple with the x and y coordinate. 
- set can be initialized by making a `{}` with something in it, if there is nothing inside the curly braces, you will end up with a dict instead. To get an empty set use `set()` or `{*()}`.
- `yield` returns a generator instead of a value. What this means is that instead of returning a list of things, it returns a generator that only provides the values when called, and cannot be iterated over again. When you call a function that uses `yield` the function does not run, it returns the generator object which is used to run the function. `yield` can be used as a way to return a list without actually needing to build said list, as `yield` doesn't terminate the function.

#### Solution
```python 
class Solution:
    def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        rows = len(grid)
        cols = len(grid[0])
        directions = [(-1,-1),(-1,0),(-1,1),(0,-1),(0,1),(1,-1),(1,0),(1,1)]
        
        #Edge case where start is 1
        if grid[0][0] == 1 or grid[rows-1][cols-1] == 1:
            return -1 
        
        # Method to get neighbors
        def getNeighbors(row, col):
            # For each direction
            for diffRow, diffCol in directions:
                # If the item is in bounds and not a 1
                newRow = row+diffRow
                newCol = col+diffCol
                if 0 <= newRow and newRow < rows and 0 <= newCol and newCol < cols and grid[newRow][newCol] != 1:
                    yield (newRow, newCol)
        
        # Make our queue, starting at top left
        Q = collections.deque([(0,0,1)])
        visited = {(0,0)}
        # While our queue isn't empty
        while Q:
            # Take off the top node
            row, col, dist = Q.popleft()
            # Check if we're at the end
            if row == rows-1 and col == cols-1:
                return dist
            # Now we need to get the neighbors of the node
            for neighbor in getNeighbors(row, col):
                if neighbor in visited:
                    continue
                visited.add(neighbor)
                Q.append((*neighbor, dist+1))
        return -1
```
