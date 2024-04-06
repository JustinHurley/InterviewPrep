---
tags:
  - easy
  - prefix_sum
  - array
---
# 303. Range Sum Query - Immutable
Link: [here]()
## Problem
Given an integer array `nums`, handle multiple queries of the following type:
1. Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.

Implement the `NumArray` class:
- `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
- `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).
## Main Idea
- Keep a prefix sum array of the sum at each index
- To get the sum between two indicies, you can take the prefix sum up to a given point, and subtract all values before the left point in the range
## Approach
- Make a prefix sum array
- Add 0 to the front in case we are looking at a range that starts with the leftmost value 
## Edge Cases/Gotchas 
- The range is inclusive, and we added a 0 to the left so for the range `[a, b]` we want to get `nums[b+1] - nums[a]`
## Solution
```python 
class NumArray:
    def __init__(self, nums: List[int]):
        prefs = [0]
        sums = 0
        for num in nums:
            sums += num
            prefs.append(sums)
        self.prefs = prefs

    def sumRange(self, left: int, right: int) -> int:
        return self.prefs[right+1] - self.prefs[left]
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
