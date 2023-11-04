---
tags: [dynamic_programming, array]
---

### 152. Maximum Product Subarray

Link: [here](https://leetcode.com/problems/maximum-product-subarray/description/)

#### Problem
Given an integer array `nums`, find a subarray that has the largest product, and return the product.

The test cases are generated so that the answer will fit in a 32-bit integer.

#### Approach
The approach for this problem is a bit tricky, as we need to consider the potential cases we can see. Since we are dealing with negative numbers, we need to be sure that we are handling cases where we could potentially multiply a negative number and a negative number.
At each step of iterating through the array, we want to track some values: 
1. The current max product of all values before the current one.
2. The current minimum product of all values before the current one.
3. The current value by itself.
These allow us to consider all cases of the potential current value and end up with the optimal solution. The max product purpose is clear, if we have a large max product and can increase the size with the current value, we should do so. The minimum product's purpose is to handle cases where the current value is negative, and we can multiply it with the minimum product to get a large positive value. The final case is for when just starting over from the current value gives us a larger product than if we included the previous values, e.g. `curr = 5, max = -4`.
We compare all these values at each step to keep track of the max and min as we introduce a new value to the existing optimal products. So what this will look like in code will be:
```
curr = nums[i]
maxProduct, minProduct = 
    max(maxProduct * curr, minProduct * curr, curr),
    min(maxProduct * curr, minProduct * curr, curr)

```

#### Solution
```python 
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        ans = nums[0]
        currMax, currMin = ans, ans

        for i in range(1,len(nums)):
            currMax, currMin = max(nums[i]*currMax, nums[i]*currMin, nums[i]), min(nums[i]*currMax, nums[i]*currMin, nums[i])
            ans = max(currMax, ans)
        
        return ans
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)`

