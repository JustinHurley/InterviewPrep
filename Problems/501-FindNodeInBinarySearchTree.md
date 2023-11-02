---
tags:
- depth_first_search
- tree
- binary_search_tree
- binary_tree
- recursion
---
### 501. Find Node in Binary Search Tree

Link: [here](https://leetcode.com/problems/find-mode-in-binary-search-tree/description/)

#### Problem
Given the `root` of a binary search tree (BST) with duplicates, return _all the [mode(s)](https://en.wikipedia.org/wiki/Mode_(statistics)) (i.e., the most frequently occurred element) in it_.

If the tree has more than one mode, return them in **any order**.

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys **less than or equal to** the node's key.
- The right subtree of a node contains only nodes with keys **greater than or equal to** the node's key.
- Both the left and right subtrees must also be binary search trees.

#### Main Idea
- We can do an [[InorderTraversal|inorder traversal]] to access the elements in order because it's a BST.
- By setting global/instance variables, we don't need to keep track of the elements with an array, and can process as we traverse the tree.

#### Approach
This problem is trivial when not limited by constant-space complexity. In that case we would just have to traverse the tree and use a dictionary to count the frequency of each element, and then get the mode from there. 
When we are limited by constant space complexity, then we will have to skip the dictionary and keep track of the max as is. Since we navigate the BST in-order, then we can just keep track of the current max, as we know that all duplicate numbers will be next to each other e.g. `1,2,3,3,3,3,4,5,...`.
To keep track of the current and best values, we use instance variables to hold the current modes, the current max frequency, the global max frequency, and the current number we are on.
We count the number until we see a new number, at which point we stop, compare current to global, and then start counting the new number.

#### Edge Cases
- Numbers are negative 
- Input is `None`

#### Solution
```python 
class Solution:
    def __init__(self):
        self.max_streak = 0
        self.curr_streak = 0
        self.curr_num = -1
        self.ans = []
    
    def dfs(self, root):
        if not root:
            return
        self.dfs(root.left)
        num = root.val
        # If num is the same, continue streak
        if num == self.curr_num:
            self.curr_streak += 1
        # If different, compare to max and start over
        else:
            self.curr_num = num
            self.curr_streak = 1
        if self.curr_streak == self.max_streak:
            self.ans.append(num)
        elif self.curr_streak > self.max_streak:
            self.ans = [num]
            self.max_streak = self.curr_streak
        self.dfs(root.right)

    def findMode(self, root: Optional[TreeNode]) -> List[int]:
        self.dfs(root)
        return self.ans```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)` but kind-of `O(1)`, since we could end up with a call-stack of size `O(n)` from the recursive calls, although that is not directly instantiated by the algorithm. 

