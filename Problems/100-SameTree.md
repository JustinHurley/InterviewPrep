---
tags:
- binary_tree
- depth_first_search
- tree
---

### 100. Same Tree

Link: [here](https://leetcode.com/problems/same-tree/description/)

#### Problem
Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

#### Approach
Like almost all DFS problems, this one involves coming up with base cases for when we want to resolve the given function, and the recursive case, for when we need to keep exploring the tree. Because we want to determine when the given nodes are the same, we have a few cases we need to handle. Obviously if the values aren't equal at a given node, then we know that the [[trees]] are not the same and we should return `False`. There is also a case where we reach the `None` children of a given node, and we don't need to look anymore so return `True`. The final base-case we need to consider is when one of the nodes we are comparing is present, and the other one is `None`, in which case we return `False`, as they are clearly not the same.
For the recursive case, we just want to iterate on the child nodes of each given node, and if any part of the tree is the same, we want to return false, so we join each of the recursive calls made on the child nodes with an `and`.

#### Solution
```python 
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if p == None and q == None:
            return True
        elif (p == None and q != None) or (p != None and q == None):
            return False
        elif p.val != q.val:
            return False
        
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

#### Time Complexity
`O(n)`