---
tags: [easy, math]
---
### 1779. Find Nearest Point That Has the Same X or Y Coordinate

Link: [here](https://leetcode.com/problems/find-nearest-point-that-has-the-same-x-or-y-coordinate/description/)

#### Problem
You are given two integers, `x` and `y`, which represent your current location on a Cartesian grid: `(x, y)`. You are also given an array `points` where each `points[i] = [ai, bi]` represents that a point exists at `(ai, bi)`. A point is **valid** if it shares the same x-coordinate or the same y-coordinate as your location.

Return _the index **(0-indexed)** of the **valid** point with the smallest **Manhattan distance** from your current location_. If there are multiple, return _the valid point with the **smallest** index_. If there are no valid points, return `-1`.

The **Manhattan distance** between two points `(x1, y1)` and `(x2, y2)` is `abs(x1 - x2) + abs(y1 - y2)`.

#### Main Idea
- Go through each point
- Only check points that share an axis
- If current point is closer than closest point, update best value

#### Approach
- See above

#### Edge Cases
- Invalid data

#### Solution
```python 
class Solution:
    def nearestValidPoint(self, x: int, y: int, points: List[List[int]]) -> int:
        best = (float('inf'), -1)
        for i, point in enumerate(points):
            if point[0] == x or point[1] == y:
                if abs(point[0]-x) + abs(point[1]-y) < best[0]:
                    best = (abs(point[0]-x) + abs(point[1]-y), i)
        return best[1]
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)`

