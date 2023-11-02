---
aliases:
- breadth-first search
- BFS
- bfs
tags:
- algorithm
- graph
- stack
- breadth_first_search
- tree
---

## Breadth-First Search

#### General Idea
Breadth-first search (BFS) is a graph traversal algorithm, that traverses nodes by depth level, starting from the root and working its way out. This happens inherently because of the data structure used to traverse the graph, a queue. By adding children into the queue before iterating to the child nodes, it ensures that after the root node is visited, the left child is visited, and then the right (assuming you choose to add the left child to the queue first). The next node to visit is taken by polling the queue and repeating the process of adding children to the queue, and then visiting the next node in the queue.

#### When to Use
BFS should be used when you need to traverse through a graph radiating from the center. This is useful in scenarios where you want to look near the root as opposed to searching the whole tree (where you might want to use something like [[DepthFirstSearch]]). Particularly, it's useful in shortest path problems, problems where you want to search by relation (like with social networks). In general, when you are dealing with a problem where the tree/graph is relatively broad, and the solution is not near the bottom of the tree. 

#### Code
```python
BFS(root: Node):
	# Make a queue and add the root 
	Q = [root]
	# Make a set to track the visited nodes
	visited = set()

	# While there are nodes in the queue
	# i.e. while there are nodes to visit
	while Q:
		# Poll the queue
		curr = Q.pop(0)
		# Visit the node
		visited.add(curr)
		# Add the nodes children to the queue
		for child in curr.children:
			if child not in visited:
				Q.append(child)

```

#### Time Complexity
`O(n)` where `n` is the number of nodes in the graph

#### Space Complexity 
`O(n)` since you could have a graph of depth 1, with a `root` node and `n-1` children