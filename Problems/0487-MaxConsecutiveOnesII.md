---
tags:
  - medium
  - array
---
# 487. Max Consecutive Ones II
Link: [here](https://leetcode.com/problems/max-consecutive-ones-ii/description/)
## Problem
Given a binary array `nums`, return _the maximum number of consecutive_ `1`_'s in the array if you can flip at most one_ `0`.
## Intuition
- The highest number of consecutive ones can look like `1,1,...,0,...,1` or just `111..`. This means we can generalize consecutive ones that allows a zero as the combination of two consecutive sums of one potentially plus a zero
- Keep track of the last 2 subsets of 1s, and when you see a zero, "shift" the sets over to acommodate the creation of the new set
## Approach
- See above
## Edge Cases/Pitfalls
- You need to account for the existence of the zero in the subset since it counts as a 1 in this problem (we can't just ignore it)
## Solution
```python 
class Solution:
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        first = 0
        second = 0
        seen_zero = 0
        best = 0
        for num in nums:
            # If we see a zero, update sections
            if num == 0:
                seen_zero = 1
                second = first
                first = 0
            # Otherwise keep building first
            else:
                first += 1
            # Update best if necessary 
            best = max(best, first+second+seen_zero)
        return best 
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
## Takeaways 
- See if this problem is made of easier problems 
