### 572. Subtree of Another Tree

#### Topics
- Tree traversal
- Trees
- Breadth-first-search
- Depth-first-search
- Recursion

#### Problem 
Given the roots of two binary trees root and subRoot, return true if there is a subtree of root with the same structure and node values of subRoot and false otherwise.

A subtree of a binary tree tree is a tree that consists of a node in tree and all of this node's descendants. The tree tree could also be considered as a subtree of itself.

#### Approach
This is a tree traversal question with an added twist. The general approach is to traverse the tree (it doesn't matter how), and search until you find a node that matches the value of the `subRoot`. Once you find this you want to recursively compare the subtree and the `subRoot`. Return `True` if you make it all they way to the `null` leaves at the bottom of the tree, and if there is ever a discrepancy, just return false. 

#### Solution
```
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

