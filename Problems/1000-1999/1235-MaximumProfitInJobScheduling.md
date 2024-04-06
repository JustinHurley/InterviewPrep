---
tags:
  - hard
  - dynamic_programming
  - interval
  - binary_search
  - memoization
  - depth_first_search
---
### 1235. Maximum Profit in Job Scheduling

Link: [here](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)

#### Problem
We have `n` jobs, where every job is scheduled to be done from `startTime[i]` to `endTime[i]`, obtaining a profit of `profit[i]`.

You're given the `startTime`, `endTime` and `profit` arrays, return the maximum profit you can take such that there are no two jobs in the subset with overlapping time range.

If you choose a job that ends at time `X` you will be able to start another job that starts at time `X`.

#### Main Idea
- We can use DP to navigate the decision tree of choosing to add or not add the current interval
- To speed up finding the next valid interval, we can use binary search

#### Approach
- First we will organize the data by zipping all 3 input arrays together, and then sorting the array (which will sort by the first element in each array by default)
- Make a memo table
- Make the DFS method, which will take an index parameter argument
- If the given input DNE in the memo table, we will try to calculate the best choice for that interval
- At each interval we can either choose to include it or choose to not include it
- Solving for `max(dfs(next_element), dfs(next_valid_element) + curr_profit)
- To find what the next valid element is, we can do binary search on our data, to find the next starting point that starts after the current event has ended.
- It's important that whatever node we add is valid to maintain a valid state while we DFS
- Update the memo table and return the answer

#### Edge Cases/Gotchas 
- Invalid data inputs, invalid dataset lengths
- Knowing that you can use `bisect.bisect_left` to do binary search is just something you need to know

#### Solution
```python 
class Solution:
    def jobScheduling(self, startTime: List[int], endTime: List[int], profit: List[int]) -> int:
        n = len(startTime)
        # First we need to stort the data by start
        data = sorted(list(zip(startTime, endTime, profit)))
        memo = {}

        def dfs(i):
            if i in memo:
                return memo[i]
            else:
                # Check if past end of list, means nxt overshot 
                if i >= n:
                    return 0
                # Try to get skip 
                skip = dfs(i+1)
                # Find next valid interval index
                nxt = bisect.bisect_left(data, data[i][1], key=lambda x: x[0])
                # Make the next call for if this value was chosen
                add = dfs(nxt) + data[i][2]
                memo[i] = max(skip, add)
                return max(skip, add)
        return dfs(0)
```

#### Time Complexity
`O(nlog(n))` which comes from sorting, and we know that for each `dfs` call we only actually do the work once, and all subsequent calls are memoized so that's `2n` work, so the `nlog(n)` term still dominates.
#### Space Complexity
`O(n)` for the memo table and if you want to count zipping the input together as using more space 
