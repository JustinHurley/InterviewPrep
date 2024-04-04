---
tags: [algorithm, union_find, graph]
---
# Kruskal's Algorithm
Kruskal's algorithm is used when we want to build a Minimum Spanning Tree (MST) of a weighted undirected graph. An MST is a tree that has no cycles, connects all the nodes in the graph together, and has the smallest total weight of all edges together.
## Use Cases
- Use when you want to create an MST of a weighted undirected graph
- Want to build an efficient graph that minimizes the weight of edges
- Want to create a graph with the "lowest cost" between edges
## General Idea
The algorithm is actually pretty easy to implement, given that it relies on greedy properties to build the MST.
- First gather all edges and sort them by weight in ascending order
- Remove all existing edges from nodes, so that no nodes is connected to any other nodes
- Add nodes to the graph from smallest to largest until there are `n - 1` edges present.
- If an edge that is going to be added would make a cycle, don't add it

## Code
```python
edges[i] = (start[i], end[i], weight[i])
edges.sort('ascending')

for edge in edges:
	if edge will not make a cycle:
		graph.connect(edge.start, edge.end)
return graph
```
## Time Complexity
`O(Elog(E))` where `E` is the number of edges present, since we must sort the edges
## Space Complexity 
`O(V + E)` since we store all the nodes and edges as a graph