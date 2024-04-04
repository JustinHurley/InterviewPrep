---
tags:
  - medium
  - binary_tree
---
# 285. Inorder Successor in BST
Link: [here](https://leetcode.com/problems/inorder-successor-in-bst/description/)
## Problem
Given the `root` of a binary search tree and a node `p` in it, return _the in-order successor of that node in the BST_. If the given node has no in-order successor in the tree, return `null`.

The successor of a node `p` is the node with the smallest key greater than `p.val`.
## Main Idea
- If you go left your successor will be the last node
- If you go right your successor will be the last successor 
## Approach
- Use a while loop as you move through the array
- If the current value is larger than in `p` we should look left 
## Edge Cases/Gotchas 
- When the value is equal to `p`, we should just keep going since we still want to maintain the same successor 
## Solution
```python 
class Solution:
    def inorderSuccessor(self, root: TreeNode, p: TreeNode) -> Optional[TreeNode]:
        succ = None
        while root:
            if p.val >= root.val:
                root = root.right
            else:
                succ = root
                root = root.left
        return succ
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
