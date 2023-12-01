---
tags:
  - dynamic_programming
  - array
  - medium
---

### 300. Longest Increasing Subsequence

Link: [here](https://leetcode.com/problems/longest-increasing-subsequence/description/)

#### Problem
Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

#### Approach
So in this problem, we need to permute the different potential subsequences as we decide to include and not include them, since there is no greedy choice we can make that would guarantee that the subsequence is as long as it can be from a given starting index. If you thought this could be solved with a monotonically increasing stack, you are wrong. Since we permute, this is a DP problem.
We need to determine the base case, which for this question is just making an array of length `n` where `n = len(nums)`, and setting every value to 1, since at the worst case, each index can at least have itself in it.
To fill the DP array, we want to start at the back and work our way to the front of the sequence, since we know for a fact that if we start at index `n-1` the best case subsequence will always be 1 since there are no elts after that.
To fill values in the sequence we need to do some checks. In the brute force solution, this would mean looking at all the different combinations of subsequences that can be made with the current elt and elts after it, but since we are precomputing these values all we have to do is look at the current element and then iterate through the DP array for each elt post the current one and ask these 2 questions: 
1. Is the current element larger than this element we are looking at?
2. Would including this subsequence starting at this position increase my optimal solution?

If both of these questions are true, we can update the DP array value. In code this looks like:
```
if nums[i] < nums[j]:
    dp[i] = max(dp[i], dp[j]+1)
```
Which exists in the inner part of loop that goes through all the elts in the DP array after the current one. So we continue this process to populate the DP array with the optimal values for each starting element. 
Now at the end we can't just return `dp[0]` since starting at the 0th index might not lead to the best solution so instead we return `max(dp).

#### Solution
```python 
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        n = len(nums)
        # Init the dp array to 1 since each LIS is worst case len 1
        dp = [1]*n

        # Iterate backwards starting at base case where there is only 1 elt
        for i in range(n-1, -1, -1):
            # We need to check this curr val against all other already calced subsequences
            for j in range(i, n):
                # Only should update iff nums[i] < nums[j]
                if nums[i] < nums[j]:
                    dp[i] = max(dp[i], dp[j]+1)
        # Since we calced the values for all starting positions, we return the max 
        return max(dp)
```

#### Time Complexity
`O(n^2)`

#### Space Complexity
`O(n)`

