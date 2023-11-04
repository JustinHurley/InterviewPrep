---
tags: [binary_tree, recursion, depth_first_search]
---

### 226. Invert Binary Tree

Link: [here](https://leetcode.com/problems/invert-binary-tree/description/)

#### Problem
Given the `root` of a binary tree, invert the tree, and return its root.

#### Approach
The approach is fairly simple, given that we are working with a tree, and in this case want to use DFS, we can just recur on the tree and swap nodes at each recursion. The base case is when we are given an empty node, in which case we just return `None`. Otherwise we swap the left and right child nodes of the root, and then make recursive calls on those children. Finally, after the recursion, we return the updated root node.

#### Solution
```python 
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        # Base case
        if not root:
            return None
        
        # Else swap nodes
        temp = root.left
        root.left = root.right
        root.right = temp

        # Then make recursive call on child nodes
        self.invertTree(root.left)
        self.invertTree(root.right)

        # When recursion is done we return the now-updated root node
        return root
```

#### Time Complexity
`O(n)`, we visit all the nodes once, and then there is extra overhead because of recursion but it is `< n` so we don't need to consider it.