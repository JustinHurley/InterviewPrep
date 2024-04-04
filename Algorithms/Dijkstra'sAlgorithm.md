---
tags:
  - algorithm
aliases:
  - Dijkstra's Algorithm
  - dijkstra's
  - Dijkstra
---
# Dijkstra's Algorithm

## Use Cases
- Used to find the shortest path between two nodes in a weighted graph
## General Idea
1. **Initialization**:
    - Assign a tentative distance value to every node. Set the initial node's distance to 0 and all other nodes' distances to infinity.
    - Create a priority queue (usually implemented with a min-heap) to keep track of the nodes to visit, with the initial node at the front.
2. **Loop**:
    - While there are still nodes to visit in the heap:
        - Remove the node with the smallest tentative distance from the priority queue (pop from the heap). This node is now visited.
        - For each neighbor of the current node:
            - Calculate the current distance from the initial node through the current node to this neighbor (current weight from origin + weight to neighbor from current node).
            - If this tentative distance is smaller than the current known distance to the neighbor, update the neighbor's distance.
            - Update the heap accordingly.
3. **Termination**:
    - When all nodes have been visited or when the smallest current distance among the unvisited nodes is infinity, the algorithm terminates.
4. **Result**:
    - After termination, the shortest path from the initial node to each node in the graph is known.
## Code
```python
def dijkstra(graph, start):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    heap = [(0, start)]

    while heap:
		# Get current node from heap
        current_distance, current_node = heapq.heappop(heap)
		# If we found a worse path stop
        if current_distance > distances[current_node]:
            continue
		# Otherwise go through all neighbors and update dists
        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(heap, (distance, neighbor))

    return distances```
## Time Complexity
$O(E + V*log(V))$
## Space Complexity 
$O(V)$ 
