---
tags:
  - easy
  - math
  - array
---
### 1266. Minimum Time Visiting All Points

Link: [here](https://leetcode.com/problems/minimum-time-visiting-all-points/description/)

#### Problem
On a 2D plane, there are `n` points with integer coordinates `points[i] = [xi, yi]`. Return _the **minimum time** in seconds to visit all the points in the order given by_ `points`.

You can move according to these rules:

- In `1` second, you can either:
    - move vertically by one unit,
    - move horizontally by one unit, or
    - move diagonally `sqrt(2)` units (in other words, move one unit vertically then one unit horizontally in `1` second).
- You have to visit the points in the same order as they appear in the array.
- You are allowed to pass through points that appear later in the order, but these do not count as visits.

#### Main Idea
- We don't need to simulate, just compute
- The distance between points in time is just the maximum difference between either the x or y coordinates

#### Approach
- See above

#### Edge Cases
 - Array contains invalid values

#### Solution
```python 
class Solution:
    def minTimeToVisitAllPoints(self, points: List[List[int]]) -> int:
        time = 0
        for i in range(1, len(points)):
            prev, curr = points[i-1], points[i]
            diff = [abs(curr[0] - prev[0]), abs(curr[1] - prev[1])]
            if diff[0] >= diff[1]:
                time += diff[0]
            else: 
                time += diff[1]
        return time
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)`

