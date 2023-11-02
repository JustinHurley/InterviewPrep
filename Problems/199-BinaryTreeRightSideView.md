---
tags:
- binary_tree
- tree
- breadth_first_search
- queue
---
### 199. Binary Tree Right Side View

Link: [here](https://leetcode.com/problems/binary-tree-right-side-view/description/)

#### Problem
Given the `root` of a binary tree, imagine yourself standing on the **right side** of it, return _the values of the nodes you can see ordered from top to bottom_.

#### Main Idea
- Want to find the right-most node in each level
- Use an adjusted [[BreadthFirstSearch|BFS]] to iterate level-by-level

#### Approach
The general approach is to traverse through the tree using BFS, with a change to the algorithm to go level by level. Instead of continuously iterating through the queue until we have visited all nodes in the tree, we want to go level-by-level. We can do this by using 2 arrays. One to keep track of nodes in the current level, and one to store the nodes in the next level. We processes each level until there are finally no more nodes left, and thus we have traversed every node in the tree.

#### Edge Cases
- When the input node is `None` that will cause issues if you add it to the queue.

#### Solution
```python 
class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        ans = []
        if not root:
            return None
        Q = [root]
        # While there are nodes in the queue
        while Q:
            next_Q = []
            last_val = -1
            # For each node in Q or all nodes in level
            for node in Q:
                last_val = node.val
                # If we have a next level, we should add it
                if node.left:
                    next_Q.append(node.left)
                if node.right:
                    next_Q.append(node.right)
            # The last val assigned is the rightmost
            ans.append(last_val)
            # Now that our next Q is ready, replace curr Q
            Q = next_Q
        return ans
```
#### Time Complexity
`O(n)` we only visit each node once.

#### Space Complexity
`O(n)` we store all the nodes in queues. 

