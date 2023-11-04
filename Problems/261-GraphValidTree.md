---
tags: [graph, tree, depth_first_search]
---

### 261. Graph Valid Tree

Link: [here](https://leetcode.com/problems/graph-valid-tree/description/)

#### Problem
You have a graph of `n` nodes labeled from `0` to `n - 1`. You are given an integer `n` and a list of `edges` where `edges[i] = [ai, bi]` indicates that there is an undirected edge between nodes `ai` and `bi` in the graph.

Return `true` if the edges of the given graph make up a valid tree, and `false` otherwise.

#### Approach
The first thing to think about when approaching this problem is to determine the criteria of what makes a graph a valid tree, since all [[trees]] are [[graphs]] but not all [[graphs]] are [[trees]]. For a graph to be a valid tree, it must satisfy the following criteria:
1. The graph must have 0 cycles.
2. The graph must be connected (meaning we can get to any node from any other node in the graph). 
Now that the criteria has been determined, we can now start to solve the problem. The first thing to do is to build an adjacency list using the supplied edges. This will make it easier and faster to traverse and access the graph. We do this by using a `dict`, where the key corresponds to the node number and the value corresponds to the list of neighbors. This will be filled with the `edges` input.
The next thing to do is to validate the graph. This can be done using DFS to traverse the graph and visit the nodes along the way.
For DFS, the base case will be if the current node we are looking at is in the `visited` set, because if it is, this implies that there is a cycle in the graph. If the current node has not been visited yet, we will iterate through the child nodes, and call DFS on them, if any of them return false, we should also return false, since that would imply one of the child nodes encountered a cycle.
The last thing to keep in mind is that the graph is undirected, unlike `TreeNodes`, this means there is no distinction between going from node `A -> B` and `B -> A`. This can lead to incorrect cycle detection with respect to treating the graph as a tree, as a parent node would be visited, then the child node would be visited, but the parent node would then be re-visited, as it is a child of the child node. So really, all the nodes are just adjacent to one another and there is no parent-child hierarchy. So to solve for this, we simply have another parameter in the DFS method we made that tracks the previous node to the current node we are visiting, that way when we are iterating through child nodes, we can check the previous node to ensure that we don't end up going back and forth between the current node and the node we just visited.
Finally, we will call the DFS method on the `0` node (chosen arbitrarily, technically you could choose any node, but choosing 0 makes the most sense since you don't have to check against `n`). This will tell us if the graph has a cycle, and then to check to see if the graph is connected, we can just look at the size of the `visited` set, and if it equals `n`, it means we were able to visit all the nodes from node `0`. So if both of these things are true, then we have a valid tree.

#### Solution
```python 
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        if n <= 1:
            return True
        
        adj = {i:[] for i in range(n)}

        for edge in edges:
            a = edge[0]
            b = edge[1]
            adj[a].append(b)
            adj[b].append(a)

        visit = set()

        def dfs(node, prev):
            # Base case
            if node in visit:
                return False

            visit.add(node)
            for child in adj[node]:
              if child == prev:
                continue
              if not dfs(child, node):
                  return False
            
            return True
        
        return dfs(0, -1) and len(visit) == n
```

#### Time Complexity
We have to visit every node in the graph, and process every edge, so that gives us a time complexity of `O(V + E)`.

#### Space Complexity
We end up making an adjacency list with a KVP for each node and a list for every edge. Because we need to get the forward and reverse connection, we technically use `O(2(V+E))` space which just simplifies to `O(V+E)`.