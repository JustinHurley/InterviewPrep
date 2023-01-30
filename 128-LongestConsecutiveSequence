### 128. Longest Consecutive Sequence

Link: [here](https://leetcode.com/problems/longest-consecutive-sequence/description/)

#### Topics
- Array
- Set

#### Problem
Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in `O(n)` time.

#### Approach
The main problem is ensuring that you are collecting all the right points, and also doing it without sorting, which is implied by the `O(n)` constraint. This means we will need to solve the problem in a set number of passes.
This immediately points to being able to use a data structure with `O(1)` lookup, which in this case will be Python's `set()`. We first put all of `nums` into a set using the `set(nums)` initializer, then pass through the set.
The next item that needs to be considered is determining if we are at the beginning of the sequence or in the middle. This can be done by checking for the existence of a previous element. For example, if we have `[1, 2, 3, 4]` and are looking at `2`, we can check to see if `2-1` or `1` exists in the set, and if it does, we know that we are in the middle of a sequence, and should keep looking.
When we see that we are at the beginning of a sequence (the number we are looking at minus 1 does not exist in the set), we can just iterate by 1 until we hit a number not in the set. When we finish, we then compare the sequence we hit with the best sequence so far, and update if necessary. 

#### Solution
```
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        # Populates set with list 
        numSet = set(nums)
        best = 0

        for num in nums:
            # Make sure not part of larger sequence
            if num-1 not in numSet:
                # Set start
                curr = num
                currCount = 0
                # Keep iterating through set while sequence is good
                while curr in numSet:
                    curr += 1
                    currCount += 1
                # Update best if new max
                best = max(best, currCount)
        return best
```

