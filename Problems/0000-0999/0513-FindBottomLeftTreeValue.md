---
tags: [medium, breadth_first_search, tree]
---
# 513. Find Bottom Left Tree Value
Link: [here](https://leetcode.com/problems/find-bottom-left-tree-value/description/)
## Problem
Given the `root` of a binary tree, return the leftmost value in the last row of the tree.
## Main Idea
- [[BreadthFirstSearch|BFS]] the tree and visit the right children first 
- The last node you visit will be the leftmost node
## Approach
- See above
## Edge Cases/Gotchas 
- None, just be aware the question is asking for the leftmost at the bottom level of the graph, not the leftmost node in the tree
## Solution
```python 
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        Q = deque()
        Q.append(root)
        leftest = None
        while Q:
            # Pull out leftmost node on level
            leftest = Q[0]
            curr = Q.popleft()
            # Process right before left, so left is last
            if curr.right:
                Q.append(curr.right)
            if curr.left:
                Q.append(curr.left)
        return leftest.val
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
