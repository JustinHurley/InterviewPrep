---
tags:
  - graph_traversal
  - tree
  - breadth_first_search
  - depth_first_search
  - recursion
  - medium
---

### 572. Subtree of Another Tree

#### Problem 
Given the roots of two binary trees root and subRoot, return true if there is a subtree of root with the same structure and node values of subRoot and false otherwise.

A subtree of a binary tree tree is a tree that consists of a node in tree and all of this node's descendants. The tree tree could also be considered as a subtree of itself.

#### Approach
**Iterative Approach**
This is a tree traversal question with an added twist. The general approach is to traverse the tree (it doesn't matter how), and search until you find a node that matches the value of the `subRoot`. Once you find this you want to recursively compare the subtree and the `subRoot`. Return `True` if you make it all they way to the `null` leaves at the bottom of the tree, and if there is ever a discrepancy, just return false. 

**Recursive Approach**
We can use DFS in this problem to recursively check if the tree we are looking at is the same as the subtree we are given. The base cases are as such: if the tree node we are looking at is `None`, then we are done, as it can't possible be the same as the subtree we are given (subtree has at least one node). The other base case is when the nodes of the tree and subtree have the same value, in which case we want to check to see if the rest of the tree and subtree is the same. If it is the same, we should return true, but if it's not the same, we don't want to return `false`, we want to keep looking.
The recursive case is how we keep looking, by recursively calling the method on the left and right children of the given tree. 
We need to implement the function that checks to see if the two [[Trees]] are the same, however this is done in a previous problem and is trivial. 
#### Solution
**Iterative Approach**
```python
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        # Make queue
        Q = collections.deque([root])
        while Q:
            node = Q.popleft()
            if node.val == subRoot.val:
                if Solution.isSame(node, subRoot):
                    return True
            if node.left:
                Q.append(node.left)
            if node.right:
                Q.append(node.right)
        
    def isSame(node, subNode) -> bool:
        # If we have 2 vals, we should compare them and keep looking
        if node and subNode:
            if node.val == subNode.val and Solution.isSame(node.left, subNode.left) and Solution.isSame(node.right, subNode.right):
                return True
        # If we have 2 empty vals, we're done
        elif not node and not subNode:
            return True
        # If we have something else, it means they aren't the same
        else:
            return False
```

**Recursive Approach**
```python
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        if not root:
            return False
        if root.val == subRoot.val:
            if self.isSame(root, subRoot):
                return True
        
        return self.isSubtree(root.left, subRoot) or self.isSubtree(root.right, subRoot)

    def isSame(self, a: Optional[TreeNode], b: Optional[TreeNode]) -> bool:
        if not a and not b:
            return True
        if (not a or not b) or (a.val != b.val):
            return False
        
        return self.isSame(a.left, b.left) and self.isSame(a.right, b.right)
```

#### Time Complexity 
`O(n^2)` since each node could have a subtree check.