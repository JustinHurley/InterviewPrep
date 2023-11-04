---
tags: [tree, breadth_first_search, linked_list, graph_traversal, queue]
---

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

#####Note
In Python, queues are initialized by writing `queue = collections.deque([root])`. When popping from the queue, it is important to use `popleft()` instead of `pop(0)`. This is because `popleft()` roughly runs in `O(1)` time while the other methods takes `O(n)` time.
#### Solution
```python 
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        # First do a null check
        if not root: 
            return root
        # Initialize the queue, the starting node in the queue is the root 
        queue = collections.deque([root])
        # Now we start processing the queue
        while queue:
            n = len(queue)
            # We need to keep track of the current size as that is the current level
            for i in range(n):
                # First we pop a node from the queue
                node = queue.popleft()
                
                # If we are less than size, AKA on the same level, we want to append the popped node
                # To the node on the top of the stack (next node on this level)
                if i < n - 1:
                    node.next = queue[0]
                
                # Then add children of current node to the queue
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        return root
```