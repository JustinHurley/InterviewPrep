---
tags:
- two_pointer
- interval
- greedy_choice
---

### 986. Interval List Intersections
Link [here](https://leetcode.com/problems/interval-list-intersections/)
  
Given 2 lists of closed intervals
Intervals are always different numbers and in order
We want to return the intersection of the two lists
Looking for closed interval [a, b]
How to evaluate two intervals
- Intersection is the highest starting value and the lowest ending value
- If an intersection has an ending value less than the other's starting value, advance to next interval
- Repeat until done
It's important to compare intervals until they aren't valid anymore

This problem comes down to finding criteria of when to advance a pointer and when to make an intersection. We can use a greedy property to determine which interval gets to stay. In this case, the interval with the earlier finish time gets their pointer advanced first. 

Logic:
 - If end_a < start_b then there is no intersection, advance a
 - If start_a > end_b then there is no intersection, advance b
 - Otherwise we have an intersection
   - To find the intersection of 2 ranges, take the max of the starts and the min of the ends.
  
Solution:
```
class Solution:
    def intervalIntersection(self, firstList: List[List[int]], secondList: List[List[int]]) -> List[List[int]]:
        
        i = 0
        j = 0
        firstLen = len(firstList)
        secondLen = len(secondList)
        ans = []
        
        while i < firstLen and j < secondLen:
            currFirst = firstList[i]
            currSecond = secondList[j]
            
            # If first start interval is > second end, move second up
            if currFirst[0] > currSecond[1]:
                j += 1
            # If first end is less than second start, move first up
            elif currFirst[1] < currSecond[0]:
                i += 1
            # Otherwise we need to find the interval
            else:
                # Want the larger start value
                start = max(currFirst[0],currSecond[0])
                # Want the smaller end value
                end = min(currFirst[1], currSecond[1])
                ans.append([start, end])
                # Now we want to advance the pointer with a lower end 
                if currFirst[1] < currSecond[1]:
                    i += 1
                else:
                    j += 1
        
        return ans
```
