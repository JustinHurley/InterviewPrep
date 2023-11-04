---
tags: [dictionary, depth_first_search, graph, recursion]
---

### 133. Clone Graph

Link: [here](https://leetcode.com/problems/clone-graph/description/)

#### Problem
Given a reference of a node in a connected undirected graph.

Return a deep copy (clone) of the graph.

Each node in the graph contains a value (`int`) and a list (`List[Node]`) of its neighbors.
```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

#### Approach
The basic approach is to use a hash table of the visited nodes, and set the values to the copies of the nodes. That allows you to traverse the graph and kind of use memoization to store the values of the nodes. The DFS workflow is like this:
1. Base case: check to see if this node already exists in the hash table, which means it is already visited, and we can just return the value of the node copy.
2. If the node has not been visited yet, we want to start to make a copy, and add it to the hash table. We will leave neighbors blank for now since we will need to make deep copies of those values as well.
3. Now we need to traverse the neighbors. This is done by iterating through the list of neighbors, and for each node, calling DFS on it, where we append the result of that DFS call to the current copy's neighbor list. Once we have iterated through the loop, and all the neighbors have been visited/copies, we can just return the copied node from the DFS method.

After running the DFS method, we just return the hash table entry for the original passed in node.

Edge Case: We need to handle the case where the original node is `None`.

#### Solution
```python 
class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        # Edge case
        if not node:
            return 

        nodeMap = {}

        def dfs(node):
            # If we already visited this node, just return the copy we made earlier
            if node in nodeMap:
                return nodeMap[node]

            # Now we make copy and add to map
            copy = Node(node.val)
            nodeMap[node] = copy

            # Now we need to look through neighbors and solve
            for nei in node.neighbors:
                copy.neighbors.append(dfs(nei))

            return copy
        
        dfs(node)
        return nodeMap[node]
```

#### Time Complexity
- `O(E+V)` where `E` is the number of edges in the graph, and `V` is the number of nodes in the graph.

#### Space Complexity
- `O(E+V)` since we are making a deep copy.
