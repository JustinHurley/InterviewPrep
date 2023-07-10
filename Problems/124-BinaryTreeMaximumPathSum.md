---
tags:
- binary_tree
- recursion
- tree
- depth_first_search
---

### 124. Binary Tree Maximum Path Sum

Link: [here](https://leetcode.com/problems/binary-tree-maximum-path-sum/description/)

#### Problem
A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return the maximum path sum of any non-empty path.

#### Approach
We want to find the maximum path sum in the tree. This means that we need to consider certain situations at each node. For a given node, it could be one of 3 things:
1. The root node of the path.
2. Part of one of the sides of the path.
3. Not related to the best path at all.
Now, because we are summing, we want to "bubble-up" the sum from the bottom of the tree to the top. This means that we're going to want to do DFS, and the generic algorithm is going to work like this:
1. Check if the current node is null, if so return 0 (recursive base case).
2. Calculate the best paths of the left and right subtrees.
3. Calculate the path if the current node was the root, and compare to the best value.
4. Return the value of the current node added to the value of the better path between the left and right paths.
This will ensure that each node is considered for being the root node, and it also ensures that the sums we are bubbling up, only consider a single path (meaning no splits). Finally we return the best value, and that's our answer.
There are a couple tricky things to consider in this problem: 
- First off, we want to use a global variable to track the best value, while we run all these recursive calls. 
- The second consideration is that for a given root node, including one of the subtrees may harm our best answer, for example if one of the subtrees sums to a negative number, so we need to make sure that when considering the best path, we also set the left and right sub-paths to 0 if they are negative. 
- The third thing is that we want to set the global variable to the root node value, this is because if it is negative, we want to capture that. 

#### Solution
```python 
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        best = [root.val]
        
        def helper(root):
            # Null check
            if not root:
                return 0
            
            # Get the subtree best paths
            # Need to make 0 if less than 0 since we can always omit part of a path
            left = max(helper(root.left), 0)
            right = max(helper(root.right), 0)
            
            best[0] = max(best[0], root.val + left + right)

            return root.val + max(left, right)
        helper(root)
        return best[0]
```

#### Time Complexity
`O(n)` since we visit the nodes once.

#### Space Complexity
`O(1)` since we only make the global variable. 