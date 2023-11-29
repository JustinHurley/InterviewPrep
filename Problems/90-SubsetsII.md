---
tags: [backtracking, recursion, depth_first_search, array]
---
### 90. Subsets II

Link: [here]()

#### Problem
Given an integer array `nums` that may contain duplicates, return _all possible_ _subsets_ _(the power set)_.
The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

#### Main Idea
- DFS on the decision tree to either add a number, or to not include it at all, and we use [[Algorithms/Backtracking|backtracking]] to explore all scenario permutations
- If we aren't including a number, advance the array until you're past it (assuming array is sorted)
- We choose to skip by number instead of value to avoid duplicates, the decision path where the first 2 was added, will also contain the decision path where the second 2 is added.

#### Approach
- First we need to sort the array, this will make it much easier to know when we are dealing with a number that has duplicates. 
- Then we want to make our recursive backtracking function
- The base case is if `i` (our index-parameter) is greater or equal to the length of the desired array, stop as we have made enough decisions to consider our subset "done" so we add it to the global answer set
- Otherwise we append the current `nums[i]` value to the subset, and continue to recur.
- Then the backtracking comes in, where we pop the element we just added, and continue to recur to cover the case where we choose not to add the element to our current subset
#### Edge Cases
- `nums` contains invalid elements
#### Solution
```python 
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        ans = []
        nums.sort()

        def backtrack(i, subset):
            if i >= len(nums):
                ans.append(subset[:])
                return
            # Include the number
            subset.append(nums[i])
            backtrack(i+1, subset)
            # Don't include the number
            curr = subset.pop()
            while i < len(nums) and nums[i] == curr:
                i += 1
            backtrack(i, subset)
        backtrack(0, [])

        return ans```

#### Time Complexity
`O(n*2^n)` the leading `n` is because making the shallow copy takes `n` time and we do it for each subset. 

#### Space Complexity
`O(n)` as the call stack can be at worst-case, length `n` for the recursive calls

