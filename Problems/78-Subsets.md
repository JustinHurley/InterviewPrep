---
tags:
- backtracking 
- recursion 
- permutation
- array
---

### 78. Subsets
Link: [here](https://leetcode.com/problems/subsets/)

#### Problem
Given an integer array `nums` of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

#### Approach
We can simply permute through the problem by iterating through the integer array, and for each integer, look at the current set of permutations and apply the current integer to those permutations.
An issue to watch out for on this problem is to make sure that you aren't adding to the array that you're iterating, as this will cause an infinite loop.

#### Solution
```python 
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        ans = [[]]
        # Iterate through each element in the set
        for num in nums:
            temp = []
            # At each element, go through all the subsets that exist
            for curr in ans:
                temp += [curr + [num]]
            # Add all the elements to the curr
            for e in temp:
                ans += [e]
        return ans
```
Alternatively, instead of the `temp` array and the second for loop, we can iterate on the set we are adding to by using this notation.
```
for num in nums:
    ans += [curr + [num] for curr in ans]
```
The above notation computes the value on the right, and then applies it to the left, preventing the infinite loop problem.