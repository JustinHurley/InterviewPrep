---
tags: [backtracking, recursion, permutation, array]
---

### 78. Subsets
Link: [here](https://leetcode.com/problems/subsets/)

#### Problem
Given an integer array `nums` of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

#### Main Idea
- Need to permute a superset
- You can use recursion to explore the decision tree of choosing to either to leave in or take out every value.
#### Approach
We can simply permute through the problem by iterating through the integer array, and for each integer, look at the current set of permutations and apply the current integer to those permutations.
An issue to watch out for on this problem is to make sure that you aren't adding to the array that you're iterating, as this will cause an infinite loop.

An alternate approach would be to simply [[DepthFirstSearch|DFS]] through the `nums` input array, adding the scenario where you add the number and where you don't add the number to the answers.

#### Solution 1 (Building)
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
```python
for num in nums:
    ans += [curr + [num] for curr in ans]
```
The above notation computes the value on the right, and then applies it to the left, preventing the infinite loop problem.

#### Solution 2 (Backtracking)
```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        ans = []
        subset = []

        def dfs(i):
            if i >= len(nums):
                ans.append(subset.copy())
                return
            subset.append(nums[i])
            dfs(i+1)
            subset.pop()
            dfs(i+1)
        
        dfs(0)
        return ans
```
Above, we can see we append the current number to the current subset, then continue to DFS on it by calling recursively. Once that call is done, we pop the just added element to then use DFS to recursively explore the decision subtree where we don't decide to include the subset. 
Note it is important that we do `subset.copy()` to make a deep copy of the current subset.

#### Time Complexity
`O(2^n)` 

### Space Complexity
`O(n)`
