---
tags:
- array
- interval
---

### 56. Merge Intervals

Link: [here](https://leetcode.com/problems/merge-intervals/description/)

#### Problem
Given an array of intervals where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

#### Approach
So the first thing to note about this problem, is that the elements aren't necessarily sorted in any order, so the first thing we should do is sort them based on their starting index, which will allow us to iterate through the `intervals` set in one pass.
As we iterate through the intervals, at each one we consider adding it to the solution set and if a merge needs to be done.
1. If the element is fully before, we can just add the current element and continue iterating, making sure to update curr to the new value, which in this case is just the current element we are iterating on.
2. If the element is fully after, we can just add the element that we are iterating on and continue with curr at the same value.
3. If there is overlap, we update the current interval with the min and max of the start and end respectively, and continue trying to merge.
   
Finally we need to add the current element to the solution if not already added, and then return the answer list that we built.

**Note**
There is a slightly simpler solution that doesn't involve needing a `curr` variable, that has the same time and space complexity. It only handles 2 cases:
1. If the start of the current element is greater than the end of the last element in the `ans` array, just add the current element to `ans`.
2. Otherwise, we know there is an overlap between the current element and the last element in `ans` so we just update the end value in the `ans` array to match that of the current element we are looking at. 
   
Because we sort the array, we don't need to worry about looking at an element where the start is before some element that is already in the `ans` array.

#### Solution
```python 
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        ans = []
        intervals.sort(key=lambda x: x[0])
        curr = intervals[0]

        for interval in intervals[1:]:
            # If curr is fully before, add curr to ans and keep going
            if curr[1] < interval[0]:
                ans.append(curr) 
                curr = interval
            # If curr is fully after, add index and keep going
            elif curr[0] > interval[1]:
                ans.append(interval)
            # Otherwise we have an intersection and need to merge
            else:
                curr[0] = min(curr[0], interval[0])
                curr[1] = max(curr[1], interval[1])
        
        ans.append(curr)
        return ans
```

#### Time Complexity
`nlog(n)`

#### Space Complexity
`O(n)`

