---
tags:
  - hard
  - array
  - in_place
---
# 41. First Missing Positive
Link: [here](https://leetcode.com/problems/first-missing-positive/description/)
## Problem
Given an unsorted integer array `nums`. Return the _smallest positive integer_ that is _not present_ in `nums`.

You must implement an algorithm that runs in `O(n)` time and uses `O(1)` auxiliary space.
## Intuition 
- Since they are asking for `O(n)` time and `O(1)` extra space, it is implied that you cannot sort, and can only use pointers and the space given by the input array to store extra space
- With that in mind, we can use the values in `nums` and associate that with a given index to determine how many positive integers exist from `[1, n]`. 
- Need to clean the data to use this approach, no negative numbers or zero allowed 
## Approach
- First we need to clean up the data so that we know which cells we made negative, this can be done by converting any number less than 1 to 1. To keep track of the existence of 1 as well, we will just use a boolean.
- Now we want to go through the array and see which integers are present
- If we have a `nums[i]` that is in the range `[1,n]` we should go to that given cell and make it negative if it's not already negative 
- Now we can go through `nums` and try to scan for the first non-negative value we see, which corresponds to the positive integer we need
## Edge Cases/Gotchas 
- Since we are using an array indexed from `[0, n-1]` we need to adjust when looking at a num's relative index, we want the index of `nums[num-1]` since 1 should index to 0
- If we don't have a 1 in the array, we can immediately return 1 since that's the smallest positive integer
- If all the elements in the array are negative, it means the smallest positive integer is `n+1`.
## Solution
```python 
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        # Clean nums
        hasOne = False
        for i in range(len(nums)):
            if nums[i] == 1:
                hasOne = True
            elif nums[i] < 1:
                nums[i] = 1
        if not hasOne:
            return 1
        # Now we can go through and mark array
        for i, num in enumerate(nums):
            num = abs(num)
            # If num is in bounds, make neg
            if num <= len(nums) and nums[num-1] > 0:
                nums[num-1] = -nums[num-1]
        # Now find smallest positive 
        for i, num in enumerate(nums):
            if num > 0:
                return i+1
        return len(nums)+1
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$ if you don't include using the array for space, otherwise $O(n)$.
## Takeaways
- You can use the indicies of an array as a very simple way to hash positive integers, storing a boolean by flipping the sign of an index to indicate true or false for that value
- If the input data isn't good enough to do what you want, clean the input data
