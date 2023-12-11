---
tags: [medium, graph, "#union_find"]
---
### 684. Redundant Connections

Link: [here](https://leetcode.com/problems/redundant-connection/description/)

#### Problem
In this problem, a tree is an **undirected graph** that is connected and has no cycles.

You are given a graph that started as a tree with `n` nodes labeled from `1` to `n`, with one additional edge added. The added edge has two **different** vertices chosen from `1` to `n`, and was not an edge that already existed. The graph is represented as an array `edges` of length `n` where `edges[i] = [ai, bi]` indicates that there is an edge between nodes `ai` and `bi` in the graph.

Return _an edge that can be removed so that the resulting graph is a tree of_ `n` _nodes_. If there are multiple answers, return the answer that occurs last in the input.

#### Main Idea
- Use [[Union Find]] to determine what node can be removed
- Need to build methods

#### Approach
- We make a `parent` and `rank` array to hold what parent each element has (they all start as their own parent), and what their rank is (all start as 1 as there are no connections made yet)
- Make a `find()` method that is able to traverse the parent tree and find the parent node i.e. when the parent is equal to itself
- We can compress the parent path by using this line: 
	`parent[p] = parent[parent[p]]` 
	which just sets the parent equal to the grandparent; we don't need to worry about out OOB issues since worst case the element is it's own parent 
- Make the `union()` method, that finds the root parents of the nodes given and combines them if not already connected, connecting the graph with the smaller rank to the larger one
- Now call `union()` on all edges of the input, and if any of them are deemed redundant, return the edge
#### Edge Cases
- Invalid input

#### Solution
```python 
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        n = len(edges)
        parent = [i for i in range(n+1)]
        rank = [1 for _ in range(n+1)]

        def find(i):
            p = parent[i]
            while p != parent[p]:
                parent[p] = parent[parent[p]]
                p = parent[p]
            return p
        
        def union(i, j):
            pi = find(i)
            pj = find(j)

            if pi == pj:
                return False
            if rank[pi] > rank[pj]:
                parent[pj] = pi
                rank[pi] += 1
            else:
                parent[pi] = pj
                rank[pj] += 1
            return True
        
        for i, j in edges:
            if not union(i, j):
                return [i, j]
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)` 
