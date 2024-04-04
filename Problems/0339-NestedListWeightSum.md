---
tags: [medium, breadth_first_search]
---
# 339. Nested List Weight Sum
Link: [here](https://leetcode.com/problems/nested-list-weight-sum/)
## Problem
You are given a nested list of integers `nestedList`. Each element is either an integer or a list whose elements may also be integers or other lists.

The **depth** of an integer is the number of lists that it is inside of. For example, the nested list `[1,[2,2],[[3],2],1]` has each integer's value set to its **depth**.

Return _the sum of each integer in_ `nestedList` _multiplied by its **depth**_.
## Main Idea
- Use BFS to process the DS level by level
## Approach
- Keep track of the level and the total sum
- Add the list to the queue
- While the queue is not empty process layer by layer
- If we have a number, stop and add to total
- If we have a list, unpack the list and add it to the queue
## Edge Cases/Gotchas 
- The input is a list and not a nested list object
## Solution
```python 
class Solution:
    def depthSum(self, nestedList: List[NestedInteger]) -> int:
        Q = deque(nestedList)
        ans = 0
        depth = 0
        while Q:
            depth += 1
            for _ in range(len(Q)):
                curr = Q.popleft()
                # If int get sum
                if curr.isInteger():
                    ans += curr.getInteger() * depth
                # Else add to Q
                else:
                    Q.extend(curr.getList())
        return ans
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
