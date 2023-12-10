---
tags: [math, recursion, depth_first_search]
---
### 1120. Maximum Average Subtree

Link: [here](https://leetcode.com/problems/maximum-average-subtree/description/)

#### Problem
Given the `root` of a binary tree, return _the maximum **average** value of a **subtree** of that tree_. Answers within `10-5` of the actual answer will be accepted.

A **subtree** of a tree is any node of that tree plus all its descendants.

The **average** value of a tree is the sum of its values, divided by the number of nodes.

#### Main Idea
We can use DFS to "bubble up" the running averages at each node and for each of it's subtrees.

#### Approach
- DFS on tree
- Get current sum and current count of nodes for the left and right subtrees
- Compare those with the current best average

#### Edge Cases
- Make sure to not divide by zero when getting averages for nodes that are `None`

#### Solution
```python 
class Solution:
    def maximumAverageSubtree(self, root: Optional[TreeNode]) -> float:
        best = [0]
        def helper(root):
            if not root:
                return (0, 0)
            left_sum, left_count = helper(root.left)
            right_sum, right_count = helper(root.right)
            curr_sum = left_sum + right_sum + root.val
            curr_count = left_count + right_count + 1
            left_avg = left_sum/left_count if left_count != 0 else 0
            right_avg = right_sum/right_count if right_count != 0 else 0
            curr_avg = curr_sum/curr_count
            best[0] = max(left_avg, right_avg, curr_avg, best[0])
            return (curr_sum, curr_count)
        helper(root)
        return best[0]
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)` for the call stack

