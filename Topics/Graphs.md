# Graphs
Graphs are a data structure consisting of a series of **nodes** connected to each other by **edges**. These nodes can have values and the edges can be a combination of weighted and/or directed. Trees are considered a subset of graphs.
# Minimum Spanning Trees (MST)
A Minimum Spanning Tree (MST) of a connected, undirected graph is a subset of the edges that forms a tree connecting all the vertices of the graph while minimizing the total weight or cost of the tree. Here are some key points about MSTs:
1. **Connectedness**: An MST must connect all the vertices of the graph. This means that there is a path between every pair of vertices in the tree.
2. **Acyclic**: An MST cannot contain any cycles. Adding an edge to an MST would create a cycle, violating the acyclic property.
3. **Minimum Weight**: Among all possible spanning trees of the graph, an MST has the minimum total weight. The weight of the tree is the sum of the weights of its edges.

```dataview
TABLE
FROM (#graph) OR (#tree_traversal) AND !(#algorithm) AND !(#category)
```
