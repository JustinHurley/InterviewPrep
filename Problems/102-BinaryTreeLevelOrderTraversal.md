---
tags:
- breadth_first_search
- tree
- binary_tree
---

### 102. Binary Tree Level Order Traversal

Link: [here](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)

#### Problem
Given the `root` of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

#### Approach
BFS already goes level by level implicitly, as the children of each node are added to the queue after all the current nodes that are on the same level are traversed/processed.
This means, that we can just implement BFS with some extra work to keep track of level to solve the problem. 
The general approach is to process the queue one level at a time, we do this by waiting for the queue to be empty, and once it is, process all the nodes we have been tracking in the level list, and then while we process those nodes and get the values for that level, we also add the child nodes to the queue for the next level to be processed.
One important thing to note, is that `[None]` is not Falsy, so we don't want unprocessed `None` nodes in the queue, so we need to check the children of nodes beforehand, so that we don't accidentally add `None` to the queue.

#### Solution
```python 
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        Q = [root]
        ans = []
        currLevel = []

        while Q:
            # Pop elt from Q
            curr = Q.pop()
            # If non-None
            if curr:
                # Add to the curr ans list
                currLevel.append(curr)
                # If the Q is empty, we finished the row
                if not Q:
                    currLevelVal = []
                    # Process all elts in currLevel
                    while currLevel:
                        currNode = currLevel.pop()
                        currLevelVal.append(currNode.val)
                        if currNode.left:
                            Q.append(currNode.left)
                        if currNode.right:
                            Q.append(currNode.right)
                    # Record answers for level
                    ans.append(currLevelVal)
                    
        return ans
```
An alternate but similar approach would be to use a `for` loop inside the `while Q` loop, this makes it a little easier to follow as we can just directly process all the elements in the queue at once, and don't need to deal with checking `if not Q` from within the while loop, and can instead process all the elements in the queue at once, because we know they will all be on the same level.

#### Time Complexity
`O(n)` each node gets added to the queue and processed at most once. 