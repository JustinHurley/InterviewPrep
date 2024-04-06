---
tags:
  - medium
  - graph
  - heap
  - minimum_spanning_tree
---
# 1584. Min Cost to Connect all Points
Link: [here](https://leetcode.com/problems/min-cost-to-connect-all-points/description/)
## Problem
You are given an array `points` representing integer coordinates of some points on a 2D-plane, where `points[i] = [xi, yi]`.
The cost of connecting two points `[xi, yi]` and `[xj, yj]` is the **manhattan distance** between them: `|xi - xj| + |yi - yj|`, where `|val|` denotes the absolute value of `val`.
Return _the minimum cost to make all points connected._ All points are connected if there is **exactly one** simple path between any two points.
## Main Idea
- Need to build an MST
- Use Prim's 
- Consider every edge in the graph since every edge is a potential part of the MST
- Use a heap to keep track of the smallest weight edges
## Approach
- Make a heap and a set to track which nodes have been added so far
- Have a while loop run while there are less than `n` edges added to the graph
- For each edge:
	- Make sure the node hasn't already been added to the graph, if so skip
	- Add node to graph, update total weight, and also increment the total edge counter
	- Also add to the already added nodes set
	- Now go through all nodes that haven't been already added to the graph, and add the new edges from the node just added to all un-added nodes, including the distance to the min-heap
- Return the answer
## Edge Cases/Gotchas 
- N/A
## Solution
```python 
class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        n = len(points)
        # First build list of edges
        heap = [(0, 0)]
        # See which nodes are included
        added = set()

        ans = 0
        edges = 0
        while edges < n:
            w, node = heapq.heappop(heap)
            # If node already included in graph skip
            if node in added:
                continue
            # Now add node to graph, and update answer
            added.add(node)
            ans += w
            edges += 1
            # Add neighbors
            for i in range(n):
                if i not in added:
                    weight = self.distance(points[node], points[i])
                    heapq.heappush(heap, (weight, i))
        return ans

    def distance(self, a, b):
        return abs(a[0] - b[0]) + abs(a[1] - b[1])
```
## Time Complexity
`O(Elog(V))`
## Space Complexity
`O(V + E)`

