---
tags: [binary_tree, tree, recursion, divide_and_conquer, array]
---

### 105. Construct Binary Tree from Preorder and Inorder Traversal

Link: [here](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

#### Problem
Given two integer arrays `preorder` and `inorder` where `preorder` is the `preorder` traversal of a binary tree and `inorder` is the `inorder` traversal of the same tree, construct and return the binary tree.

#### Approach
Like many of these binary tree problems, this one hinges on some knowledge of how traversals work, and figuring out how to apply that to the problem. As we can see, we are given 2 lists, `preorder` and `inorder`. Now something we know about preorder traversals is that they start from the root, process the left node until they ca n't, then process the right node. This means that the first element in `preorder` is the root value of the tree. So that's pretty handy, but what can we learn from `inorder`? Well we know that `inorder` displays the relative order of the elements to each other. Consider:
```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
```
Which was made from a tree that looked like this:
```
        3
    9       15
          20   7
```
As we can see, the inorder traversal lets us know if an element is on the left or right side of a given node. 9 is to the left of 3 and the other values are to the right of 3. So, using both lists, we can determine the current root node of a tree, and which nodes belong in the left or right subtree. So that's all we have to do, we recursively find the root node value, and the left and right subtrees, and then partition the left and right sub-lists to build the tree.
Note: The easy python way to find the index of a value in a list is by using `List.index(val)`. This returns the index of the value in the list, but will throw and error if the value is not in the list. This does run in `O(n)` time. 
#### Solution
```python 
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        # Null check
        if not preorder or not inorder:
            return None

        # Find the index of val in inorder
        mid = inorder.index(preorder[0])
        
        # Now that we have the mid, we need to prep the next recursion level
        return TreeNode(preorder[0], self.buildTree(preorder[1:mid+1], inorder[:mid]), self.buildTree(preorder[mid+1:], inorder[mid+1:]))
```

#### Time Complexity
`O(n^2)` since worst case would involve us needing to iterate over most of the list each time (for example if we were given a tree that only had right nodes or something).

#### Space Complexity
`O(n)`, but you could optimize with counter variables so that you don't have to slice the array and just change the ranges as you iterate, meaning you don't mutate the original arrays, just the portion you access.