---
tags:
  - medium
  - graph
  - breadth_first_search
  - priority_queue
---
# 787. Cheapest Flights with K Stops
Link: [here](https://leetcode.com/problems/cheapest-flights-within-k-stops/description/)
## Problem
There are `n` cities connected by some number of flights. You are given an array `flights` where `flights[i] = [fromi, toi, pricei]` indicates that there is a flight from city `fromi` to city `toi` with cost `pricei`.

You are also given three integers `src`, `dst`, and `k`, return _**the cheapest price** from_ `src` _to_ `dst` _with at most_ `k` _stops._ If there is no such route, return `-1`.
## Main Idea
- Use [[Dijkstra'sAlgorithm|Dijkstra's Algorithm]] to find the minimum path from the source to the destination
- Also track the overall distance in nodes from `src` incase the cheapest path is too long
## Approach
- First take the graph and build an adjacency list 
- Make a heap starting with the `src` node, using the price as the key
- Also track distance from the source node
- For each element in the heap
	- Pop the closest node
	- Check if it's the `dst` node and valid, since Dijkstra's is greedy, we know that the first time the destination node is reached, it will be the cheapest route (but potentially too many legs)
	- If we're not at the answer node, we go through the neighbors of the current node
	- We only go through the neighbors in the case that we haven't visited the current node yet (means on the cheapest path to that node) or if the distance of the current node is less than the saved distance for that node (meaning we already found the cheapest path, but still want to check this one in the case that the cheapest path has a distance greater than `k`).
## Edge Cases/Gotchas 
- Dijkstra's will find the cheapest path, but not necessarily the shortest path when given the option
- Graph is valid but we could potentially have to deal with invalid flights, negative weights, invalid input
## Solution
```python 
class Solution:
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
        # Build the graph
        graph = {i:[] for i in range(n)}
        for flight in flights:
            # src, dst, price
            s, d, w = flight[0], flight[1], flight[2]
            graph[s].append((d, w))
        # Do Dijkstras
        heap = [(0, src, 0)]
        # Track distances to each node
        distances = {}
        while heap:
            # Get closest node
            curr = heapq.heappop(heap)
            curr_w, curr_node, curr_dist = curr[0], curr[1], curr[2]
            # If the node is the dest and valid return the weight
            if curr_node == dst and curr_dist-1 <= k:
                return curr_w
            # Otherwise, see if node has been visited or we already found a shorter path
            if curr_node not in distances or distances[curr_node] > curr_dist:
                distances[curr_node] = curr_dist
                for nei in graph[curr_node]:
                    heapq.heappush(heap, (curr_w + nei[1], nei[0], curr_dist+1))
        # If we got here we never reached the dest
        return -1
```
## Time Complexity
$O(N + E * k * log(E*k))$ where `N` is number of cities and `E` is number of flights (node and edge). 
## Space Complexity
$O(N + E * k)$ as the priority queue can only have `E*k` elements since we look at all the edges, but we will either find the node or terminate Dijkstra's by the time we iterate `k` times.
