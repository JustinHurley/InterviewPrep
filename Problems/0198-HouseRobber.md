---
tags:
  - recursion
  - dynamic_programming
  - medium
---

### 198. House Robber

Link: [here](https://leetcode.com/problems/house-robber/)

#### Problem
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

#### Approach
This is a [[DynamicProgramming|dynamic programming]]/memoization problem. There is no greedy choice we can make so that usually means we have to do [[DynamicProgramming|dynamic programming]]. The main choice the code has to make is to either rob the house or to not rob the house.

###### Approach 1: Memoization
Also known as the top-down approach to [[DynamicProgramming|dynamic programming]]. The idea behind memoization is that we want to save values that have already been computed, which saves us extra work. In the decision tree, we can see that there are different ways to get to the same index in the array.
To solve this problem we make a recursive function, that starts from 0. 
- First it checks if it's past all elements of the list (there are 0 houses) and if so, returns 0 as there is nothing to rob. 
- The next check is to see if the value is in the memoization table, if so, return that result to save unnecessary work.
- If the above two conditions are not satisfied, we continue navigating the decision tree with a recursive call, where we find the max of the scenario where the current house is robbed, and when it isn't robbed.

###### Approach 2: Dynamic_Programming
This is the bottom up approach, where instead of starting at the beginning of the list and working our way down the decision tree, we instead start at the leaves of the tree and work our way up. In this case the bottom leaf is when `i >= len(nums)`. In this case there are no houses to steal so we just return 0. Then we look at the `i-1` case where there is only 1 house, in which case we just return the value for that one house. Using these cases, we set those as the values for `n` and `n-1` in our table and then work backwards. 
We can just iterate backwards through the array and at each house the way to find the best value is the same. We can either rob this house and skip the next, or skip this house and check the next. We are just skipping the part with all the recursive calls.
Something to watch out for is that we need to make an array for the empty case, when `i == n`. This means when making the array to store the DP values in it, it needs to be of size `n+1` and not size `n`.

###### Approach 3: Optimized Dynamic_Programming
This approach iterates through the array, and leverages the fact that we only really need to track the previous 2 values to determine a current value. We instantiate 2 variables and set them equal to 0. One will represent the the robber that has robbed the previous house and thus must skip the current house, and the other robber will represent the robber that robbed the house 2 houses ago, and thus can rob this house. We then take the max of those two values. We then want to update the position of our robbers, so we set the robber two houses away to the robber 1 house away (so it will be 2 houses away on the next iteration), and then set the robber 1 house away to the max value that was just calculated, so that it will represent the robber who is 1 house away to the current house.
```
Current setup: [rob1, rob2, num, num+1, ...]
Setup for next iteration: [rob1, rob2, num+1, ...]
```
So as you can see we prepare the robber variables for the next iteration (num+1).

#### Solution
###### Approach 1:
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        self.n = len(nums)
        # Dictionary for memoization
        self.memo = {}
        return self.robFrom(0, nums)
        
    def robFrom(self, i, nums):
        # If we have already examined everything then we are done.
        if i >= self.n:
            return 0
        # First check the memo table to see if work has been done already
        if i in self.memo:
            return self.memo[i]
        # If not we need to keep looking
        # self.robFrom(i+2)+nums[i] is the scenario where we choose the current elt
        # self.robFrom(i+1) is the scenario where we skip the current elt
        ans = max(self.robFrom(i+2, nums)+nums[i], self.robFrom(i+1, nums))
        # Now we add the solution to the memo list
        self.memo[i] = ans
        return ans
```
###### Approach 2:
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        # Edge case
        if not nums:
            return 0
        n = len(nums)
        
        # Make the DP array, note the +1 because we want to handle case when we are at position n
        maxRobbedAmount = [None for _ in range(len(nums)+1)]
        
        # Now we initialize the base case
        maxRobbedAmount[n], maxRobbedAmount[n-1] = 0, nums[n-1]
        
        # Now we fill in the table (working backwards)
        for i in range(n-2, -1, -1):
            # This is saying our current cell is equal to the max of 2 cases
            # When this house is skipped
            # When this house is robbed
            maxRobbedAmount[i] = max(maxRobbedAmount[i+1],maxRobbedAmount[i+2]+nums[i])
        
        return maxRobbedAmount[0]
```
###### Approach 3:
```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        rob1, rob2 = 0, 0

        # [rob1, rob2, num, num+1, ...]
        for num in nums:
            # Calculate best path up to num
            curr = max(rob1 + num, rob2)
            # Move up rob1 to rob2 pos
            rob1 = rob2
            # Update rob2 to num position (which is equal to curr)
            rob2 = curr

        return rob2
```

#### Time Complexity
`O(n)` we pass through `nums` once.

#### Space Complexity
`O(n)` if you use the memoized approach, otherwise it's just `O(1)` for the 2 variable solution.