---
tags: [easy, binary_tree, recursion, depth_first_search]
---
# 543. Diameter of Binary Tree
Link: [here](https://leetcode.com/problems/diameter-of-binary-tree/description/)
## Problem
Given the `root` of a binary tree, return _the length of the **diameter** of the tree_.
The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.
The **length** of a path between two nodes is represented by the number of edges between them.
## Main Idea
- At each node, determine the max length if it's the root node in the path
- Once compared to global best, keep DFSing on the left and right subtrees
## Approach
- Set a global best value 
- Make a recursive method that DFS-es to find the longest path in the binary tree, but also checks the longest path that can be made with the given node as a root
- Call the longest path DFS method over the root
- Return the best value
## Edge Cases/Gotchas 
- The longest node might not be the root node
- The root node might be null
- Check to make sure graph is a valid binary tree
## Solution
```python 
class Solution:
    def __init__(self):
        self.best = 0

    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.longestPath(root)
        return self.best
        
    def longestPath(self, node):
        if not node:
            return 0
        left_longest = self.longestPath(node.left)
        right_longest = self.longestPath(node.right)
        self.best = max(self.best, left_longest + right_longest)
        return max(left_longest, right_longest) + 1
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$ for stack space
