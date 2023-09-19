---
tags:
- binary_search
- array
- divide_and_conquer
---
### 4. Median of Two Sorted Arrays

Link: [here](https://leetcode.com/problems/median-of-two-sorted-arrays/description/)

#### Problem
Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log(m+n))`.

#### Approach
This is a fairly trivial problem in general, because you could just merge the arrays in `O(m+n)` time and then do binary search in `O(log(m+n))` time. The question specifically asks for logarithmic performance however which very likely means we will need some variation of [[BinarySearch|binary search]] to solve this problem, which is what makes it a hard. 
This means that we will need some way to logarithmically shrink the problem space, using only `log()` time operations. We can do this by comparing the midpoints of the relative arrays, to the midpoint of the combined arrays, since that will correspond to the overall median. Then by comparing the midpoints, we can determine which array should have their half removed.

#### Important Part
The important part of this problem is knowing how to determine which array can have it's half removed. To do that, we first compare the current midpoints with the overall midpoint, which we'll call `k`.
If `mid1 + mid2 < k` it means that the overall median must not be in the smallest left side of the array, and if `mid1 + mid2 > k`, then the overall median must not be in the largest right side of the array. When it comes to determining which of the smaller halves to remove, we can just see which midpoint is smaller to determine the smaller half. 

#### Edge Cases
This is more of the base-case than an edge case, but if `l > r` for one of the arrays, meaning all the elements have been visited, you can just return the unfinished array with the index `nums1[k - l2]` since we want the kth element in the unfinished array, but to also account for the left-most elements that have been "removed" from the problem. 

We also need to account for the even length case, which we handle by making 2 calls if the length is even and averaging them.

#### Solution
```python 
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        n1 = len(nums1)
        n2 = len(nums2)
        n = n1 + n2

        def helper(k, l1, r1, l2, r2):
            # If we looked over all elts in one arr, return kth in other
            if l1 > r1:
                # Return the smallest num shifted by all elts in the other arr
                return nums2[k - l1]
            if l2 > r2:
                return nums1[k - l2]
            
            # If we didn't then we want to grab midpoints 
            mid1 = (l1+r1)//2
            mid2 = (l2+r2)//2
            midVal1 = nums1[mid1]
            midVal2 = nums2[mid2]

            # If the midpoint indicies are less than k, we should cut the smaller half
            if mid1 + mid2 < k:
                # Cut whichever arr has the smaller half
                if midVal1 < midVal2:
                    return helper(k, mid1+1, r1, l2, r2)
                else:
                    return helper(k, l1, r1, mid2+1, r2)
            # Otherwise we need to cut the bigger half 
            else:
                # Cut whichever arr has the larger half
                if midVal1 < midVal2:
                    return helper(k, l1, r1, l2, mid2-1)
                else:
                    return helper(k, l1, mid1-1, l2, r2)
        
        # If odd just return middle
        if n % 2:
            return helper(n//2, 0, n1-1, 0, n2-1)
        # Otherwise need to take the average of the mid-left and mid-right elements 
        else:
            return (helper(n//2 - 1, 0, n1-1, 0, n2-1) + helper(n//2, 0, n1-1, 0, n2-1))/2
```

#### Time Complexity
`O(log(m+n))`

#### Space Complexity
`O(log(m) + log(n))`

