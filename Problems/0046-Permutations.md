---
tags:
  - backtracking
  - depth_first_search
  - medium
---
### 46. Permutations

Link: [here](https://leetcode.com/problems/permutations/description/)

#### Problem
Given an array `nums` of distinct integers, return _all the possible permutations_. You can return the answer in **any order**.

#### Main Idea
- Choose an element to be static, then permute on the rest
- Use backtracking to save space on subarrays and working arrays

#### Approach
- Use recursion to visit each case of the decision tree (choosing `ith` element as 1st position, etc.)
- Our base case is when the array is size 1, since the permutations would just be the one element array
- Otherwise, we should run a for loop `len(nums)` times, where we pop the 0th element, then permute on the sub-problem, and then finally once we have permuted on the subproblem, append the original number to all the permutations done on the sub-array
- Finally, we should put back the element we removed, which we can do by putting the first element at the end of the `nums` array
- The reason that we put the element we took from the front of the array to the back, is so that we then have a new first element for the next iteration, that we can the permute on the new sub-problem for.

#### Edge Cases
- `len(nums) == 0` but not an issue in the problem
- Non-unique elements not an issue in the problem but should be considered

#### Solution
```python 
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        ans = []
        if len(nums) == 1:
            return [nums[:]]

        for _ in range(len(nums)):
            # Pop 0th elt
            n = nums.pop(0)
            # Try to permute on the rest of the arr
            perms = self.permute(nums)
            # Now add elt 
            for perm in perms:
                perm.append(n)
            ans += perms
            # Append to the back for a new n
            nums.append(n)
        return ans
```

#### Time Complexity
`O(2^n)`

#### Space Complexity
`O(n)` for stack space

