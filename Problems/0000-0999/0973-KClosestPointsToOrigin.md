---
tags:
  - heap
  - array
  - medium
---
### 973. K Closest Points to Origin

Link: [here](https://leetcode.com/problems/k-closest-points-to-origin/description/)

#### Problem
Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., `√(x1 - x2)^2 + (y1 - y2)^2`).

You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).

#### Approach
So we know that we want to order the points based on distance to the origin, thus we want to keep track of items in a list optimally, ensuring that the largest/most important elements are easy to capture. Immediately we should be thinking heap.
We create a heap and add items to the heap, using the calculated distance to the middle as the sort key. 

#### Edge Cases
- If no points are given
- If coordinates are missing/not provided

#### Solution
```python 
class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        heap = []

        for point in points:
            # Get distance
            dist = -((point[0] ** 2) + (point[1] ** 2))
            # Add distance and coords to arr
            heap.append((dist, point[0], point[1]))
        
        # Now that all dists are added heapify
        heapq.heapify(heap)

        # Now pop all elts larger than k
        while len(heap) > k:
            heapq.heappop(heap)
        
        # Now we have k closest so return
        ans = []
        for item in heap:
            ans.append([item[1], item[2]])
        return ans
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)`

