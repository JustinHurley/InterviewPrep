---
tags: [tree, depth_first_search, binary_tree]
---
### 110. Balanced Binary Tree

Link: [here](https://leetcode.com/problems/balanced-binary-tree/description/)

#### Problem
Given a binary tree, determine if it is [[Trees#^6f1160|height balanced]]

#### Main Idea
The main idea is to determine if a tree is balanced by determining if the subtrees were balanced, and the heights of the subtrees differed by less than 2. 

#### Approach
We know we want to traverse the tree and look at subproblems, so we want to use [[DepthFirstSearch|depth-first search]]. We make a helper method that recursively looks at the left and right children, gets the max depth from each, and whether or not that subtree is balanced. 
So at each step we are basically ensuring that the subtrees are balanced, and then ensuring that the overall tree is balanced as well. 
The base case will be when we get a null node, and we have reached the bottom. Since there is nothing to balance we can just return `-1, True` for the height and if it's balanced.
If we see that the left or right children are unbalanced, we can also return `False`. 
Otherwise we return the current height, and if the heights of the left and right children are balanced. 

#### Edge Cases
- Determining if a `None` tree counts as balanced or not.

#### Solution
```python 
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        return self.dfs(root)[1]

    def dfs(self, root):
        # If we pass a leaf, stop
        if not root:
            return -1, True
        # Otherwise check the diff
        left_h, left_b = self.dfs(root.left)
        right_h, right_b = self.dfs(root.right)
        # If either aren't balanced, stop
        if not left_b or not right_b:
            return 0, False
        # Otherwise keep going
        return max(left_h,right_h) + 1, (abs(left_h - right_h) < 2)
```

#### Time Complexity
`O(n)` where `n` is the total number of nodes in the tree.

#### Space Complexity
`O(n)` for the space created in the call stack 

