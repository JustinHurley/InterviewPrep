### 117. Populating Next Right Pointers in Each Interval II
Link: [here](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

#### Problem
Given a binary tree
```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

#### Approach
Given this is a problem that will involve tree traversals and we care about level, this is a great candidate for a Breadth-First-Search problem.

All we need to do is BFS on the graph and link the nodes on the same level together. We can know how many nodes are on each level by:
1. Check the size of the queue
2. Iterate through the first `len(Q)` elements, these are the elements on a given level.
3. Add each of the child nodes to the queue. 
4. Now that all the nodes of the level have been popped from the queue, and the children nodes have been added, we know that the current queue has all the nodes in the next level.

It is important to note that we have to check while iterating through the level that we haven't gone past the size of the level. Because we add in the child nodes as we process each node, we need to be sure that we don't overextend our iteration of the queue.

