---
tags: [algorithm, greedy]
---
# Prim's Algorithm
Prim's algorithm is used to find the Minimum Spanning Tree of a connected, undirected graph. Similar to Kruskal's algorithm, this uses a greedy property to build the graph from the ground up. 
## Use Cases
- Use when you want to create an MST of a weighted undirected graph
- Want to build an efficient graph that minimizes the weight of edges
- Want to create a graph with the "lowest cost" between edges
## General Idea
- Choose a random starting vertex.
- Make a [[Heap|min-heap]] to store potential edges
- Add edges connected to the starting vertex
- While there are still edges in the min-heap:
	- Remove the edge with the minimum weight
	- If it doesn't make a cycle, add it to the graph
	- Add all un-added neighbor nodes to the min-heap
- Continue until every vertex has been added to the graph
## Code
```python
heap = [(0, 0)] # (weight, index)
added = {}

total_edges = 0
while total_edges < len(points):
	weight, node = heappq.heappop(heap)
	# Make sure node isn't added
	if node in added:
		continue
	# Add node to graph and update
	added.add(node)
	total_edges += 1
	graph.add(node, weight) # Connecting to graph
	# Add neighbors to heap
	for neighbor in node.neighbors:
		heappq.heappush(heap, neighbor)
```
## Time Complexity
`O(Elog(V))` where `E` is edges and `V` is vertexes when using a min-heap.
## Space Complexity 
`O(V + E)`