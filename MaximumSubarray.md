### 53. Maximum Subarray

Link: [here](https://leetcode.com/problems/maximum-subarray/)

#### Topics
- Dynamic programming
- Arrays
- Greedy choice
- Divide and conquer (different approach)

#### Problem
Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

A subarray is a contiguous part of an array.

#### Approach
When you see a problem that is looking for a global maximum or minimum, it is typically a DP problem.
In this case it's a DP-lite problem as we don't even need to make an array to solve this. 
The first thing to think about when dealing with a dynamic programming problem is the choice that is being made. In this case, there is the choice to add the elt to the list or you could start over from that elt.
So we just keep track of the current sum, compare it to the max sum, and for each elt in `nums`, we use: `currSum = max(currSum+num,num)` where `currSum + num` is if we continue building the subarray, and `num` is when we elect to start over. 
When setting the initial values, you can just set them to `nums[0]` and then start from `nums[1]` since that would happen anyway if we started from `nums[0]`.

#### Solution
```
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