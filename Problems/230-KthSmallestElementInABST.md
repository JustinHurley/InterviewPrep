tags:
- tree
- binary_search_tree
- depth_first_search
---

### 230. Kth Smallest Element in a BST

Link: [here](https://leetcode.com/problems/kth-smallest-element-in-a-bst/description/)

#### Problem
Given the `root` of a binary search tree, and an integer `k`, return the `kth` smallest value (1-indexed) of all the values of the nodes in the tree.

#### Approach
The general approach involves using intrinsic properties of a BST to help us find the answer we want. The first question is, how do we know what the smallest element in the tree is? In this case, we can just go to the left-most node in the tree. 
The trickier part of the problem is determining the kth smallest value from these values. We can also use properties of BSTs in this situation to determine what that value is. The next left-most value will be the next smallest value, so if the left-most value has a right-node, that's the next smallest, otherwise the parent node of the smallest is the next smallest. 
So the general approach to this problem is:
- Go all the way to the left.
- Once you reach the left-most node, start looking right
- If there are no right nodes, start [[Categories/Backtracking|backtracking]].
Since we will have to "backtrack" in this scenario. We use a stack to keep track of nodes in this situation. We add left nodes until we can't to the stack, and then we start adding right nodes until we can't to the stack. The difference between adding left and right nodes is that when we see a right node, we start counting, since it means that we are looking at the next smallest value. We keep doing this until our counted value equals k, then we return the current node.
This problem uses DFS, however benefits from using an iterative approach instead of a recursive approach, since we use a stack anyway, and [[Categories/Backtracking|backtracking]] while also returning values only in some cases with a recursive approach is more difficult. 

#### Solution
```python 
class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        n = 0
        stack = []
        cur = root

        while cur or stack:
            # If there is a cur, let's go left
            while cur:
                stack.append(cur)
                print(cur.val)
                cur = cur.left
                
            # Once there isn't we should pop a node from the stack
            cur = stack.pop()
            n += 1
            # Now we should see if we're at the right node
            if n == k:
                return cur.val
            # If k != n, we want to look right 
            cur = cur.right
        return -1
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)` since we use a stack that could contain all the nodes in the tree. 