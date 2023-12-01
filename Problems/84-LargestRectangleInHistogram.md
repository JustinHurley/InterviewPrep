---
tags:
  - stack
  - monotonic_stack
  - array
  - hard
---
### 84. Largest Rectangle in Histogram

Link: [here](https://leetcode.com/problems/largest-rectangle-in-histogram/description/)
#### Problem
Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return _the area of the largest rectangle in the histogram_.

#### Approach
The general approach is to build the solution iteratively, i.e. look at the state with 1 item, then 2, and continue building and maintaining the status of the largest rectangle in histogram (LRIH) seen so far. 
Since we are tracking a max value over time, this seems like a good opportunity to use a stack, since we want to know the max area, but still need to depend on previous values for information. 
So in each iteration, i.e. each histogram bar we look at, we want to add it into the stack, but also maintain that the element on the TOS is the tallest elt seen so far. This is because we want to store the largest area that can be made from a histogram going from it's own bar, to the right. So if we see an elt that isn't as tall as the current elt, we need to do a few things:
- Pop the elt from the stack. 
	- We do this to maintain that the TOS elt can go all the way back to the beginning of the array.
- Calculate the area from the index of the current elt to the popped elt.
	- Since we're removing the current elt, we want to see the rectangle from the popped element to the current element. We know this is a valid rectangle, because none of the other added elts popped out the elt currently being popped out.
- Set the starting index of the current elt to the popped elt.
	- We now know that we have popped all elts that are taller than the current elt from the stack, this means, if we started from the popped elt and went to the position of the current elt, all the elts in between would be tall enough to include the rectangle.
Once we do this, we are left with a final stack that needs to be processed, where every elt in the stack can make a rectangle from the position of the elt, to the end of the array.

#### Solution
```python 
class Solution:

def largestRectangleArea(self, heights: List[int]) -> int:
	maxArea = 0
	stack = [] # 0: index, 1: height
	for i, h in enumerate(heights):
		start = i
		
	# If the current elt is higher than the TOS elt
	while stack and stack[-1][1] > h:
		# Pop TOS
		index, height = stack.pop()
		# Compare to maxArea with curr elt to
		maxArea = max(maxArea, height*(i-index))
		start = index
		
	# Now add the curr elt to the stack
	stack.append((start, h))
	for i, h in stack:
		maxArea = max(maxArea, h * (len(heights) - i))
		
	return maxArea
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)`

