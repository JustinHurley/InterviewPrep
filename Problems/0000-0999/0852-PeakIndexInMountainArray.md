---
tags:
  - array
  - binary_search
  - medium
---
### 852. Peak Index in Mountain Array

Link: [here](https://leetcode.com/problems/peak-index-in-a-mountain-array/description/)

#### Problem
An array `arr` a **mountain** if the following properties hold:
- `arr.length >= 3`
- There exists some `i` with `0 < i < arr.length - 1` such that:
    - `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
    - `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`

Given a mountain array `arr`, return the index `i` such that `arr[0] < arr[1] < ... < arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`.

You must solve it in `O(log(arr.length))` time complexity.

#### Approach
Given the time constraint, we should immediately know that this is a [[BinarySearch|binary search]] problem, since there really aren't many other ways to get that kind of time-complexity. With that in mind, we just implement binary search on the array. Checking a few things on each iteration:
1. If the numbers to the left and right of the midpoint are less than the midpoint, you have found your peak.
2. Else, if only the left side is less than the right side, the peak must be in the right side of the array.
3. Else, the left side must be more than the right, so we should look for the peak in the left side of the array.
The above process continues until the peak is found.

#### Edge Cases
This problem didn't have any edge cases, but all arrays less than 3 would be invalid, and you could also get invalid arrays that had no peak. We also did not need to deal with duplicates. 

#### Solution
```python 
class Solution:
    def peakIndexInMountainArray(self, arr: List[int]) -> int:
        n = len(arr)
        
        # Need to iterate through arr
        left, right = 0, n-1

        while left <= right:
            # Take midpoint
            mid = (left + right)//2
            # Now check if peak
            if mid != 0 and mid != n-1 and arr[mid-1] < arr[mid] and arr[mid] > arr[mid+1]:
                return mid
            # If it's not the peak, but the mid is > the num to the left, look right
            if mid != 0 and arr[mid-1] < arr[mid]:
                left = mid+1
            # Otherwise look left
            else:
                right = mid-1
        return 1
```

#### Time Complexity
`O(log(n))`

#### Space Complexity
`O(1)`

