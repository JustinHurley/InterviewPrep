---
tags: [dynamic_programming, memoization, tree, binary_tree, recursion]
---
### 894. All Possible Full Binary Trees

Link: [here](https://leetcode.com/problems/all-possible-full-binary-trees/description/)

#### Problem
Given an integer `n`, return _a list of all possible **full binary trees** with_ `n` _nodes_. Each node of each tree in the answer must have `Node.val == 0`.

Each element of the answer is the root node of one possible tree. You may return the final list of [[trees]] in **any order**.

A **full binary tree** is a binary tree where each node has exactly `0` or `2` children.

#### Approach
The approach to this is to use recursion to inherently break down our problem into smaller subproblems. For a given value `n`, we know that `n` must be odd to make a valid balanced tree. This is because a valid balanced BST either has 0 or 2 nodes, and thus in a VBBST every node will have a partner besides the root node. 
The next case to think about is when there is only one node, in which case we return a `[TreeNode()]` since there is only 1 way to make a VBBST out of 1 node. 
Anything more than that, and we should refer to our memoization table to let us know if we have calculated this before. If so, then we can just return the list of trees already stored in the table. 
If we actually have to calculate the tree formations, then we should run through all the different ways we can split the nodes between the left and right children, recursively calling each left and right child-count permutation.
When we finally get these values, we should go through all the values for the left and right subtrees, create a root, and add it to a stored list that holds all the calculated trees for that level.
Finally all the trees for that `n` are added into the memoization table, and the list is returned.

#### Edge Cases
- If `n` is even, return an empty list since there are no VBBSTs you can make with that amount of nodes. 

#### Solution
```python 
class Solution:
    def __init__(self): 
        self.trees = {}

    def allPossibleFBT(self, n: int) -> List[Optional[TreeNode]]:
        # Base cases
        if n % 2 == 0:
            return []
        # If we need to make a balanced BST w/ 1 there is only the root
        if n == 1:
            return [TreeNode()]
        
        if n in self.trees:
            return self.trees[n]
        ans = []
        for i in range(1, n, 2):
            left = self.allPossibleFBT(i)
            right = self.allPossibleFBT(n-i-1)

            for l in left:
                for r in right:
                    root = TreeNode(0, l, r)
                    ans.append(root)
        
        self.trees[n] = ans
        return ans
```

#### Time Complexity
`O(2^(n/2))` since every 2 nodes double the number of trees you can make with that amount.

#### Space Complexity
`O(2^(n/2))` since you need to actually store every created tree.