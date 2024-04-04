---
tags:
  - medium
  - linked_list
  - binary_search_tree
  - binary_tree
  - two_pointer
  - depth_first_search
---
# 426. Convert Binary Search Tree to Doubly Linked List
Link: [here](https://leetcode.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/description/)
## Problem
Convert a **Binary Search Tree** to a sorted **Circular Doubly-Linked List** in place.

You can think of the left and right pointers as synonymous to the predecessor and successor pointers in a doubly-linked list. For a circular doubly linked list, the predecessor of the first element is the last element, and the successor of the last element is the first element.

We want to do the transformation **in place**. After the transformation, the left pointer of the tree node should point to its predecessor, and the right pointer should point to its successor. You should return the pointer to the smallest element of the linked list.
## Main Idea
- Use DFS and traverse the graph using an [[InorderTraversal]] to process the nodes in the correct order
- For each recursive call, we call the left subtree, attach it to the current node, and then update the pointers to reflect the new linked list with left and right pointers, once that's all updated, we call the method on the right part of the subtree, which will recursively add itself to the right side of the linked list
## Approach
- Make left and right pointers to keep track of the left and right heads of the doubly linked list
- Make a DFS helper method
- Do a null check in the method
- Recursively call the left side
- If there is a right side, connect it to the current node, if there isn't a right side it means the current node is the left side, so update it to that
- Once the left subtree has been explored, the current node is now the rightmost node, so we should update it
- Outside of the DFS method, we call it on the root
- We also need to connect the left and right ends to each other to make the linked list circular 
## Edge Cases/Gotchas 
- Root node is null
## Solution
```python 
class Solution:
    def treeToDoublyList(self, root: 'Optional[Node]') -> 'Optional[Node]':
        first, last = None, None
        # DFS
        def dfs(node):
            nonlocal first, last
            # null check
            if not node:
                return None
            # Traverse left subtree to update first and last
            dfs(node.left)
            # If we have a last, connect to it
            if last:
                last.right = node
                node.left = last
            # Otherwise the current node is the new leftmost
            else:
                first = node
            # Rightmost node is also curr
            last = node
            # Traverse the right subtree to connect 
            dfs(node.right)
        if not root:
            return None
        dfs(root)
        # Connect the last to the first to make circular
        last.right = first
        first.left = last
        return first
```
## Time Complexity
$O(n)$ we traverse over every node
## Space Complexity
$O(n)$ for call-stack space
