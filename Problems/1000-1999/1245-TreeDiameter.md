---
tags: [medium, tree, depth_first_search]
---
# 1245. Tree Diameter
Link: [here](https://leetcode.com/problems/tree-diameter/description/)
## Problem
The **diameter** of a tree is **the number of edges** in the longest path in that tree.
There is an undirected tree of `n` nodes labeled from `0` to `n - 1`. You are given a 2D array `edges` where `edges.length == n - 1` and `edges[i] = [ai, bi]` indicates that there is an undirected edge between nodes `ai` and `bi` in the tree.
Return _the **diameter** of the tree_.
## Main Idea
For each node, find the longest 2 paths from that node, and then see what the diameter of that path would be. After finding that distance, return the longest path for recursive calls in the stack

You could also solve using [[BreadthFirstSearch|BFS]] where you start from the root to find the furthest away node (the last node that you process in the queue), and then do a search to find the furthest node from the furthest node. Basically you are finding the two furthest nodes from each other. 
## Approach
- First convert the input into an adjacency list 
- Now we want to [[DFS]] on that tree, so make a DFS method
	- Since we are using an adjacency list, we don't need to check for `None`
	- Visit the current node
	- For each child of that node:
		- If the node is not already visited, make a recursive call to get the max depth
	- Track the longest and second longest path from that node
	- Using the longest and second longest values, compare that to the global longest path
	- Once the comparison has been made, we can return the longest path for that specific node, to be used in other recursive calls
- We call the DFS method for the `0` node. It's important to note that if this graph wasn't connected, this approach would not visit all the nodes and we would need a different method of solving
- Once DFS has traversed the tree, return the computed global best
## Edge Cases/Gotchas 
- Be aware that the graph is undirected, meaning that you'll need to keep track of visited nodes although it's a tree
- The tree can have 0 or 1 branches so we need to be cognizant of that (does not affect the solution below)
## Solution
```python 
class Solution:
    def __init__(self):
        self.best = 0

    def treeDiameter(self, edges: List[List[int]]) -> int:
        # Make undirected graph
        graph = [set() for i in range(len(edges)+1)]
        for edge in edges:
            u, v = edge
            graph[u].add(v)
            graph[v].add(u)

        # Try to get the longest path
        visited = {}
        def dfs(node):
            first = second = 0
            # Visit the node
            visited[node] = True
            # For each child
            for child in graph[node]:
                curr_length = 0
                # If node not yet processed (since undirected)
                if child not in visited:
                    # Get the max length
                    curr_length = dfs(child) + 1
                    # Update if necessary 
                    if curr_length > first:
                        second = first
                        first = curr_length
                    elif curr_length > second:
                        second = curr_length
            # Now compare curr best to global best
            self.best = max(self.best, first+second)
            # Now update memo table and return computed ans
            return first
        dfs(0) 
        return self.best```
## Time Complexity
$O(n)$ visit each node once
## Space Complexity
$O(n)$ for the adjacency list 
