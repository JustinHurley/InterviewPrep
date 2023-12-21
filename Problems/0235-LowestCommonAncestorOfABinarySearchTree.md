---
tags:
  - binary_search_tree
  - recursion
  - tree
  - depth_first_search
  - medium
---

### 235. Lowest Common Ancestor of a Binary Search Tree

Link: [here](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)

#### Problem
Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow a node to be a descendant of itself).”

#### Approach
The most important thing to do in this problem is to think about what information we get from having the data in a binary search tree. Thinking about how a binary search tree is structured, we know that at a given node, all the children in the left subtree are smaller, and all the children in the right subtree are larger. This helps us narrow down on what our LCA is.
To find the LCA, we want to find the first node that includes or splits the range of `p` and `q`. What I mean by that is say we have `p=3` and `q=8`. If we are looking at `root=11`, we know that both `p` and `q` exist to the left of the `root` node, and thus there must be a lower common ancestor. However, let's consider the case, `root=6`. In this case, we see that `p < root` and `root < q`. This means that `p` is in the left subtree, and `q` must be in the right subtree. Which means that we cannot go deeper in the tree or we will not be looking at an LCA anymore.
So what this mean for finding a solution is that the LCA is found (assuming `p.val < q.val`) when:
```
p.val <= root.val <= q.val
```
because we know that if we go to either subtree, we will no longer be an ancestor of one of the nodes. 

Something else to consider in this problem is the fact that we are dealing with unique values, and there is always an answer in each test case. In reality, this would be a more complex problem if we needed to handle when there wasn't an answer (`p` or `q` missing), or if there are duplicates (we need to find more than 1 valid case and have a way of comparing which one is "better").

#### Solution
**Recursive**
```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        nodeRange = (p,q)

        if not root:
            return

        if p.val > q.val:
            nodeRange = (q,p)

        # Base case, curr val splits tree
        if root.val >= nodeRange[0].val and root.val <= nodeRange[1].val:
            return root
        
        # Otherwise, go left or right
        if root.val > nodeRange[0].val and root.val > nodeRange[1].val:
            return self.lowestCommonAncestor(root.left, p, q)
        else:
            return self.lowestCommonAncestor(root.right, p, q)
```
**Iterative**
```
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        curr = root

        while curr:
            if p.val < curr.val and q.val < curr.val:
                curr = curr.left
            elif p.val > curr.val and q.val > curr.val:
                curr = curr.right
            else:
                return curr
```

#### Time Complexity
`O(n)` we visit each node at most once. 