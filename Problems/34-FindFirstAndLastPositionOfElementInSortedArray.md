---
tags:
- binary_search
- two_pointer
---

### 34. Find First and Last Position of Element In Sorted Array
Link: [here](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

#### Problem
Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.
If target is not found in the array, return [-1, -1].
You must write an algorithm with O(log n) runtime complexity.

#### Main Idea
Sorted array means binary search, we can do 2 binary searches to find each endpoint.

#### Approach
We just run binary search twice, looking for the target element on the leftmost and rightmost edges. The main difference between this and just running typical binary search, is that our match condition is that the value has to match `target`, but it also must be different than the value next to it, to ensure it is one of the edges.

#### Edge Cases
- Array is empty
- Array is all the same number
- Array doesn't have `target`

#### Solution
```python 
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        left = self.findLeft(nums, target)
        right = self.findRight(nums, target)
        return [left, right]
    
    def findLeft(self, nums, target):
        left, right = 0, len(nums)-1
        while left <= right:
            mid = (left+right)//2
            # Found target 
            if (mid == 0 or nums[mid] > nums[mid-1]) and nums[mid] == target:
                return mid
            # Check if we need to look right
            if nums[mid] < target:
                left = mid+1
            else:
                right = mid-1
        return -1

    def findRight(self, nums, target):
        left, right = 0, len(nums)-1
        while left <= right:
            mid = (left+right)//2
            # Found target 
            if (mid == len(nums)-1 or nums[mid] < nums[mid+1]) and nums[mid] == target:
                return mid
            # Check if we need to look left
            if nums[mid] > target:
                right = mid-1
            else:
                left = mid+1
        return -1
```

#### Time Complexity
`O(log(n))`

#### Space Complexity
`O(1)`
