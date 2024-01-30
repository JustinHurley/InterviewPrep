---
tags: [medium, design, graph, depth_first_search]
---
# 2049. Count Nodes with the Highest Score

Link: [here](https://leetcode.com/problems/count-nodes-with-the-highest-score/description/)

## Problem
There is a **binary** tree rooted at `0` consisting of `n` nodes. The nodes are labeled from `0` to `n - 1`. You are given a **0-indexed** integer array `parents` representing the tree, where `parents[i]` is the parent of node `i`. Since node `0` is the root, `parents[0] == -1`.

Each node has a **score**. To find the score of a node, consider if the node and the edges connected to it were **removed**. The tree would become one or more **non-empty** subtrees. The **size** of a subtree is the number of the nodes in it. The **score** of the node is the **product of the sizes** of all those subtrees.

Return _the **number** of nodes that have the **highest score**_.

## Main Idea
- We can use the `parents` input to build a graph of nodes
- We can then DFS on the tree and determine the score for each node
- Then get the frequency of times the high score was reached 

## Approach
- Make a `Node` object with a value, and left and right children
- Then go through `parents` and build the graph, we will set child nodes on the next pass
- Now set child nodes when applicable
- Now that the graph is done, it's time to do DFS
- If our node is null then we return 0
- Otherwise we make recursive calls to try and get the sizes of the left and right subtrees
- Then to get the size of the tree "above" our current nodes, we just take the total size of the tree, and subtract the sizes of the left and right subtrees, and the subtract one more to account for the removal of the current node
- Finally we multiply the 3 values together, changing 0s to 1s to not set the score to 0
- Then we save that value in our scores dictionary
- We then return the frequency of the high score

## Edge Cases/Gotchas 
- It's very helpful to convert the `parents` list into an actual graph
- There could have been invalid inputs 

## Solution
```python 
class Node:
    def __init__(self, val, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
        
class Solution:
    def countHighestScoreNodes(self, parents: List[int]) -> int:
        n = len(parents)
        graph = {}
        scores = {}
        # Init nodes
        for i in range(n):
            graph[i] = Node(i)
        # Add children to nodes
        for i in range(1, n):
            # i has parent parents[i]
            parent = parents[i]
            if not graph[parent].left:
                graph[parent].left = graph[i]
            else:
                graph[parent].right = graph[i]
        # Now dfs on nodes, this just gets size
        def dfs(root):
            if not root:
                return 0
            leftSize = dfs(root.left)
            rightSize = dfs(root.right)
            rest = n - leftSize - rightSize - 1
            # or 1 so we don't multiply by 0
            scores[root.val] = (leftSize or 1) * (rightSize or 1) * (rest or 1)
            return leftSize + rightSize + 1
        dfs(graph[0])
        # Get the high score
        highScore = max(scores.values())
        # Return the num of high scores
        return Counter(scores.values())[highScore]
```

## Time Complexity
`O(n)` we do many passes but only a constant number

## Space Complexity
`O(n)`
