---
tags: [medium, greedy, string]
---
# 763. Partition Labels
Link: [here](https://leetcode.com/problems/partition-labels/description/)
## Problem
You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part.
Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.
Return _a list of integers representing the size of these parts_.
## Main Idea
- Learn the ranges of all the characters and store them in a hash map
- Greedily try to make an interval, keeping track of the furthest char in that interval and updating when necessary 
- If you reach the end of the interval, it means you can make a new one 
## Approach
- Scan the input string to get the range of all chars (can technically just look at the last instance of each char)
- Iterate through the string again, while also keeping track of the current interval we are in
	- If we are outside the interval, it means we can start a new one 
	- Otherwise we should be looking at the range for each char given, and updating the current interval if necessary 
## Edge Cases/Gotchas 
- Don't really need to keep track of the starting interval 
- Be careful of getting the last interval, because if it ends on `n`, the index will never reach it and the interval will never be closed, probably a better way to do it in the future 
## Solution
```python 
class Solution:
    def partitionLabels(self, s: str) -> List[int]:
        ranges = {}
        n = len(s)
        # Build intervals
        for i in range(n):
            if s[i] not in ranges:
                # Range from i to i+1 exclusive 
                ranges[s[i]] = [i,i+1]
            else:
                ranges[s[i]][1] = i+1
        ans = []
        curr_range = [0,1]
        for i in range(n):
            c = s[i]
            # Look to see if we are out of the range
            if i >= curr_range[1]:
                # If so, we finished the curr partition so take len
                ans.append(curr_range[1]-curr_range[0])
                # New working range is from the curr char
                curr_range = ranges[c]
            else:
                # Update the range if we need to
                curr_range[1] = max(curr_range[1], ranges[c][1])
        if curr_range[1] == n:
            ans.append(curr_range[1]-curr_range[0])
        return ans
```
## Time Complexity
$O(n)$ since we do 2 passes of the input string
## Space Complexity
$O(1)$ since we only use lowercase chars so max size of the ranges dictionary would be 26
