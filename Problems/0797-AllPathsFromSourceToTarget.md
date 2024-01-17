---
tags:
  - graph_traversal
  - depth_first_search
  - backtracking
  - queue
  - medium
---

### 797. All Paths from Source to Target
Link: [here](https://leetcode.com/problems/all-paths-from-source-to-target/)

#### Problem
Given a directed acyclic graph (DAG) of n nodes labeled from 0 to `n - 1`, find all possible paths from node 0 to node `n - 1` and return them in any order.

The graph is given as follows: `graph[i]` is a list of all nodes you can visit from node i (i.e., there is a directed edge from node i to node `graph[i][j]`).

#### Approach
This is a [[Topics/Backtracking|backtracking]] problem, which is very similar to DFS. The only difference is, with [[Topics/Backtracking|backtracking]], we keep track of a solution through the recursive cases and discard the built solution if it doesn't fit the criteria.
The approach to the problem involves setting up DFS, and then traversing the graph, while we pass an array through the recursive calls to keep track of the path we are on. 
The structure of the DFS algorithm has the base case, where the target node of the graph is reached, so we add the path to the answer and then stop, otherwise we keep iterating over the edges of the node and running recursively for each edge. 
It's important to only add the edge we are currently on to the path, and then to remove it before we process the next connected node.

#### Solution
```python 
class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        target = len(graph) - 1
        ans = []
        
        # Backtrack recursive function
        def backtrack(currNode, path):
            # If we hit the target, add the path to answers and return
            if currNode == target:
                ans.append(list(path))
                return
            
            # Look through neighbor nodes
            for nextNode in graph[currNode]:
                # Append the next node to the path
                path.append(nextNode)
                # Begin backtracking with that node
                backtrack(nextNode, path)
                # Make sure to remove the node just added for future iterations
                path.pop()
        
        # We use a queue to allow for the pop to take the node just added off the list
        backtrack(0, deque([0]))
        return ans
```