---
tags:
  - array
  - two_pointer
  - sorting
  - easy
---
### 905. Sort Array by Parity

Link: [here](https://leetcode.com/problems/sort-array-by-parity/description/)

#### Problem
Given an integer array `nums`, move all the even integers at the beginning of the array followed by all the odd integers.

Return _**any array** that satisfies this condition_.

#### Main Idea
You can sort it in place using a fast and slow pointer to swap indicies. 

#### Approach
Make a fast and slow pointer, and iterate over `nums` from left to right. When you see an odd number, move the fast pointer until you find an even number or hit the end of the array.

#### Edge Cases
- `len(nums) < 2` 

#### Solution
```python 
class Solution:
    def sortArrayByParity(self, nums: List[int]) -> List[int]:
        if len(nums) < 2:
            return nums

        slow, fast = 0, 1

        while slow < len(nums) and fast < len(nums):
            # If odd, look for even to swap with
            if nums[slow] % 2 == 1:
                # While we only have odds or are at the end
                while fast < len(nums) and nums[fast] % 2 == 1:
                    # Keep looking
                    fast += 1
                # If we can swap, then swap
                if fast < len(nums):
                    nums[slow], nums[fast] = nums[fast], nums[slow]
            fast += 1
            slow += 1
            
        return nums
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)`

