---
tags: [depth_first_search, recursion, math]
---
### 2265. Count Nodes Equal to Average of Subtree

Link: [here](https://leetcode.com/problems/count-nodes-equal-to-average-of-subtree/)

#### Problem
Given the `root` of a binary tree, return _the number of nodes where the value of the node is equal to the **average** of the values in its **subtree**_.

**Note:**
- The **average** of `n` elements is the **sum** of the `n` elements divided by `n` and **rounded down** to the nearest integer.
- A **subtree** of `root` is a tree consisting of `root` and all of its descendants.
#### Main Idea
- Use DFS to evaluate each subtree
- Bubble-up the sum of values and sum of nodes to get the average

#### Approach
You basically want to DFS the tree and on each node, get the averages of the subtrees, and then compare that average to the current root node, and finally update the counter if the current value is higher than the average of the subtree. 

#### Edge Cases
- How to handle empty tree (divide by 0)

#### Solution
```python 
class Solution:
    def __init__(self):
        self.count = 0

    def averageOfSubtree(self, root: Optional[TreeNode]) -> int:
        self.getAverages(root)
        return self.count

    # (sum, count)
    def getAverages(self, root):
        if not root:
            return 0, 0
        left = self.getAverages(root.left)
        right = self.getAverages(root.right)
        if root.val == (left[0]+right[0]+root.val)//(left[1]+right[1]+1):
            self.count += 1
        return left[0]+right[0]+root.val, left[1]+right[1]+1
```
Note: you could also just keep track of the count with a return parameter from `getAverages`. 
#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)` but just for the call stack, otherwise it would be `O(1)` since no objects are instantiated besides one-off variables. 

