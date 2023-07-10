---
tags:
- dynamic_programming
- array
- greedy_choice
- divide_and_conquer 
---

### 53. Maximum Subarray

Link: [here](https://leetcode.com/problems/maximum-subarray/)

#### Problem
Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

A subarray is a contiguous part of an array.

#### Approach
When you see a problem that is looking for a global maximum or minimum, it is typically a DP problem.
In this case it's a DP-lite problem as we don't even need to make an array to solve this. 
The first thing to think about when dealing with a dynamic programming problem is the choice that is being made. In this case, there is the choice to add the elt to the list or you could start over from that elt.
So we just keep track of the current sum, compare it to the max sum, and for each elt in `nums`, we use: `currSum = max(currSum+num,num)` where `currSum + num` is if we continue building the subarray, and `num` is when we elect to start over. 
When setting the initial values, you can just set them to `nums[0]` and then start from `nums[1]` since that would happen anyway if we started from `nums[0]`.

**Greedy Approach**

We can use a greedy approach to solve this problem. This is done by keeping a running total of the sum so far, and deciding if we want to keep the current sum, or to start over and make the subarray starting on the current elt. 
The way to determine this is to just see if the current sum is positive or negative. If it's positive, that means that the subarray up to now is beneficial to the answer, i.e. we should keep the number of the subarray we have traversed so far in. If it's negative, we know that summing the numbers up until now is detrimental and we should not include the subarray so far and start over. 


#### Solution
```python 
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # Edge case check
        if len(nums) <= 1:
            if nums:
                return nums[0]
            else:
                return []
        
        # Set the values to the first elt as a starting point
        currSum = nums[0]
        maxSum = nums[0]
        
        # Start with 2nd elt because we already used the first one on the sums
        for num in nums[1:]:
            currSum = max(currSum+num,num)
            maxSum = max(currSum, maxSum)        
        
        return maxSum
```
**Greedy Solution**
```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        ans = nums[0]
        currSum = 0
        for num in nums:
            # Update the currSum
            currSum += num
            # Compare to best
            ans = max(ans, currSum)
            # If negative, reset else continue
            if currSum < 0:
                currSum = 0
        return ans
```