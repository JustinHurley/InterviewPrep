---
tags:
  - hard
  - graph
  - tree
  - graph_traversal
  - depth_first_search
---
# 2246. Longest Path With Different Adjacent Nodes
Link: [here](https://leetcode.com/problems/longest-path-with-different-adjacent-characters/description/)
## Problem
You are given a **tree** (i.e. a connected, undirected graph that has no cycles) **rooted** at node `0` consisting of `n` nodes numbered from `0` to `n - 1`. The tree is represented by a **0-indexed** array `parent` of size `n`, where `parent[i]` is the parent of node `i`. Since node `0` is the root, `parent[0] == -1`.

You are also given a string `s` of length `n`, where `s[i]` is the character assigned to node `i`.

Return _the length of the **longest path** in the tree such that no pair of **adjacent** nodes on the path have the same character assigned to them._
## Main Idea
- This is the same as finding the longest path in a tree problem, with a small twist to check for adjacency
- At each node we want to get the longest and second longest subtrees to then try and make the longest path, but we should only do this if the root node and the child node have different adjacent characters
- If the characters are the same, we can't make a path but want to keep looking in that subtree
- This problem is hard because you have to build a graph from the input, and also handle not keeping adjacent-char nodes
## Approach
- First since this problem is harder when working with parents, convert the input to an adjacency list
- The root has its parent as `-1` so make sure to remove that from the list
- Now we make our DFS method to traverse the graph and find the longest path
	- Init our first and second variables that will be used to hold the longest and second longest valid subpaths
	- For each child in our graph, we will check if the char is equal between current and child, if it isn't equal we can keep the value and use it to continue to build our longest path
	- If child and parent match, we just make a DFS call starting at the child node, to see if it can build a larger subtree from the given nodes
	- Once we go through child nodes, see if we can make a better global best 
	- Then we return the longest path (with no branching) for that given node to be used in higher-level recursive calls
- To actually get the answer, we call `dfs(0)` and then return the `self.ans` value
## Edge Cases/Gotchas 
- The `root` node has a parent of `-1` which we don't end up handling but should be aware of
- When a child node has the same char as the current node, we can't just stop DFSing on that subtree, since there could be a larger total path starting with the child node
## Solution
```python 
class Solution:
    def __init__(self):
        self.ans = 0

    def longestPath(self, parent: List[int], s: str) -> int:
        graph = defaultdict(list)
        # Convert to normal tree
        for i, val in enumerate(parent):
            graph[val].append(i)
        # Now DFS on tree
        def dfs(node):
            # Go through child nodes
            first, second = 0, 0
            for child in graph[node]:
                # Only check subarrs that will be valid
                child_len = dfs(child)
                if s[node] != s[child]:
                    if child_len > first:
                        second = first
                        first = child_len
                    elif child_len > second:
                        second = child_len
            # Update potential best
            self.ans = max(self.ans, first+second+1)
            return max(first,second)+1
        dfs(0)
        return self.ans
```
## Time Complexity
$O(n)$ to build the graph from the input, we don't revisit nodes so no issue from traversal
## Space Complexity
$O(n)$ since we hold each edge from parents
