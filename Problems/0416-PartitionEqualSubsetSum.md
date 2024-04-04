---
tags:
  - medium
  - dynamic_programming
  - set
---
# 416. Partition Equal Subset Sum
Link: [here](https://leetcode.com/problems/partition-equal-subset-sum/description/)
## Problem
Given an integer array `nums`, return `true` _if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or_ `false` _otherwise_.
## Main Idea
- Go through the array and make all possible sum combinations
- If you were able to make the target sum, then you should return true
- The dynamic programming part of the question involves making the choice to either include or not include an element in the current sum
## Approach
- Find the total sum of the input
- Check to see if it is odd
- If not then find the target sum
- Now go through every element in `nums`
- For each element we want to go through the existing set of sums and then add the current value to each existing set element 
- If the target exists in the set, it means we have a valid input and return `True`
## Edge Cases/Gotchas 
- Handling the even case saves time
- Returning `False` as soon as you find the target sum saves time
- You can't edit a set while you are iterating through it
## Solution
```python 
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        total = sum(nums)
        # If odd, impossible to split evenly 
        if total % 2 == 1:
            return False
        target = total//2
        sums = {0}
        # Build set of all possible sums
        for num in nums:
            temp = set()
            for s in sums:
                temp.add(num + s)
            sums = sums | temp
        # If target was summed, means it is possible
        return target in sums
```
## Time Complexity
$O(k)$ where `k` is related to the sum of the input array, this is because the larger the range of sums, the more sums that will need to be checked
## Space Complexity
$O(k)$ since we store all possible sums 
