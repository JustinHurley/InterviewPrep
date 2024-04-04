---
tags:
  - array
  - interval
  - medium
---
# 253. Meeting Rooms II

Link: [here](https://leetcode.com/problems/meeting-rooms-ii/description/)

## Problem
Given an array of meeting time [[Intervals]] `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of conference rooms required.
## Main Idea
- "Simulate" meetings chronologically
- Sort all start and end times
- Start a meeting, update the time, and then end all meetings that end after the current time, track max number of meetings open at a given time
## Approach
So the approach for this problem is pretty intuitive when you think about it. Here, we are given all the starting and ending times of meetings. So to determine how many meetings need to run at the same time, we just step through the day, seeing how many meetings are happening at a given time. 
We do this by extracting the start and end times, putting them into their own arrays, sorting the arrays in order, and then running a while loop through the start times, with pointers for the start and end array. 
If the current start time is before the current end time, we increment the start pointer and also increment the room counter as well. Otherwise, we increment the end pointer and decrement the room counter. We can think of incrementing the start counter as "starting" a meeting, and incrementing the end counter as "ending" a meeting. When we see a start time and it's before the current end time, it means we need to start another meeting, but there is a meeting that hasn't reached it's end time (because the start time is less), so we should book another room. When we advance the end pointer, it means that the current start time has not been reached yet, so we can end the current meeting and focus on the end of the next meeting.

#### Solution
```python 
class Solution:
    def minMeetingRooms(self, intervals: List[List[int]]) -> int:
        n = len(intervals)
        starts = sorted(i[0] for i in intervals)
        ends = sorted(i[1] for i in intervals)
        
        # Move through meetings
        s_i, e_i = 0, 0
        rooms, max_rooms = 0, 0
        time = 0
        while s_i < n:
            # Start meeting
            time = starts[s_i]
            s_i += 1
            rooms += 1
            # End all meetings that are already over
            while ends[e_i] <= time:
                rooms -= 1
                e_i += 1
            # See how many meetings are happening 
            max_rooms = max(rooms, max_rooms)
        return max_rooms
```

#### Time Complexity
$O(n*log(n))$ for the sorts.
#### Space Complexity
$O(n)$
