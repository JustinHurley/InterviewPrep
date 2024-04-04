---
tags:
  - easy
  - graph
  - binary_tree
  - graph_traversal
  - depth_first_search
---
# 145. Binary Tree Postorder Traversal
Link: [here](https://leetcode.com/problems/binary-tree-postorder-traversal/description/)
## Problem
Given the `root` of a binary tree, return _the postorder traversal of its nodes' values_.
## Main Idea
- Visit node, make recursive calls, then do work
## Approach
- Make a [[DepthFirstSearch|DFS]] method
## Edge Cases/Gotchas 
- Make sure the node isn't null
## Solution
```python 
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        ans = []
        def dfs(node):
            if not node:
                return
            dfs(node.left)
            dfs(node.right)
            ans.append(node.val)
        dfs(root)
        return ans
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
