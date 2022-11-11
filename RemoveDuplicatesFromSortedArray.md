### \#26. Remove Duplicates from Sorted Array

Link: [here](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)

#### Topics
- Two pointers
- Arrays
- In place array manipulation

#### Problem
Given an integer array `nums` sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

Return `k` after placing the final result in the first `k` slots of `nums`.

Do not allocate extra space for another array. You must do this by modifying the input array in-place with `O(1)` extra memory.

#### Approach
The general approach is a two pointer solution, one to keep track of where in the array the unique elements should go, and one to scan through the array to find unique elements.
The way this algorithm works is that the fast pointer moves through the array until it finds a unique element, relative to the element before it. We then need to move the element to the position of the other pointer, which keeps track of the position of the next unique element. It's important to start the pointers at index 1 instead of index 0, as we need 1 space of buffer so we can check the element before without having to worry about an `OutOfBounds` exception at index position 0. We also know that the 0-index element is the smallest so we don't have to ever worry about swapping it.

#### Solution
```

class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        n = len(nums)
        p = 1
        # Start a 1 so you can look back 1 elt without outofbounds excep 
        for i in range(1, n):
            # If we have a new val we want to record it
            if nums[i] != nums[i-1]:
                # Swap the curr postition witht the prev position
                nums[p] = nums[i]
                # Increment p for the next new elt 
                p += 1
        # Return p as it will be equal to index of elts + 1 (num elts)
        return p
```
