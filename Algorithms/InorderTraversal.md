### Inorder Traversal

#### General Idea
Inorder traversal is an algorithm used to traverse binary trees. A variation of DFS, we recursively call child nodes if present from the parent node. The main thing is that we "visit" the node between making the recursive call on the left and right subtrees. 
The main benefit of traversing this way is that when we are given a valid BST, we can use inorder traversal to get the nodes in non-decreasing order, i.e. sorted from smaller to larger.

#### Code
```
root: TreeNode = treeNode

def inorder(node):
    if not node:
        return
    
    inorder(node.left)
    # Where we "do work" on the given node
    visit(node.value)
    inorder(node.right)

# Traverses the whole tree
inorder(root)
```

#### Time Complexity
`O(n)`

#### Space Complexity 
`O(n)` since we need to make a call stack for recursive calls, the hypothetical worst case scenario is a BST that has only left nodes, where we make a call stack of size `n`.