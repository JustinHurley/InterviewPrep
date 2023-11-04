---
tags: [heap, array, sorting]
problem: [medium]
---
### 215. Kth Largest Element in an Array

Link: [here](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)

#### Problem
Given an integer array `nums` and an integer `k`, return _the_ `kth` _largest element in the array_.
Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.
Can you solve it without sorting?

#### Approach
So the non-optimal but close-enough approach is to use a heap, which is typically the data structure to use whenever you see "kth largest." For this problem all we need to do is `heapify`, which costs `O(n)` time, and then. pop every element until we have `k` elements remaining in the heap, and then pop the `k`th item in the heap, which because python uses min-heaps, is the kth largest value. 

#### Edge Cases
The LC problem has no edge cases, but there could be times when `k > len(nums)` or when there are cases with duplicate values which could cause problems (not in the heap approach). Also empty array handling wasn't mentioned in the problem. 

#### Solution
```python 
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        # Heapify nums
        heapq.heapify(nums)
        # Pop every item in heap larger than kth item
        while len(nums) > k:
            heapq.heappop(nums)
        # Pop the kth item
        return heapq.heappop(nums)
```

#### Time Complexity
`O(n * log(k))` since we are popping at most `n` elements and each pop costs `log(k)` time. 

#### Space Complexity
`O(k)` since the heap will store `O(k)` items, but you could argue in Python since the heap uses the `nums` array then there is no extra space complexity. 

