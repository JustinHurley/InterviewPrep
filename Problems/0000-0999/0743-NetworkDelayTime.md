---
tags:
  - medium
  - graph
  - heap
  - breadth_first_search
  - graph_traversal
---
# 743. Network Delay Time
Link: [here](https://leetcode.com/problems/network-delay-time/description/)
## Problem
You are given a network of `n` nodes, labeled from `1` to `n`. You are also given `times`, a list of travel times as directed edges `times[i] = (ui, vi, wi)`, where `ui` is the source node, `vi` is the target node, and `wi` is the time it takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return _the **minimum** time it takes for all the_ `n` _nodes to receive the signal_. If it is impossible for all the `n` nodes to receive the signal, return `-1`.
## Main Idea
- Use Dijkstra's to find the distance from the origin node to all the other nodes
## Approach
- Use the input to build an adjacency list and a list to keep track of the shortest path to each of the nodes
- Follow [[Dijkstra'sAlgorithm|Dijkstra's Algorithm]] to determine the distance from the target node to the other 
- Return the max of the distances as that represents the node that it would take the longest to reach
## Edge Cases/Gotchas 
- The nodes are indexed starting at 1, so to use the indexes as keys, you need to have an empty 0-index and all arrays need to be size `n+1`. 
## Solution
```python 
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        # Build an adj list first
        adj_list = {}
        dists = [float('inf')] * (n+1)
        for i in range(n+1):
            adj_list[i] = []
        for time in times:
            start = time[0]
            adj_list[start].append((time[1], time[2])) # (dest, weight)
        # Now traverse with some BFS from the src node
        heap = [(0, k)] # Dist, node
        # Set the distance from the origin to 0
        dists[k] = 0
        while heap:
            # Pull out closest node
            curr_dist, curr_node = heapq.heappop(heap)
            # If we found a better path already, skip
            if curr_dist > dists[curr_node]:
                continue
            # Otherwise process the neighbors
            for neighbor_node, weight in adj_list[curr_node]:
                # Dist to neighbor from origin node
                neighbor_dist = curr_dist + weight
                # See if it's better than the curr best
                if neighbor_dist < dists[neighbor_node]:
                    dists[neighbor_node] = neighbor_dist
                    heapq.heappush(heap, (neighbor_dist, neighbor_node))
        if max(dists[1:]) == float('inf'):
            return -1
        return max(dists[1:])
```
## Time Complexity
$O(V+E*log(V))$ Dijkstra's takes `Elog(V)`, since we traverse every edge,  and we also need to take the max of all the max distances.
## Space Complexity
$O(V + E)$ for the adjacency list which has every edge and node
