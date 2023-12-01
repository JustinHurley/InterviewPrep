---
tags:
  - breadth_first_search
  - tree
  - binary_tree
  - medium
---
### 1361. Validate Binary Tree Nodes

Link: [here](https://leetcode.com/problems/validate-binary-tree-nodes)

#### Problem
You have `n` binary tree nodes numbered from `0` to `n - 1` where node `i` has two children `leftChild[i]` and `rightChild[i]`, return `true` if and only if **all** the given nodes form **exactly one** valid binary tree.

If node `i` has no left child then `leftChild[i]` will equal `-1`, similarly for the right child.

Note that the nodes have no values and that we only use the node numbers in this problem.

#### Main Idea
The main idea is that we know the binary tree is valid if there are no cycles, every node has between 0 and 2 children, and there is a distinct root node that all other nodes in the tree can be reached from.

#### Approach
1. Determine the root by finding the node that has no parents
2. Start BFS from that node
3. Keep track of visited nodes
4. If you were able to visit all nodes only once, it is connected and non-cyclic
5. Check the visited set, since the size of that should equal `n`, as we need to confirm all nodes have been visited once

#### Edge Cases
- 0-index node is not the root
- Tree is not connected 
- There are cycles
- `n` is an invalid value

#### Solution
```python 
class Solution:
    def findRoot(self, n, leftChild, rightChild):
        seen = set(leftChild) | set(rightChild)
        for i in range(n):
            if i not in seen:
                return i
        return -1

    def validateBinaryTreeNodes(self, n: int, leftChild: List[int], rightChild: List[int]) -> bool:
        seen = set()
        Q = deque()
        root = self.findRoot(n, leftChild, rightChild)
        if root == -1:
            return False
        Q.appendleft(root)
        # Let's go through the graph with a Q
        while Q:
            # Pop index
            i = Q.pop()
            # Check if cycle 
            if i in seen:
                return False
            seen.add(i)
            # Get left and right and add to Q
            left, right = leftChild[i], rightChild[i]
            # If not empty, add
            if left != -1:
                Q.appendleft(left)
            if right != -1:
                Q.appendleft(right)
        
        # Now we should have visited every node
        if len(seen) == n:
            return True
        
        return False
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)`

