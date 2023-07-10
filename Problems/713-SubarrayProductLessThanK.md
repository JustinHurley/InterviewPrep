---
tags:
- sliding_window
- math
- array
---

### 713 Subarray Product Less than K
Link: [here](https://leetcode.com/problems/subarray-product-less-than-k/)

Given an array of integers and a value `k` find the number of continuous subarrays where the product is `< k`.

The approach for this problem is to use a sliding window. Keep a front and back pointer, once the product is `>= k` move the back up until that is no longer the case. 

The tricky part of the problem is knowing how to count subarrays without making a permutation. This can be done by thinking about what we know from the current values in our window.
Given an array like `[10,5,2,6]` our sliding window finds these valid windows:
- [10]
- [10, 5]
- [5, 2]
- [5, 2, 6]
  
What do we know about these values? Well we know that every subarray not involving the right-most integer has been counted already. This means for a subarray of length `n` there are `n` continuous subarrays that can be made:
`1...n`, `2...n`, ... , `n-1...n`, `n`
This means, to find the number of subarrays you can just add the size of the subarray to a count to keep track.

The leap needed to solve this problem is to think about how many new subarrays we get on each iteration, and focusing on the right-most node.

Solution
```
class Solution:
    def numSubarrayProductLessThanK(self, nums: List[int], k: int) -> int:
        # Edge case for when there must be no values that work
        if k <= 1: 
            return 0
        # Set prod to 1 so we can multiply it by stuff
        prod = 1
        ans = left = 0
        # Iterate through nums
        for right, val in enumerate(nums):
            # Multiply the current value by the product
            prod *= val
            # While the product is larget than k, we want to move left up
            while prod >= k:
                # So we remove the left val by dividing prod by that number
                prod /= nums[left]
                # We then actually move the left index up by 1
                left += 1
            # When we know that prod < k, we can take the count of items in the window 
            ans += right - left + 1
        return ans
```
