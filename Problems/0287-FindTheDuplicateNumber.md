---
tags:
  - array
  - linked_list
  - cycle
  - two_pointer
  - medium
---
### 287. Find the Duplicate Number

Link: [here](https://leetcode.com/problems/find-the-duplicate-number/description/)

#### Problem
Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return _this repeated number_.

You must solve the problem **without** modifying the array `nums` and uses only constant extra space.

#### Main Idea
- Approach the input as a linked-list, where value is value but also the pointer to the next element.
- Use [[Floyd'sAlgorithm|Floyd's Algorithm]] to find the starting node of the cycle in the linked list, as that corresponds to the duplicate node.

#### Approach
Honestly you just need to learn this one. For future reference if you see a problem with an array of integers ordered `[1..n+1]` and we cannot modify or use extra space, and the array isn't sorted that is likely an indication we should try to traverse the input like a linked list. That's because we can't use binary search if it's not sorted, and thus have no great strategy to efficiently navigate the data.

#### Edge Cases and Tricky Parts
- You are looking to find when `fast` == `slow` , but cannot set both nodes equal to `0` before your `while` loop checks to see if the values are equal. Instead, we can create a faux-do-while loop by having a `while True:` and then adding a break condition at the end of the block.

#### Solution
```python 
class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        # Find fast and slow intersection
        slow, fast = 0, 0
        while True:
            slow = nums[slow]
            fast = nums[nums[fast]]
            if slow == fast:
                break
        # Now find slow and new slow intersection
        new_slow = 0
        while True:
            slow = nums[slow]
            new_slow = nums[new_slow]
            if slow == new_slow:
                break
        return slow
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)`

