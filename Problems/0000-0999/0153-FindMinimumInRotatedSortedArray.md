---
tags:
  - array
  - binary_search
  - medium
---

### 153. Find Minimum In Rotated Sorted Array

Link: [here](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/)

#### Problem
Suppose an array of length `n` sorted in ascending order is rotated between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

`[4,5,6,7,0,1,2]` if it was rotated 4 times.
`[0,1,2,4,5,6,7]` if it was rotated 7 times.
Notice that rotating an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in `O(log n)` time.

#### Approach
We see that this array is sorted, just rotated a bit. Whenever we see sorted we should start thinking about if some binary search is possible. In this is case it is. The goal of this problem is to find the minimum element of the array, which we know will be at the 0th index of the array, pre-rotation. So if we can find the point of rotation, then we can find the minimum element of the array. How do we know where that starting point is? Well, we can look at scenarios where the right element is less than the left element, this tells us that the rotation point must be somewhere in-between those values, otherwise the array would be sorted and the left value would be less than the right value.
So we set up binary search using left and right pointers like normal, however our evaluation criteria will be different. In the while loop, we look and see if the left value is less than the right value, if we see this, then we know we are in the sorted part of the array, and can just use the leftmost value and compare it to the global min. If that condition isn't true, we want to find the half of the array that has the rotation point. To do this we compare our left value to the midpoint value. If left > mid, then we know that the rotation point must be in the left side of the array and move up the right.
It is important to check the midpoint value as we are doing this problem, as we could potentially set the midpoint value equal to the minimum value in the array, and then would not touch it again as the left and right values would skip over it as we make the problem space smaller and smaller. 

#### Solution
```python 
class Solution:
    def findMin(self, nums: List[int]) -> int:
        n = len(nums)

        l, r = 0, n-1

        ans = nums[l]

        while l <= r:
            if nums[l] < nums[r]:
                ans = min(nums[l], ans)
                break

            mid = (l+r)//2
            ans = min(nums[mid], ans)
            if nums[l] > nums[mid]:
                r = mid-1
            else:
                l = mid+1

        return ans
```

#### Time Complexity
`O(log(n))` as we do binary search on `n` elements.
