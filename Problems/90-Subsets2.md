---
tags:
- permutation
- set
- tuple
---

### 90. Subsets II
Link: [here](https://leetcode.com/problems/subsets-ii/)

#### Problem
Given an integer array `nums` that may contain duplicates, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

#### Approach
We are asked to give the power set on a list of integers. What makes this different than [Subsets I](https://leetcode.com/problems/subsets/) is that in this problem, values in the list can be repeated.
To handle this, we need to use a set, which will prevent duplicate items from being added.
We also need to sort the numbers, otherwise we would end up with `[1,2]` and `[2,1]`. 
We also need to use tuples for this problem, as lists cannot be stored in sets due to the fact that Python cannot hash them.
This means our approach will involve tuples and sets, where we simply iterate through each value, and add on to each value to the set. 

#### Solution 
```
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