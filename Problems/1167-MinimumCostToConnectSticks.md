---
tags:
  - medium
  - heap
  - greedy
---
### 1167. Minimum Cost to Connect Sticks

Link: [here](https://leetcode.com/problems/minimum-cost-to-connect-sticks/)

#### Problem
You have some number of sticks with positive integer lengths. These lengths are given as an array `sticks`, where `sticks[i]` is the length of the `ith` stick.

You can connect any two sticks of lengths `x` and `y` into one stick by paying a cost of `x + y`. You must connect all the sticks until there is only one stick remaining.

Return _the minimum cost of connecting all the given sticks into one stick in this way_.

#### Main Idea
- Want to minimize the number of times the longer sticks get added
- If we always add the two shortest sticks together we end up with an optional solution

#### Approach
- Use a heap to store the stick lengths in a min-heap
- Iterate through the heap until there is only one stick left
- Use heap operations to maintain the heap property

#### Edge Cases/Gotchas 
- Invalid data
- We could speed the code up a bit by skipping the heapify if there are 0 or 1 sticks

#### Solution
```python 
class Solution:
    def connectSticks(self, sticks: List[int]) -> int:
        heapq.heapify(sticks)
        cost = 0
        while len(sticks) > 1:
            smallest = heapq.heappop(sticks)
            nextSmallest = heapq.heappop(sticks)
            cost += smallest + nextSmallest
            heapq.heappush(sticks, smallest + nextSmallest)
        return cost
```

#### Time Complexity
`O(nlogn)` which comes from calling `heappush` `n` times

#### Space Complexity
`O(n)` since we need to make the heap to store the data

