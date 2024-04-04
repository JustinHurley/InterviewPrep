---
tags:
  - medium
  - interval
  - sorting
  - greedy
---
# 452. Minimum Number of Arrows to Burst Balloons
Link: [here](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/description/)
## Problem
There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array `points` where `points[i] = [xstart, xend]` denotes a balloon whose **horizontal diameter** stretches between `xstart` and `xend`. You do not know the exact y-coordinates of the balloons.

Arrows can be shot up **directly vertically** (in the positive y-direction) from different points along the x-axis. A balloon with `xstart` and `xend` is **burst** by an arrow shot at `x` if `xstart <= x <= xend`. There is **no limit** to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array `points`, return _the **minimum** number of arrows that must be shot to burst all balloons_.
## Main Idea
- Sort the intervals so they are in order
- Greedily build a range that fits as many intervals as possible, until there is no more overlap, this counts as a pop of all balloons that contain that range
## Approach
- Sort the points by starting element
- Set the start and end of the range to be the start and end of the first point
- Set the pop to be 1, since we will have a range left over at the end we'll need to account for 
- Now go through all the points in the array
	- If the current point starts after the current range ends, we should pop all balloons in the current range and start over (1 pop total)
	- If there is an overlap, we should make the range smaller so reflect the updated smaller range for that interval and the existing interval (0 pops). This is done by taking the max of the starts and the min of the ends
- The count at the end is the answer
## Edge Cases/Gotchas 
- Need to handle the input array being null
- Might need to handle invalid points
## Solution
```python 
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        points.sort()
        # Set range to first point
        end = points[0][1]
        pops = 1
        for point in points:
            # if no overlap, pop what we have and update
            if point[0] > end:
                pops += 1
                end = point[1]
            # if overlap, take smallest range
            else:
                end = min(end, point[1])
        return pops
```
## Time Complexity
$O(n*log(n))$ for the sorting 
## Space Complexity
$O(n)$ since sort uses `n` space 
