### 34. Find First and Last Position of Element In Sorted Array
Link: [here](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

This problem is effectively asking us to do binary search when there is more than one target value.

Implementation is very similar to normal binary search, the only catch is once you find the target value, you need to use 2 pointers to find the start and end of the item.

####Things to Know for This Problem:
- Binary search
- Two Pointer
- Finding midpoint
- While loops

```
# Given array of integers sorted in non-decreasing order
# Want to find the starting and ending position of the target value
# Can do binary search to find the target, then 2 pointers to find the start and end

class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        # Instantiate left and right
        l, r = 0, len(nums)-1
        while(l <= r):
            # Establish a midpoint
            mid = (l+r)//2
            # If the midpoint is equal to the target, we need to look for the start and end positions
            if nums[mid] == target:
                left = mid
                right = mid
                # Find the leftmost end
                while(left > 0 and nums[left-1] == target):
                    left -= 1
                # Find the rightmost end
                while(right < len(nums)-1 and nums[right+1] == target):
                    right += 1
                return [left, right]
            # Need to go right
            elif nums[mid] < target:
                l = mid+1
            else:
                r = mid-1
        return [-1,-1]
```