---
tags:
- interval
---
### 435. Non-overlapping Intervals

Link: [here](https://leetcode.com/problems/non-overlapping-intervals/description/)

#### Problem
Given an array of [[intervals]] `intervals` where `intervals[i] = [starti, endi]`, return _the minimum number of [[intervals]] you need to remove to make the rest of the intervals non-overlapping_.

#### Approach
So to approach this problem, we are obviously going to need to look for overlaps. Determining if 2 [[intervals]] are overlapping is trivial, the difficult part is ensuring that you are removing the minimum number. Consider the [[intervals]] `[1,3], [4,6], [2,5]`. If we remove `[1,3]` or `[4,6]` we will still have an overlapping interval, as opposed to just removing `[2,5]` which leaves the set with no overlap.
This means, we need some way to know if we are removing the right interval or not. 
So firstly, we need to take a step back from this problem and look at what we are trying to determine. The question isn't asking you to tell them what was removed it's just asking for the _minimum number_ of [[intervals]] that need to be removed. 
When we see a collision in [[intervals]], we immediately know that we're going to have to remove at least one of them. To determine which one we want to remove, we simply look at which one ends earlier. We do this by keeping track of the previous end of the interval we passed. So when we get to an interval we have 2 options:
1. No collision so update the previous end and move on.
2. There is a collision, so we update the previous end to whatever interval ends sooner, by choosing 1 interval endpoint from the 2 options, we are effectively "removing" the interval that isn't used to update the previous end.
If we make sure to count every time there is a collision, then we can return that amount and be done.

#### Solution
```python 
class Solution:
	def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
		# Sort array by start
		intervals.sort()
		# Capture the end of the 1st elt
		prevEnd = intervals[0][1]
		count = 0

		  for interval in intervals[1:]:
			# If there is an overlap (prev end is after curr start) we know we need to remove
			if prevEnd > interval[0]:
				count += 1
				# We update the end to the smaller end
				# This ensures we "remove" the larger interval that ends later
				# Since we continue to track as if the prev node is still smaller
				prevEnd = min(prevEnd, interval[1])
			# If there is no overlap, just update the prev end and keep going
			else:
				prevEnd = interval[1]
		return count
```

#### Time Complexity
`O(nlog(n))` for the sort operation.
#### Space Complexity
`O(1)`

