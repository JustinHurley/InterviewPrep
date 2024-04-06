---
tags:
  - sliding_window
  - array
  - medium
---

### 209. Minimum Size Subarray Sum 
Link: [here](https://leetcode.com/problems/minimum-size-subarray-sum/)

The problem is asking to find the smallest subarray sum that is larger than a given target value.

####Approach 
The approach for this problem is to just use a [[Topics/SlidingWindow|sliding window]] and keep track of a running sum, and whenever that current sum goes over the target value, move up the back pointer until the sum is below the target again. Now you have found the smallest subarray sum given the current front pointer.

An edge case to watch out for is when the total sum of the elements is still less than the target, this can be resolved by checking the answer before returning it, and assigning some value larger than `len(nums)` so that we know when we see it, a subarray was never found, and we aren't just using the initial answer value.

#### Solution
```python 
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        # We add the +1 so we can check later if we ever found a lower value
        # We don't want to submit ans as len(nums)+1 if no subarray was found (we return 0)
        ans = len(nums)+1   
        back = 0
        currSum = 0
        # Iterate through the list
        for i in range(len(nums)):
            # Add currValue to sum
            currSum += nums[i]
            # If the current sum is >= target, move back up until not the case to ensure smallest 
            if currSum >= target:
                # While we have a greater sum, move up the back
                while currSum >= target:
                    currSum -= nums[back]
                    back += 1
                # We are now 1 less than the smallest subarray
                currLen = i - back + 2
                ans = min(ans, currLen)
        # Edge case check when total sum < target
        if ans == len(nums)+1:
            return 0
        return ans
```