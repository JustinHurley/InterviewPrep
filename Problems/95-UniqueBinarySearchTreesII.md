---
tags:
  - tree
  - recursion
  - dynamic_programming
  - backtracking
  - binary_search_tree
  - medium
---
### 95. Unique Binary Search Trees II

Link: [here](https://leetcode.com/problems/unique-binary-search-trees-ii/description/)

#### Problem
Given an integer `n`, return _all the structurally unique **BST** 's (binary search trees), which has exactly_ `n` _nodes of unique values from_ `1` _to_ `n`. Return the answer in **any order**.

#### Approach
The basic approach to this problem involves breaking the problem down into smaller subproblems, using the fact that we are making a BST with values `[1,n]` . For example, if we chose `i` as the root, it would mean that the left and right subtrees are made up of all the values `[1,i-1]` and `[i+1,n]` respectively. So we recursively break down the problem into left and right sides, until the left and right pointers overlapped, in which case there are no values left to add and we can just return `None`. In the non-base case however, we want to consider all the remaining values as the potential root value of this subtree, and the values for the left and right recursive calls as the potential children.

#### Edge Cases
There really aren't any edge cases to handle, since we know we will get a number between 1 and 8. 

#### Solution
```python 
class Solution:
    def generateTrees(self, n: int) -> List[Optional[TreeNode]]:
        memo = {}

        # Here we use a helper method to create all possible BST in a given number range
        def allPossibleBST(start, end):
            ans = []
            # If start > end, then there is no range to look at
            if start > end:
                ans.append(None)
                return ans
            # If the range is already computed, return it
            if (start, end) in memo:
                return memo[(start, end)]
            # Otherwise we need to iterate on all next nodes as the root
            for i in range(start, end + 1):
                # Since it's a BST, we split in half
                leftSubTrees = allPossibleBST(start, i - 1)
                rightSubTrees = allPossibleBST(i + 1, end)
                # Now iterate through every combo of left and right subtrees
                for left in leftSubTrees:
                    for right in rightSubTrees:
                        root = TreeNode(i, left, right)
                        ans.append(root)
            # Add the now calculated range to the memo dict
            memo[(start, end)] = ans
            return ans
```

#### Time Complexity
`O(lol)`
If you get asked this on an interview, unless it's like for Staff at Citadel, you should not be expected to provide time complexity. Asking for an upper bound is reasonable though, which we can say is `O(2^n)`

#### Space Complexity
`O(lol)`

