---
tags: [array, interval]
---

### 253. Meeting Rooms II

Link: [here](https://leetcode.com/problems/meeting-rooms-ii/description/)

#### Problem
Given an array of meeting time [[intervals]] `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of conference rooms required.

#### Approach
So the approach for this problem is pretty intuitive when you think about it. Here, we are given all the starting and ending times of meetings. So to determine how many meetings need to run at the same time, we just step through the day, seeing how many meetings are happening at a given time. 
We do this by extracting the start and end times, putting them into their own arrays, sorting the arrays in order, and then running a while loop through the start times, with pointers for the start and end array. 
If the current start time is before the current end time, we increment the start pointer and also increment the room counter as well. Otherwise, we increment the end pointer and decrement the room counter. We can think of incrementing the start counter as "starting" a meeting, and incrementing the end counter as "ending" a meeting. When we see a start time and it's before the current end time, it means we need to start another meeting, but there is a meeting that hasn't reached it's end time (because the start time is less), so we should book another room. When we advance the end pointer, it means that the current start time has not been reached yet, so we can end the current meeting and focus on the end of the next meeting.

#### Solution
```python 
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        starts = sorted([i[0] for i in intervals])
        ends = sorted([i[1] for i in intervals])

        ans = 0
        count = 0

        si, ei = 0, 0
        # While we haven't "started" every meeting
        while si < len(starts):
            # If the start time if before the current meeting is over, add to the count
            if starts[si] < ends[ei]:
                si += 1
                count += 1
            # Otherwise we can end the meeting and decrement the count 
            else:
                ei += 1
                count -= 1
            # After every meeting start or end, eval the max count
            ans = max(ans, count)
        return ans
```

#### Time Complexity
`O(nlog(n))` for the sorts.

#### Space Complexity
`O(n)`

