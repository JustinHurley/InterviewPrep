---
tags: [binary_tree, recursion, string, depth_first_search, tree]
---

### 297. Serialize and Deserialize Binary Tree

Link: [here](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description/)

#### Problem
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

Clarification: The input/output format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

#### Approach
For this problem, we are going to use preorder traversals to serialize the binary tree. Preorder traversal just means we first process the node, then look left, process the left subtree, then process the right subtree. 
One perk of doing it this way is that we will ensure that the first node in the serialization corresponds to the value of the first node in the tree.
**Serialization**
To serialize, we just make an empty array, and then do DFS on the tree, adding values to the array as we traverse the tree. The only different thing is that if we hit a `None` node, we add `N` to the array, as this is something we want to keep track of. Finally we convert the list to a string using `','.join(list)`. Also make sure that when you're appending, that you cast the numbers to strings.
**Deserialization**
To deserialize, we basically run DFS but on the string we generated during serialization. To do this, we will need to first convert the string to a list, using `data.split(',')`. Then, we will use a helper function to recursively iterate through the built list. Our base case is when the value at the current pointer equals `N`, in which case we know we are at a null value and thus the end of a branch of the tree so should return `None`, and also increment the pointer to the next item in the list. The general case will be to add the value to a node, increment the pointer by 1, and then set `node.left` and `node.right` by calling DFS. 
It's important to use `self.i` as a global pointer in this problem, as we want to iterate over the list we built from the string as we are doing the recursion. 

#### Solution
```python 
class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        res = []
        def dfs(node):
            if not node:
                res.append('N')
                return 
            res.append(str(node.val))
            dfs(node.left)
            dfs(node.right)
        dfs(root)
        return ','.join(res)

            
    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        self.i = 0
        vals = data.split(',')
        def dfs():
            if vals[self.i] == 'N':
                self.i += 1
                return None
            node = TreeNode(int(vals[self.i]))
            self.i += 1
            node.left = dfs()
            node.right = dfs()
            return node
        return dfs()
```

#### Time Complexity
`O(n)` for both operations.

#### Space Complexity
`O(n)` for both operations.

