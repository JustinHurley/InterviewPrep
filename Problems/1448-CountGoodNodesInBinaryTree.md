---
tags:
  - binary_tree
  - depth_first_search
  - tree
  - recursion
  - medium
---
### 1448. Count Good Nodes in Binary Tree

Link: [here](https://leetcode.com/problems/count-good-nodes-in-binary-tree/description/)

#### Problem
Given a binary tree `root`, a node _X_ in the tree is named **good** if in the path from root to _X_ there are no nodes with a value _greater than_ X.

Return the number of **good** nodes in the binary tree.

#### Main Idea
- Determine if all values on the path from the root to a given node are smaller than the node's value
- Do this for all nodes
- Use recursion to "bubble-up" the answer

#### Approach
We are going to use [[DepthFirstSearch|DFS]] since the problem is asking about the path from the root to a given node, which DFS's recursive calls will make it easy to do. We will make a helper method that tracks the current max val from itself and the root. For each node we can just return 1 if it's a good node, or 0 if it's a bad node, and we then need to add that value to the counts of good nodes for each subtree. 

#### Edge Cases
- `node.val` is negative
- `root` is null

#### Solution
```python 
class Solution:
    def goodNodes(self, root: TreeNode) -> int:
        return self.DFS(root, float('-inf'))
    
    def DFS(self, root, curr_max):
        if not root:
            return 0
        
        if root.val >= curr_max:
            left_count = self.DFS(root.left, root.val)
            right_count = self.DFS(root.right, root.val)
            return left_count + right_count + 1
        else:
            left_count = self.DFS(root.left, curr_max)
            right_count = self.DFS(root.right, curr_max)
            return left_count + right_count 
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)` for call stack space.

