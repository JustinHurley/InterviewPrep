---
tags: ["#easy", "#math", "#array"]
---
### 1913. Maximum Product Difference Between Two Pairs

Link: [here](https://leetcode.com/problems/maximum-product-difference-between-two-pairs/)

#### Problem
The **product difference** between two pairs `(a, b)` and `(c, d)` is defined as `(a * b) - (c * d)`.

- For example, the product difference between `(5, 6)` and `(2, 7)` is `(5 * 6) - (2 * 7) = 16`.

Given an integer array `nums`, choose four **distinct** indices `w`, `x`, `y`, and `z` such that the **product difference** between pairs `(nums[w], nums[x])` and `(nums[y], nums[z])` is **maximized**.

Return _the **maximum** such product difference_.

#### Main Idea
- Can do it in one pass by keeping track of the largest, second largest, smallest, and second smallest values

#### Approach
- See above

#### Edge Cases
- `len(nums) < 4`
- input is invalid
#### Solution
```python 
class Solution:
    def maxProductDifference(self, nums: List[int]) -> int:
        biggest = 0
        second_biggest = 0
        smallest = inf
        second_smallest = inf
        
        for num in nums:
            if num > biggest:
                second_biggest = biggest
                biggest = num
            else:
                second_biggest = max(second_biggest, num)
                
            if num < smallest:
                second_smallest = smallest
                smallest = num
            else:
                second_smallest = min(second_smallest, num)
        
        return biggest * second_biggest - smallest * second_smallest```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)`

