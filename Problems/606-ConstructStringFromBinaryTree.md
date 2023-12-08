---
tags: [easy, recursion, string]
---
### 606. Construct String from Binary Tree

Link: [here](https://leetcode.com/problems/construct-string-from-binary-tree/submissions/)

#### Problem
Given the `root` of a binary tree, construct a string consisting of parenthesis and integers from a binary tree with the preorder traversal way, and return it.

Omit all the empty parenthesis pairs that do not affect the one-to-one mapping relationship between the string and the original binary tree.

#### Main Idea
- Use recursion 
- Handle all cases of left and right children being present

#### Approach
- See above

#### Edge Cases
- N/A
#### Solution
```python 
class Solution:
    def tree2str(self, root: Optional[TreeNode]) -> str:
        if not root:
            return
        left = self.tree2str(root.left)
        right = self.tree2str(root.right)
        if not left and not right:
            return f"{root.val}"
        elif not right:
            return f"{root.val}({left})"
        elif not left:
            return f"{root.val}()({right})"
        else:
            return f"{root.val}({left})({right})"
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)` for the call stack

