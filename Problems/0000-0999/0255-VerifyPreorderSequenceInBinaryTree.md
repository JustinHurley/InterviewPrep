---
tags:
  - medium
  - graph_traversal
  - binary_tree
  - stack
  - monotonic_stack
---
# 255. Verify Preorder Sequence in Binary Tree
Link: [here](https://leetcode.com/problems/verify-preorder-sequence-in-binary-search-tree/)
## Problem
Given an array of **unique** integers `preorder`, return `true` _if it is the correct preorder traversal sequence of a binary search tree_.
## Intuition
- Know what it means to traverse in preorder, we first do work on the node and then after we we visit the left subtree, and then finally the right subtree
- How do we know we have an invalid tree? We know a tree is invalid when we get an element that belongs in the left subtree that is in the right subtree
- For example `[5, 2, 6, 1, 3]` would be invalid because we would start with `5`, add `2` to the left subtree, and then `6` would imply that we should move to the right subtree, but when we see `1` adding it anywhere in the right side is invalid 
- If we can keep track of when we switch from the left to right branches, we can use that to track of a global maximum value that all of the future incoming nodes need to be larger to keep a valid tree
- Using the `[5, 2, 6, 1, 3]` example, we can set `5` to the max, then switch when we see `6` and then when we see that `1 < 5` return false 
## Approach
- Init a stack and a minimum valid value that all nodes need to be larger than (start at `-inf`)
- Go through the nodes in `preorder`
- If a node is smaller than the TOS, it means we are building the left subtree and should keep going
- If a node is not smaller, we should pop elements in the stack until we find the value that is larger, which represents the root for the node that we can use to start building the right subtree
- When we switch to the right subtree (popping in the stack) we should also use the TOS value to potentially update the largest value if one is found
- Finally, if we then see a value that is smaller than the allowed largest value, it means we are trying to add a value in the right subtree that needs to be in the left subtree
- If we found no invalid values, return `True`
## Edge Cases/Pitfalls
- You need to use a stack because any of the potential previous larger values could be where we want to branch off to make the right subtree i.e. you don't always want to go back up to the root node
- Largest value should only be updated with nodes in the stack 
## Solution
```python 
class Solution:
    def verifyPreorder(self, preorder: List[int]) -> bool:
        stack = []
        largest = -inf
        for val in preorder:
            while stack and val > stack[-1]:
                largest = max(largest, stack[-1])
                stack.pop()
            if val < largest:
                return False
            stack.append(val)
        return True
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
## Takeaways 
- Think about what makes an input valid and what makes an input invalid 
- You can just do `inf` and `-inf` for infinity 
