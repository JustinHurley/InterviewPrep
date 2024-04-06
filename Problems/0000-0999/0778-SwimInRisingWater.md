---
tags: [hard, graph_traversal, heap, breadth_first_search, matrix]
---
# 778. Swim in Rising Water
Link: [here](https://leetcode.com/problems/swim-in-rising-water/description/)
## Problem
You are given an `n x n` integer matrix `grid` where each value `grid[i][j]` represents the elevation at that point `(i, j)`.

The rain starts to fall. At time `t`, the depth of the water everywhere is `t`. You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most `t`. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return _the least time until you can reach the bottom right square_ `(n - 1, n - 1)` _if you start at the top left square_ `(0, 0)`.
## Main Idea
- Use modified Dijkstra's to find the least time it takes to get to each point
- Instead of adding the weight to the existing weight, we take the max of the current time and the height of the cell
- Brute forcing by checking all times until valid works but is a bad solution
- We can treat the water level at a given time as the "weight" of getting to that square, and then run Dijkstra's 
## Approach
- Create min-heap and add starting node, using the height of the starting position as the starting time
- Make a set to track visited nodes
- Now go through the heap and pop while there are still nodes to process
- If we visited the node already, skip 
- If we are at the destination node, then we can stop and return the time at that point
- Otherwise we should look through adjacent nodes to see if any of them can be added
- When we add a node, for the weight we take the max of the current time and the height of that cell, as whichever one is larger will limit the person's ability to swim to that cell
- Continue until heap is empty and return `t`
## Edge Cases/Gotchas 
- Handle OOB cases
- You can save time by starting `t = grid[0][0]` instead of `t = 0`
- You need to treat time as the weight of the graph in this problem
## Solution
```python 
class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:
        t = grid[0][0]
        heap = [(t, 0, 0)]
        seen = set()
        adjacent = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        n = len(grid)

        while heap:
            t, x, y = heapq.heappop(heap)
            # Make sure not visited
            if (x, y) in seen:
                continue
            seen.add((x, y))
            # If at the corner, we can stop
            if x == n-1 and y == n-1:
                return t
            # Otherwise look throgh valid neighbors
            for dx, dy in adjacent:
                new_x, new_y = x+dx, y+dy
                # If in bounds add
                if new_x >= 0 and new_x < n and new_y >= 0 and new_y < n:
                    # If not already visited
                    if (new_x, new_y) not in seen:
                        # The max time can be considered the weight
                        heapq.heappush(heap, (max(grid[new_x][new_y], t), new_x, new_y))
        return t```
## Time Complexity
$O(N^2*log(N))$ since we visit each node once, and each node has worst case 4 neighbors, and for each node we do a heap-push which is a `log(N)` operation
## Space Complexity
$O(N)$ since worst-case every node is stored in the min-heap at the same time 
