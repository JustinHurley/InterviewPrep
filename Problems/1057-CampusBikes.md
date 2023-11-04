---
tags: [math, heap, set]
---
### 1057. Campus Bikes

Link: [here](https://leetcode.com/problems/campus-bikes/description/)

#### Problem
On a campus represented on the X-Y plane, there are `n` workers and `m` bikes, with `n <= m`.

You are given an array `workers` of length `n` where `workers[i] = [xi, yi]` is the position of the `ith` worker. You are also given an array `bikes` of length `m` where `bikes[j] = [xj, yj]` is the position of the `jth` bike. All the given positions are **unique**.

Assign a bike to each worker. Among the available bikes and workers, we choose the `(workeri, bikej)` pair with the shortest **Manhattan distance** between each other and assign the bike to that worker.

If there are multiple `(workeri, bikej)` pairs with the same shortest **Manhattan distance**, we choose the pair with **the smallest worker index**. If there are multiple ways to do that, we choose the pair with **the smallest bike index**. Repeat this process until there are no available workers.

Return _an array_ `answer` _of length_ `n`_, where_ `answer[i]` _is the index (**0-indexed**) of the bike that the_ `ith` _worker is assigned to_.

The **Manhattan distance** between two points `p1` and `p2` is `Manhattan(p1, p2) = |p1.x - p2.x| + |p1.y - p2.y|`.
#### Main Idea
- Know how to calculate the Manhattan distance
- You need to check every combination of worker and bike
- Use a heap to keep track of distance, worker, and bike

#### Approach
Break the distance calculation out into it's own method to keep the code neater. Then we will want to start building the heap. To do so, fill an array with the distances of each worker-bike pair, and the distance into a heap. We want to use a [[heap|min heap]] to keep track of the shortest distances, using the worker index value to break ties. We can then pop the stack, keeping track of which bikes have been claimed and which workers have been assigned bikes, until we have checked all the routes. Once that is completed, we can return the answer set.

#### Edge Cases
- `len(workers) != len(bikes)`
- Missing inputs
- Invalid inputs

#### Solution
```python 
class Solution:
    def assignBikes(self, workers: List[List[int]], bikes: List[List[int]]) -> List[int]:
        heap = []
        for i, worker in enumerate(workers):
            for j, bike in enumerate(bikes):
                heap.append((self.distance(worker, bike), i, j))
        heapq.heapify(heap)
        bikes_seen = set()
        workers_seen = set()
        ans = [-1]*len(workers)
        while heap:
            curr = heapq.heappop(heap)
            if curr[2] not in bikes_seen and curr[1] not in workers_seen:
                ans[curr[1]] = curr[2]
                bikes_seen.add(curr[2])
                workers_seen.add(curr[1])
        return ans

    def distance(self, a, b):
        return abs(b[0]-a[0]) + abs(b[1]-a[1])
```

#### Time Complexity
`O(nmlog(nm))` 

#### Space Complexity
`O(nm)`

