### 739. Daily Temperatures

Link: [here](https://leetcode.com/problems/daily-temperatures/)

#### Topics
- Stack
- Monotonic stack
- Array

#### Problem
Given an array of integers `temperatures` represents the daily temperatures, return an array answer such that `answer[i]` is the number of days you have to wait after the `ith` day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

#### Approach
The general approach hinges on a monotonically decreasing stack. We use this because it is a good way to "remember" all the values that are less than the current value, and process them when the current value is larger. 
The way this works is that we add values to the stack when they are less than the top of the stack. But when they are larger, we pop the values in the stack until we hit a val that is `>=` our current value. As we pop values from the stack, we do work on them to determine the difference in days between the current value and the past value. Keeping them in this monotonically decreasing stack ensures that we process the past days as soon as we see a new day with a higher temperature.
So we traverse the input array from left to right, adding `(temp, index)` pairs as we go along. We do this until we see a value larger than the top of stack value. When this happens, we pop the top of the stack, and look at it's values and index. We know that this is the first time we are seeing a temp higher than the temp listed on the value we just popped, because otherwise that value would have been popped already. We then take the difference of the current index and that of the index of the value we just popped, and then add it to our answer array (in the position of the index of the popped value). We then do this for all values smaller than our current value, and once that is done, we add our current value to the stack, ready to wait for another value to come along, or in the case that there is no larger value, it is never popped from the stack and the answer remains 0, because that was what we initialized it to. 
This monotonically stack approach is good when we want to "remember" past values, and do work on those values when a condition is met, and there is a variable distance from a value and the value that triggers that condition. In the context of this problem it means we can "remember" and keep track of values until a larger one comes along and we can then process and calculate the difference in days relative to the popped value. 

#### Solution
```
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        stack = [] # t,i
        # Make the ans array all 0s
        ans = [0]*len(temperatures)

        for i,t in enumerate(temperatures):
            # While there are items in the stack less than t
            while stack and t > stack[-1][0]:
                # Pop the stack val
                stackT, stackI = stack.pop()
                # We have now found a larger val, so we update the ans with the diff
                ans[stackI] = i - stackI
            # Finally we add our new stack val to the ans
            stack.append([t,i])
        
        return ans
```

#### Time Complexity
`O(n)` since we do one pass and each element is processed at most 1 other time, so `O(2n) -> O(n)`.

