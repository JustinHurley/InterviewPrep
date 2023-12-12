---
tags:
  - algorithm
  - union_find
aliases:
  - Union Find
  - union find
---
### Union Find
Union Find is an algorithm that should be considered when dealing with problems that ask about connectivity, or edge redundancy. Union Find basically builds the graph from the ground up, adding edges to a given set of nodes, and combining them until there is one connected graph as a result. Union Find can be implemented to detect cycles, and tracking flow through a network.
Union find is also great at building a minimally-connected tree, i.e. a tree with no cycles, where all edges are necessary in keeping the graph connected.

#### General Idea
- Go through a set of nodes and edges
- Keep track of every node's parent (all nodes are their own parent at the start)
- Keep track of every node's rank, which is the size of the graph that a given node is connected to (all nodes are set to a rank of 1 at the beginning)
- When we find, we find the root parent of a given node
- When we union, we add the smaller graph to the larger graph, and update the arrays keeping track of parents and rank

#### Code
```python
	n = len(edges)
	parent = [i for i in range(n+1)]
	rank = [1 for _ in range(n+1)]

	def find(i):
		p = parent[i]
		while p != parent[p]:
			parent[p] = parent[parent[p]] # Optimization 
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
		union(i, j)
```

#### Time Complexity
`O(n)` 

#### Space Complexity 
`O(n)` since we use arrays to track the parents and rank of each node