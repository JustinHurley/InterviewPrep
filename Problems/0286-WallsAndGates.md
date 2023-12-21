---
tags: [graph, breadth_first_search, medium, matrix]
---
### 286. Walls and Gates

Link: [here](https://leetcode.com/problems/walls-and-gates/description/)

#### Problem
You are given an `m x n` grid `rooms` initialized with these three possible values.

- `-1` A wall or an obstacle.
- `0` A gate.
- `INF` Infinity means an empty room. We use the value `231 - 1 = 2147483647` to represent `INF` as you may assume that the distance to a gate is less than `2147483647`.

Fill each empty room with the distance to _its nearest gate_. If it is impossible to reach a gate, it should be filled with `INF`.

#### Main Idea
- [[BreadthFirstSearch|BFS]] allows us to track the distance from all of the gate nodes at the same time
- Instead of finding the distance from each room -> gate, it's much easier to find the distance from gate -> each room

#### Approach
- Set up the problem like any other BFS problem (queue, get lengths, etc.)
- To seed the queue, we should add all gate nodes
- Now we want to start BFS, it's important that we process the queue in bulk so that we visit all nodes of distance 2 before we visit all nodes of distance 3

#### Edge Cases
- Graph is empty
- Invalid values are present 

#### Solution
```python 
class Solution:
    def wallsAndGates(self, rooms: List[List[int]]) -> None:
        """
        Do not return anything, modify rooms in-place instead.
        """
        rows, cols = len(rooms), len(rooms[0])
        directions = [[-1, 0], [1, 0], [0, -1], [0, 1]]

        Q = deque()
        seen = set()
        dist = 0

        # Add elts to queue
        for r in range(rows):
            for c in range(cols):
                if rooms[r][c] == 0:
                    Q.appendleft((r,c))
                    seen.add((r,c))

        # Method to determine if neighbor should be added and visits
        def process(row, col):
            if row >= 0 and col >= 0 and row < rows and col < cols and (row, col) not in seen and rooms[row][col] != -1:
                Q.appendleft((row,col))
            seen.add((row,col))
            return 

        # Do BFS now
        while Q:
            for _ in range(len(Q)):
                r, c = Q.pop()
                rooms[r][c] = min(rooms[r][c], dist)
                # Now try and add neighbors
                for dr, dc in directions:
                    process(r+dr, c+dc)
            dist += 1
        return
```

#### Time Complexity
`O(m*n)` since we visit each node max once

#### Space Complexity
`O(1)`since everything is done in-place

