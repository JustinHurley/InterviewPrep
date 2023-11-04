---
tags: [array, dynamic_programming, dictionary]
---

### 1027. Longest Arithmetic Subsequence

Link: [here](https://leetcode.com/problems/longest-arithmetic-subsequence/)

#### Problem
Given an array `nums` of integers, return the length of the longest arithmetic subsequence in `nums`.

Note that:

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.
A sequence seq is arithmetic if `seq[i + 1] - seq[i]` are all the same value (for `0 <= i < seq.length - 1`).

#### Approach
So for this problem, we can see that there are many potential permutations of the input to get to the solution. Not finding a greedy response, it seems that this is a DP problem. So with that in mind, what is the subproblem? 
In this case, it's dealing with a smaller array. Like all DP problems, we want to rely on previously computed work to help us build our solution. So to build our solution, we want to make a dictionary that stores all the lengths of arithmetic subsequences of inputs up until now. We do this by making a dictionary that stores the index of the ending point of the subsequence and the arithmetic difference as the key, and use the count as the value. So the way we would fill `nums[i]` would be to look at all the values that occur before `nums[i]`, take the difference between `nums[i]` and one of the previous values, see if there is a corresponding arithmetic sequence found, and then update it. If no sequence is found, we just add 1 for that value, since any 2 numbers are technically an arithmetic sequence. 
The important line of the problem is this:
```
 dp[(right, nums[right]-nums[left])] = dp.get((left, nums[right] - nums[left]), 1) + 1
```
This is the code that will get the subsequence length for a previous value relative to the current value.

#### Solution
```python 
class Solution:
    def longestArithSeqLength(self, nums: List[int]) -> int:
        dp = {}

        # Right traverses through whole arr
        for right in range(len(nums)):
            # Left goes from start to right
            for left in range(0, right):
                # For each item, we want to set the right value for that index and diff
                # That value is based on if the left val diff is the same, and if it has
                dp[(right, nums[right]-nums[left])] = dp.get((left, nums[right] - nums[left]), 1) + 1
        
        return max(dp.values())
```

#### Time Complexity
`O(n^2)`

#### Space Complexity
`O(n^2)`

