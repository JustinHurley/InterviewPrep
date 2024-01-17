### Overview
Trees are acyclic [[Graphs]], where nodes typically start at a "root" and then branch off until the "leaves" are reached.

### Vocabulary
###### Root 
The top-most node in the tree. There is only one root.
###### Leaves
The bottom-most leaves in a tree, i.e. nodes with no children.
###### Internal Nodes
Any node that isn't a root or a leaf is an internal node, i.e. it has both child and parent nodes. 
###### Height-Balance

^6f1160

A tree where leaf nodes depth differs by at most 1.
### Kinds of Trees

**Binary Tree**: A binary tree is a tree where each node has at most 2 child nodes.

**Balanced Tree**: A tree is considered *balanced* when the leaf depth between all leaves in the tree differs by at most 1. 

**Binary Search Tree (BST)**: A BST is a kind of binary tree where each node in the left subtree is less than the current node, and each node in the right subtree is larger than the current node.

**Complete Binary Tree**: This tree has all levels besides the last one filled, and nodes in the lowest depth level are as left as possible. This kind of tree is often used in heap implementations. 


```query
tag:#tree OR tag:#graph_traversal
```
