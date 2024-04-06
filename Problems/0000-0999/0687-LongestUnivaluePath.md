---
tags: [medium, graph_traversal, depth_first_search]
---
### 687. Longest Univalue Path

Link: [here](https://leetcode.com/problems/longest-univalue-path/)

#### Problem
Given the `root` of a binary tree, return _the length of the longest path, where each node in the path has the same value_. This path may or may not pass through the root.

**The length of the path** between two nodes is represented by the number of edges between them.

#### Main Idea
- Make a helper method that tracks the parent node's value
- Use a global var to track the longest path, and return statements to track the longest current path

#### Approach
- Instead of checking every element's children to see if they have values present or not, make a helper method that takes parent values 
- Recursively call the left and right subtrees 
- Compare the sum of the left and right subtrees to the global max, we can compare the sums since we only return when the child element matches the parent element
- Now we actually handle the return for the function, which will be used for calls made by the parent node. Thus we don't need to consider the case where the current node is the root node, as we have already handled that scenario with the comparison to the global best variable
- Just return the highest value between the left and right subtrees +1 if the current node matches the parent. Otherwise it is not connected to the parent and thus we have a path length of 0.

#### Edge Cases
- `root` is None

#### Solution
```python 
class Solution:
    def __init__(self):
        self.ans = 0

    def longestUnivaluePath(self, root: Optional[TreeNode]) -> int:
        self.helper(root, -1)
        return self.ans

    def helper(self, root, parent):
        if not root: 
            return 0
        # Check the left and right children
        left = self.helper(root.left, root.val)
        right = self.helper(root.right, root.val)
        # Left and right are only nonzero if they are equal to parent
        self.ans = max(self.ans, left+right)
        # Return a longer path if we match our parent
        return max(left, right) + 1 if root.val == parent else 0
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)` or `O(n)` if you count the call stack

