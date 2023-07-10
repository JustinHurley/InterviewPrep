---
tags:
- binary_tree
- recursion
- depth_first_search
- tree
---

### 104. Maximum Depth of Binary Tree

Link: [here](https://leetcode.com/problems/maximum-depth-of-binary-tree/description/)

#### Problem
Given the `root` of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

#### Approach
The approach for this algorithm is really simple. We are given a tree and want to find the maximum depth, so automatically we should be thinking DFS. To implement, we need to set up the base case, and the recusive case. The base case will be when we see an empty node, in which case we just return 0, as there is no node to count. The recursive case is more interesting, in which we want to return the max between the depth of the left and right child nodes. So we recursively call the child nodes in a max function, but also add 1 to each of the calls, because we want to account for the node that we are currently on. This sets up the recursive call structure that adds one for each call made, and returns 0 when you finally hit `None`.

#### Solution
```python 
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        return max(self.maxDepth(root.left)+1, self.maxDepth(root.right)+1)
```

#### Time Complexity
`O(n)` we need to visit all the nodes to determine the max depth of the tree.