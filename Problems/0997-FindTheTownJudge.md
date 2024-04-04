---
tags:
  - easy
  - dictionary
  - graph
---
# 997. Title
Link: [here](https://leetcode.com/problems/find-the-town-judge/description/)
## Problem
In a town, there are `n` people labeled from `1` to `n`. There is a rumor that one of these people is secretly the town judge.
If the town judge exists, then:
1. The town judge trusts nobody.
2. Everybody (except for the town judge) trusts the town judge.
3. There is exactly one person that satisfies properties **1** and **2**.
You are given an array `trust` where `trust[i] = [ai, bi]` representing that the person labeled `ai` trusts the person labeled `bi`. If a trust relationship does not exist in `trust` array, then such a trust relationship does not exist.
Return _the label of the town judge if the town judge exists and can be identified, or return_ `-1` _otherwise_.
## Main Idea
- Find the node that is trusted by everyone but trusts nobody (directed graph)
## Approach
- I made 2 adjacency lists but you could also just use a counter to get the count of how many neighbors trusts a given neighbor, and how many neighbors a given neighbor trusts
- Find the node trusted by everyone and that trusts nobody
## Edge Cases/Gotchas 
- Not in problem but there is the issue of data validity, duplicate edges, nodes that trust themselves, etc. 
## Solution
```python 
class Solution:
    def findJudge(self, n: int, trust: List[List[int]]) -> int:
        # Add nodes to graph
        graph = {i:[] for i in range(1,n+1)}
        reverse_graph = {i:[] for i in range(1,n+1)}
        # Add edges
        for t in trust:
            node = t[0]
            edge = t[1]
            graph[node].append(edge)
            reverse_graph[edge].append(node)
        # Go through graph
        ans = -1
        for key in graph.keys():
            # If no neighbors and all other nodes point to node
            if (key not in graph or not graph[key]) and len(reverse_graph[key]) == n-1:
                ans = key
        return ans
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
