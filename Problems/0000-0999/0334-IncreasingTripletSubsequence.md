---
tags:
  - medium
  - array
  - greedy
---
# 334. Increasing Triplet Subsequence
Link: [here](https://leetcode.com/problems/increasing-triplet-subsequence/description/)
## Problem
Given an integer array `nums`, return `true` _if there exists a triple of indices_ `(i, j, k)` _such that_ `i < j < k` _and_ `nums[i] < nums[j] < nums[k]`. If no such indices exists, return `false`.
## Intuition
- Checking every combo would take $O(n^3)$ so there has to be a better way
- Finding 2 elements in order is easy, how can we do 3?
- We can greedily keep track of the smallest and second smallest valid values as we scan the array, and keep going until we see one
- Intuition is pretty hard for this one honestly
- The "trick" of the problem is that we can greedily overwrite the 1st and 2nd smallest values, since once we find a second value, any other value that is larger than the second value makes a valid answer.
- Consider `[1,2,0,3]` where we would end up originally having 1 be smallest and 2 be second smallest. When we introduce 0, it replaces the first smallest but we still preserve that we have a second smallest value that has a first smallest value so when we see 3, it is still larger than 2, and so by resetting the smallest value, we aren't "restarting" our triplet finding process, but instead we are lowering the threshold for smaller values. Basically making the smallest or second smallest values smaller won't have an impact on finding larger values later on in the array since they'll still be larger
## Approach
- We want to keep track of the smallest and second smallest values in the array as we move down
- If the number is smaller than the smallest, update it
- If the number is just smaller than the second smallest, update it
- If the number is larger than the smallest and second smallest, we found an answer
- If we can pass through the entire array and not find an answer, then return `False`
## Edge Cases/Pitfalls
- We need to replace first and second for `<=` since having numbers of equal value doesn't count as a valid triplet
- Since we're going for smallest we need to use `float('inf')`
## Solution
```python 
class Solution:
    def increasingTriplet(self, nums: List[int]) -> bool:
        first = second = float('inf')
        for num in nums:
            if num <= first:
                first = num
            elif num <= second:
                second = num
            else:
                return True
        return False
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
## Takeaways 
- When trying to find a subsequence that stays in order, we don't really care what each value is as long as it is valid