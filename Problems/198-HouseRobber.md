### 198. House Robber

Link: [here](https://leetcode.com/problems/house-robber/)

#### Topics
- Recursion
- Dynamic programming
- Memoization
- Top-down approach
- Bottom-up approach

#### Problem
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array `nums` representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

#### Approach
This is a dynamic programming/memoization problem. There is no greedy choice we can make so that usually means we have to do dynamic programming. The main choice the code has to make is to either rob the house or to not rob the house.

###### Approach 1: Memoization
Also known as the top-down approach to dynamic programming. The idea behind memoization is that we want to save values that have already been computed, which saves us extra work. In the decision tree, we can see that there are different ways to get to the same index in the array.
To solve this problem we make a recursive function, that starts from 0. 
- First it checks if it's past all elements of the list (there are 0 houses) and if so, returns 0 as there is nothing to rob. 
- The next check is to see if the value is in the memoization table, if so, return that result to save unnecessary work.
- If the above two conditions are not satisfied, we continue navigating the decision tree with a recursive call, where we find the max of the scenario where the current house is robbed, and when it isn't robbed.

###### Approach 2: Dynamic Programming
This is the bottom up approach, where instead of starting at the beginning of the list and working our way down the decision tree, we instead start at the leaves of the tree and work our way up. In this case the bottom leaf is when `i >= len(nums)`. In this case there are no houses to steal so we just return 0. Then we look at the `i-1` case where there is only 1 house, in which case we just return the value for that one house. Using these cases, we set those as the values for `n` and `n-1` in our table and then work backwards. 
We can just iterate backwards through the array and at each house the way to find the best value is the same. We can either rob this house and skip the next, or skip this house and check the next. We are just skipping the part with all the recursive calls.
Something to watch out for is that we need to make an array for the empty case, when `i == n`. This means when making the array to store the DP values in it, it needs to be of size `n+1` and not size `n`.

#### Solution
###### Approach 1:
```
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
```
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