---
tags:
  - two_pointer
  - array
  - medium
---

### 167. Two Sum II - Input Array is Sorted

Link: [here](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/)

#### Problem
Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return the indices of the two numbers, `index1` and `index2`, added by one as an integer array `[index1, index2]` of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

Your solution must use only constant extra space.

#### Approach
Given that the array is sorted, we should take advantage of this. I immediately thought binary search, but for this situation that approach would only yield an `O(nlog(n))` solution, when we can get a `O(n)` solution instead by using two pointers.
We just start with one pointer on either end of the sorted array and look at the sum. If the sum is larger than the target, we move the right pointer left by 1, reducing the sum, and if the sum is less than the target, we move the left pointer right by 1, increasing the sum. We do this until we hit the answer (answer is guaranteed to exist for this problem). In the case that it didn't, our two pointer while loop would stop when `l > r` and we could return `False` or `[-1,-1]` or something.

#### Solution
```python 
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        length = len(numbers)
        l = 0
        r = length-1
        # Do 2 pointer on the set since sorted
        while l <= r:
            currSum = numbers[l] + numbers[r]
            if currSum == target:
                return [l+1,r+1]
            elif currSum < target:
                l += 1
            else:
                r -= 1

        return [-1,-1]
```