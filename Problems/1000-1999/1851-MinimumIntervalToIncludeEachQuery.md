---
tags: [hard, heap, sorting, interval]
---
# 1851. Minimum Interval to Include Each Query
Link: [here](https://leetcode.com/problems/minimum-interval-to-include-each-query/description/)
## Problem
You are given a 2D integer array `intervals`, where `intervals[i] = [lefti, righti]` describes the `ith` interval starting at `lefti` and ending at `righti` **(inclusive)**. The **size** of an interval is defined as the number of integers it contains, or more formally `righti - lefti + 1`.

You are also given an integer array `queries`. The answer to the `jth` query is the **size of the smallest interval** `i` such that `lefti <= queries[j] <= righti`. If no such interval exists, the answer is `-1`.

Return _an array containing the answers to the queries_.
## Main Idea
- Sort the queries and intervals in order
- Add all potentially valid intervals to heap
- Pop all intervals that are invalid
- The top of the heap will be the smallest interval
## Approach
- Sort the intervals by starting time (python does this natively when you sort an array)
- Sort the queries but before keep track of indicies for each value
- Now we want to go through each query (in sorted order)
	- First we look at the intervals to see if we can add more intervals potentially for this query 
	- Then we look at the heap, and pop anything that's too old (we are past the end interval for that)
	- Finally, if there are still intervals left, we can peek at the top of the heap and add that length to the index positions that correspond to those queries 
- Return the answer array
## Edge Cases/Gotchas 
- Need to sort but also need to keep track of query position for return answer
- We can keep old intervals in the heap if they are sufficiently large, since we would never encounter them when peeking at the top of the heap
- We need to be able to deal with multiple queries that are the same, so there is not a 1:1 mapping of query and index
## Solution
```python 
class Solution:
    def minInterval(self, intervals: List[List[int]], queries: List[int]) -> List[int]:
        intervals.sort()
        # Keep track of queries indicies to use for answer
        query_map = defaultdict(list)
        for i, q in enumerate(queries):
            query_map[q].append(i)
        queries.sort()
        # Build answer
        ans = [-1]*len(queries)
        heap = []
        i = 0
        for query in sorted(query_map.keys()):
            # Add all inter that starts at or before query
            while i < len(intervals) and intervals[i][0] <= query :
                length = intervals[i][1] - intervals[i][0] + 1
                heapq.heappush(heap, (length, intervals[i][1]))
                i += 1
            # Pop heap until top is valid
            while heap and heap[0][1] < query:
                heapq.heappop(heap)
            # If valid interval, build answer
            if heap:
                for index in query_map[query]:
                    ans[index] = heap[0][0]
        return ans
```
## Time Complexity
$O(n*log(n) + q*log(q))$ since the bulk of processing comes from the sorting of the intervals, and the actual algorithm runs in $O(n+q)$
## Space Complexity
$O(n+q)$ since we potentially store all intervals in the heap, and potentially store all queries in the hash map to track indicies 
