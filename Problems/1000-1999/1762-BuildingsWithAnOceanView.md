---
tags: [medium, monotonic_stack]
---
# 1762. Buildings with an Ocean View
Link: [here](https://leetcode.com/problems/buildings-with-an-ocean-view/description/)
## Problem
There are `n` buildings in a line. You are given an integer array `heights` of size `n` that represents the heights of the buildings in the line.

The ocean is to the right of the buildings. A building has an ocean view if the building can see the ocean without obstructions. Formally, a building has an ocean view if all the buildings to its right have a **smaller** height.

Return a list of indices **(0-indexed)** of buildings that have an ocean view, sorted in increasing order.
## Main Idea
- Extra problem constraint that we can't iterate right to left
- Use a monotonic stack to keep track of tallest buildings with a view
- If a smaller building in the stack, pop it
## Approach
- See above
## Edge Cases/Gotchas 
- Know how we should handle when the heights are equal
## Solution
```python 
class Solution:
    def findBuildings(self, heights: List[int]) -> List[int]:
        stack = []

        for i in range(len(heights)):
            # While we are blocking existing buildings
            while stack and heights[i] >= heights[stack[-1]]:
                stack.pop()
            stack.append(i)
        
        return stack
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$ unless you don't count the answer array
