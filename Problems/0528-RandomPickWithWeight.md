---
tags:
  - medium
  - design
  - prefix_sum
  - binary_search
---
# 528. Random Pick with Weight
Link: [here](https://leetcode.com/problems/random-pick-with-weight/description/)
## Problem
You are given a **0-indexed** array of positive integers `w` where `w[i]` describes the **weight** of the `ith` index.

You need to implement the function `pickIndex()`, which **randomly** picks an index in the range `[0, w.length - 1]` (**inclusive**) and returns it. The **probability** of picking an index `i` is `w[i] / sum(w)`.
- For example, if `w = [1, 3]`, the probability of picking index `0` is `1 / (1 + 3) = 0.25` (i.e., `25%`), and the probability of picking index `1` is `3 / (1 + 3) = 0.75` (i.e., `75%`).
## Main Idea
- Get the weight adjusted total of all the weights
- Use prefix sum so we know what the sum is at each cell 
- Use binary search with the prefix sums to get an index
## Approach
- For the init method we want to make an array that's the prefix sum of up to each element
- We also want to set the total of all the weights
- If we want to get a random index, we can generate a random number from 0 to total, and then use binary search to find the value in the array where the prefix sum target is greater than or equal to the current value, but less than the next one
## Edge Cases/Gotchas 
- N/A
## Solution
```python 
class Solution:
    def __init__(self, w: List[int]):
        # Use prefix sum
        prefs = []
        pref = 0
        for weight in w:
            pref += weight
            prefs.append(pref)
        self.prefs = prefs
        self.total = pref

    
    def pickIndex(self) -> int:
        # Generate number from total
        target = random.random() * self.total
        left, right = 0, len(self.prefs)-1
        # Binary search
        while left < right:
            mid = (left+right)//2
            if self.prefs[mid] < target:
                left = mid+1
            else:
                right = mid
        return left
```
## Time Complexity
$O(n)$ to make $O(log(n))$ to pick
## Space Complexity
$O(n)$ for data structure 
