---
tags:
- array
- interval
---

### 57. Insert Interval

Link: [here](https://leetcode.com/problems/insert-interval/description/)

#### Problem
You are given an array of non-overlapping [[intervals]] intervals where `intervals[i] = [starti, endi]` represent the start and the end of the ith interval and [[intervals]] is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into intervals such that intervals is still sorted in ascending order by `starti` and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return intervals after the insertion.

#### Approach
Tackling this problem comes down to handling the different interval cases. As we move through the array of intervals we want to consider each case. As we pass through these [[intervals]], we build them together and handle different cases and add them to an answer set.

1. The interval we want to insert is before the current interval we are looking at: In this case we have already "handled" the previous windows and so we can just insert the new interval, and then append the rest of the interval array from that position.
2. The interval we want to insert is after the current interval we are looking at: In this case, we know that the current interval is "safe" i.e. we don't have to worry about overlap with the interval we want to add, and we already know it has no overlap with previous elements. So we add it to the answer array and keep going.
3. This is the case where there is overlap. In this case, we want to merge the new interval and the current interval, which can be done by taking the `min` of the starting points, and the `max` of the ending points. Once this is done we keep iterating through the array, since we don't know that the next interval we are going to look at also doesn't overlap with the current element. 

Once we stop the loop we just need to append the new interval, since if we don't return in the loop it means we never found a scenario where the new interval is fully before the current interval, and thus the only place to put it is at the end of the array. 

#### Solution
```python 
class Solution:
    def insert(self, intervals: List[List[int]], newInterval: List[int]) -> List[List[int]]:
        ans = []

        for i in range(len(intervals)):
            # If interval is before, add new and be done
            if intervals[i][0] > newInterval[1]:
                ans.append(newInterval)
                return ans + intervals[i:]
            # If interval is after, add curr and keep going
            elif intervals[i][1] < newInterval[0]:
                ans.append(intervals[i])
            # Otherwise we have some kind of overlap
            else:
                # Since we have an overlap, we should update the interval we want to merge
                newInterval[0] = min(newInterval[0], intervals[i][0])
                newInterval[1] = max(newInterval[1], intervals[i][1])

        # Need to append for elif and else case where we never append newInterval
        ans.append(newInterval)
        return ans
```

#### Time Complexity
`O(n)` 

#### Space Complexity
`O(n)`
