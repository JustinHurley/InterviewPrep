---
tags:
  - medium
  - math
  - sorting
---
### 1637. Widest Vertical Area Between Two Points Containing No Points

Link: [here](https://leetcode.com/problems/widest-vertical-area-between-two-points-containing-no-points/description/)

#### Problem
Given `n` `points` on a 2D plane where `points[i] = [xi, yi]`, Return _the **widest vertical area** between two points such that no points are inside the area._

A **vertical area** is an area of fixed-width extending infinitely along the y-axis (i.e., infinite height). The **widest vertical area** is the one with the maximum width.

Note that points **on the edge** of a vertical area **are not** considered included in the area.

#### Main Idea
- We only care about the x-coordinate 
- Sort based on that, and find the maximum distance between any 2 consecutive points

#### Approach
- See above

#### Edge Cases
- Missing or invalid data

#### Solution
```python 
class Solution:
    def maxWidthOfVerticalArea(self, points: List[List[int]]) -> int:
        points.sort(key=lambda x: x[0])
        best = 0
        for i in range(1, len(points)):
            best = max(best, points[i][0]-points[i-1][0])
        return best
```

#### Time Complexity
`O(nlog(n))`

#### Space Complexity
`O(1)`

