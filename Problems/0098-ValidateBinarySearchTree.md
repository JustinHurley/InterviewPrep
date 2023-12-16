---
tags:
  - binary_search_tree
  - recursion
  - tree
  - depth_first_search
  - medium
---

### 98. Validate Binary Search Tree

Link: [here](https://leetcode.com/problems/validate-binary-search-tree/description/)

#### Problem
Given the `root` of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.

#### Approach
The general approach is to create a helper method that tracks the appropriate range of a given node relative to the tree. This works because of fundamental properties of BSTs. We know that a left child must be less than the parent node, and that the right child must be larger than the parent node. We can keep these facts in mind when navigating the tree, and keeping track of bounds as we go. 
We need to use a helper method to track the allowed range of a given node, while still having it be a binary tree. The helper method just adds 2 new fields, a `left` and `right` range based on previously traversed nodes. If our current node is equal to or outside that range (because BSTs don't allow for duplicates), we are working with an incorrectly designed tree, and should return false, otherwise, we should keep looking. 

NOTE: to have infinity and negative infinity in Python, use `float("inf")` and `float("-inf")` respectively.

#### Solution
```python 
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        return self.helper(root, float("-inf"), float("inf"))
    
    def helper(self, node, left, right):
        if not node:
            return True
        
        if node.val <= left or node.val >= right:
            return False
        
        return self.helper(node.left, left, node.val) and self.helper(node.right, node.val, right)
```

#### Time Complexity
`O(n)` since we visit each node once.

#### Space Complexity
`O(1)` we just traverse the tree, and don't add anything to it, so no we have constant space complexity. 