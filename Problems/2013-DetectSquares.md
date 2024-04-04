---
tags: [math, medium]
---
# 2013.  Detect Squares
Link: [here](https://leetcode.com/problems/detect-squares/description/)
## Problem
You are given a stream of points on the X-Y plane. Design an algorithm that:
- **Adds** new points from the stream into a data structure. **Duplicate** points are allowed and should be treated as different points.
- Given a query point, **counts** the number of ways to choose three points from the data structure such that the three points and the query point form an **axis-aligned square** with **positive area**.

An **axis-aligned square** is a square whose edges are all the same length and are either parallel or perpendicular to the x-axis and y-axis.

Implement the `DetectSquares` class:
- `DetectSquares()` Initializes the object with an empty data structure.
- `void add(int[] point)` Adds a new point `point = [x, y]` to the data structure.
- `int count(int[] point)` Counts the number of ways to form **axis-aligned squares** with point `point = [x, y]` as described above.
## Main Idea
- Use a hashmap to keep track of points
- When doing `count` we can see which points are diagonal of the original point, then try to find the other sides of that square based on the diagonal
- Find number of squares you can make by multiplying the number of corner points that exist as long as a valid square is made
## Approach
- Use a hashmap to track points, we need to use a map and not a set because there are duplicate points
- When adding just increment the hashmap
- For count, go through all points seen so far
	- If a point is diagonal of the query point, check and see if the other points in the square exist 
	- If they do, multiply the count of each point in that square and add it to the overall count
- Return count after going through all the points
## Edge Cases/Gotchas 
- Need to handle when there is a square with an area of 0 since we don't want that
## Solution
```python 
class DetectSquares:

    def __init__(self):
        self.points = defaultdict(int)

    def add(self, point: List[int]) -> None:
        self.points[(point[0], point[1])] += 1
        
    def count(self, point: List[int]) -> int:
        sums = 0
        x, y = point
        for px, py in self.points:
            # If diagonal count how many diagonal points
            if abs(x-px) == abs(y-py) and x != px and y != py:
                # If the other corners exist
                if (x, py) in self.points and (px, y) in self.points:
                    # Get sum by multiplying num points
                    sums += self.points[(px, py)] * self.points[(x, py)] * self.points[(px, y)]
        return sums
```
## Time Complexity
$O(n)$ since we only go through points once
## Space Complexity
$O(n)$
