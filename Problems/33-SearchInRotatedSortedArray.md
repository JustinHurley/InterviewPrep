### 33. Search in Rotated Sorted Array

Link: [here](https://leetcode.com/problems/search-in-rotated-sorted-array/description/)

#### Topics
- Array
- Binary Search

#### Problem
There is an integer array `nums` sorted in ascending order (with distinct values).

Prior to being passed to your function, `nums` is possibly rotated at an unknown pivot index `k` `(1 <= k < nums.length)` such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (0-indexed). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array nums after the possible rotation and an integer `target`, return the index of target if it is in nums, or `-1` if it is not in `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

#### Approach
The general approach to this problem relies on the fact that we can determine information about the arrangement of the entire array based on the values of the left, right and middle pointers. When we see "sorted" we automatically think binary search and although this array is rotated, we can still leverage the fact that it's sorted to see if our value exists in there, we just need to be careful and handle all the cases that can occur when we are evaluating which side of the array to look at in each binary search iteration.
Consider the array `[4,5,6,7,0,1,2]`, while the entire array isn't sorted, we can clearly see that there are 2 sorted halves of the array that are joined. This means that within the sorted portions of the array, we can simply use binary search the same way we always do. So using the same example array, consider when `nums[mid] == 6` and `nums[l] == 4` and `nums[r] == 2`. We can immediately determine which half of the array is sorted, and which half of the array contains the pivot point. In this case, the left half of the array is the sorted half. So we know that if `nums[l] <= target < nums[mid]` then if the target exists, it will be on the left side of the array. Otherwise it will be on the right side of the array. We can keep using this logic until we either find the target, or we end iterating and return `-1`.
The main idea of this solution is that we leverage the sorted portions of the array to do binary search.

#### Solution
```
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        n = len(nums)
        l, r = 0, n-1

        while l <= r:
            mid = (l+r)//2

            # If we have target we're done
            if nums[mid] == target:
                return mid
            # If we are looking at left and it's sorted
            if nums[l] <= nums[mid]:
                # If the target is outside of the left sorted portion, look right
                if target > nums[mid] or target < nums[l]:
                    l = mid+1
                # Otherwise keep looking left
                else:
                    r = mid-1
            # Else we are looking at right sorted portion
            else:
                # If the target is outside of the right sorted portion, look left
                if target < nums[mid] or target > nums[r]:
                    r = mid-1
                # Otherwise keep looking right
                else: 
                    l = mid+1
        return -1
```

#### Time Complexity
`O(log(n))`, there are `n` elements and we divide the problem space by 2 each time.