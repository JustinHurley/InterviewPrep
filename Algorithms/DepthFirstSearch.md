### Depth-First Search

#### General Idea
Depth-First Search, or pre-order traversal, is a way to navigate a tree in such a way that it processes a node, then processes the entire left subtree, then processes the entire right subtree. As the name implies, it goes all the way to the bottom of the left children of the tree first, as opposed to traversing all the elements in a level before moving on. Depth-first search is commonly used when you are looking for something in a tree, or need to find elements in a way where you don't need to traverse by level. Commonly, DFS is implemented recursively, by processing the root node and then recursively calling the children nodes.

#### Code
```
DFS(root):
    # If the current node is null, then we are at the bottom of the tree.
    if not root:
        return None
    # Some function that does work on the current node.
    process(root)
    # Once we are done doing work on the current node, we then go to the left and right subtrees.
    DFS(root.left)
    DFS(root.right)

```

#### Time Complexity
`O(n)` you will visit all the nodes in the tree once.

#### Space Complexity 
`O(1)` no extra space is needed (besides the negligible space used in the call stack).