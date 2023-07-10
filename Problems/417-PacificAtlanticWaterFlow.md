---
tags:
- depth_first_search
- recursion
- matrix
- graph
- array
---

### 417. Pacific Atlantic Water Flow

Link: [here](https://leetcode.com/problems/pacific-atlantic-water-flow/)

#### Problem
There is an `m x n` rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix heights where `heights[r][c]` represents the height above sea level of the cell at coordinate `(r, c)`.

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a 2D list of grid coordinates result where `result[i] = [ri, ci]` denotes that rain water can flow from cell `(ri, ci)` to both the Pacific and Atlantic oceans.

#### Approach
**Wrong Approach**

There are a few approaches that seem like the right approach but are actually wrong. The first one would be the brute-force approach, where we run DFS at every cell and determine if it can reach both oceans. From this approach, it would seem to make sense to memoize the answers to the cells, that way we only need to compute the path from each cell once to each ocean. This seems like a good idea, but it runs into some problems inherent to DFS. 
DFS does not work when there are cyclic graphs where each cell needs to be evaluated. Imagine the case of a `3 x 3` map where the values are:
```
5 5 5
5 1 5
5 5 5
```
If we start at the top left (`(0,0)`), then we try to evaluate the path for that cell. This step will involve evaluating the paths of the neighbors as well, which then leads to the evaluation of the neighbors' neighbors' paths and etc. This process of attempting to evaluate neighbors continues until we reach the top left cell again and as we see, we are stuck in a loop. Now you may think that if we keep track of visited cells it will solve the issue, but it just leads us to not evaluate the some of the already visited nodes. So the takeaway from this incorrect approach, is that: If you want to recursively build a graph e.g. building a parent node by first evaluating the child nodes, you cannot have a cyclic graph. This approach works with trees because we know that we will eventually reach the end of the tree, while we cannot guarantee that for the nodes in this problem.

**Right Approach**
So if we can't memoize the matrix, how are we going to solve the problem? Well we can firstly split the problem in two. Instead of trying to find the cells that can reach both oceans, we can instead find the cells that can reach the Atlantic ocean or the Pacific ocean, and compare the sets.
We can also use existing information from the problem to make finding this information easier. We know that all the edge cells reach one of the oceans, so we can start there, and instead of DFSing to the ocean, we instead DFS from the cells connected to the ocean on the same level or "uphill". So we basically just DFS from each of the "costal" cells, and add every visited cell to the respective ocean set. At the end we can get the intersection of these sets to determine the final solution.

For this problem it helps to:
- Use existing information given from the problem description.
- Break the problem into smaller, easier to manage parts.
- Know that DFS doesn't work on cyclic graphs.

#### Solution
```python 
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        rows, cols = len(heights), len(heights[0])
        pac, atl = set(), set()

        def dfs(row, col, visit, prevHeight):
            if (row, col) in visit or row < 0 or col < 0 or rows <= row or cols <= col or heights[row][col] < prevHeight:
                return 
            
            visit.add((row, col))

            dfs(row-1,col,visit,heights[row][col])
            dfs(row+1,col,visit,heights[row][col])
            dfs(row,col-1,visit,heights[row][col])
            dfs(row,col+1,visit,heights[row][col])

        # Need to start with items we know can reach the ocean
        for c in range(cols):
            # North row bordering Pac
            dfs(0, c, pac, heights[0][c])
            # South row bordering Atl
            dfs(rows-1, c, atl, heights[rows-1][c])
        
        for r in range(rows):
            # West row bordering Pac
            dfs(r, 0, pac, heights[r][0])
            # East row bordering Atl
            dfs(r, cols-1, atl, heights[r][cols-1])
        
        return pac.intersection(atl)
```

#### Time Complexity
`O(m*n)` since we visit the nodes at most once. 

#### Space Complexity
`O(m*n)` since we store the node coordinates in sets.

