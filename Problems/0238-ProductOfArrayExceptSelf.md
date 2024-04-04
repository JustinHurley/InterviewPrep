---
tags:
  - array
  - prefix_sum
  - math
  - medium
---
# 238. Product of Array Except Self

Link: [here](https://leetcode.com/problems/product-of-array-except-self/description/)

## Problem
Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.
## Main Idea
- We can take the prefix product and postfix product of each number (the product of all elements up to that element) and then to get the answer multiply the prefix and postfix products for that given cell
## Approach
This problem relies on prefix sum (in this case prefix product), where we utilize the fact that multiplication (and addition) is commutative, so we can multiply the items in any order and end up with the same solution.
This means that for element `x` with index `i`, we only need to get the product of all elements to the left and all elements to the right, and then can multiply them by each-other to solve the problem.
We can do this by doing 2 passes, left-to-right and then right-to-left in the array, where at each step of the iteration, we take the current product and multiply the current element, then multiply the current product by the number in the corresponding `nums` array and then move on. This ensures that we aren't adding the corresponding `nums` element to the answer array.
We have now generated the left prefix product of the array, and now we have to generate the right. This can be done in the same fashion as previously, just in this instance we iterate backwards. Starting at the end of the array and moving to the first element.

#### Solution
```python 
class Solution: 
	def productExceptSelf(self, nums: List[int]) -> List[int]: 
		n = len(nums) 
		ans = [1]*n currProd = 1 
		
		# First pass 
		for i in range(0,n): 
			# First we set the ans 
			ans[i] = currProd 
			# Then mult by the curr index 
			currProd *= nums[i] 
		# Reset currProd 
		currProd = 1 
		# Second pass 
		for j in range(n-1,-1,-1): 
			#First set the ans 
			ans[j] = ans[j]*currProd 
			# Then incr 
			currProd *= nums[j] 
		return ans
```
