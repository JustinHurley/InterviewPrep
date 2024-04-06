---
tags:
  - binary_tree
  - graph_traversal
  - breadth_first_search
  - medium
  - tree
---
# 314. Binary Tree Vertical Order Traversal
Link: [here](https://leetcode.com/problems/binary-tree-vertical-order-traversal/description/)
## Problem
Given the `root` of a binary tree, return _**the vertical order traversal** of its nodes' values_. (i.e., from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from **left to right**.
## Main Idea
- Keep track of the index of the current node, moving left means `i-1` and moving right means `i+1`
- Use BFS since we care about vertical order (DFS would not preserve this)
- So we don't need to sort the elements of our hashmap at the end, we can remove 
## Approach
- Make the `deque` for the BFS Q
- Make a hashmap to store values at each index
- Make a left and right pointer to keep track of the leftmost and rightmost index
- Add the root node to the `deque` with the index of 0
- Now go into the BFS method
	- Pop the Q
	- Update the leftmost and rightmost pointers
	- Add the current value to its respective index
	- If applicable, add left and right children to Q with respective indicies
- Once we ran the BFS, we need to make the answer array in order, which we can do by using the leftmost and rightmost pointers to call the hashmap indicies in order (as opposed to sorting the keys)
## Edge Cases/Gotchas 
- The input root can be `None` so we need to check that node before doing work
- Make sure to check left and right children to see if they exist before adding to the node
## Solution
```python 
class Solution:
    def verticalOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        Q = deque()
        Q.append((root, 0))
        ans = defaultdict(list)
        leftmost, rightmost = 0, 0

        while Q:
            curr, i = Q.popleft()
            leftmost = min(leftmost, i)
            rightmost = max(rightmost, i)
            # Add to ans
            ans[i].append(curr.val)
            # Add child nodes to Q
            if curr.left:
                Q.append((curr.left, i-1))
            if curr.right:
                Q.append((curr.right, i+1))

        return [ans[i] if i in ans else _ for i in range(leftmost, rightmost+1)]
```
## Time Complexity
$O(n)$ 
## Space Complexity
$O(n)$
