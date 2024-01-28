---
tags:
  - medium
  - binary_search
  - sorting
  - array
---
### 826. Most Profit Assigning Work

Link: [here](https://leetcode.com/problems/most-profit-assigning-work/description/)

#### Problem
You have `n` jobs and `m` workers. You are given three arrays: `difficulty`, `profit`, and `worker` where:
- `difficulty[i]` and `profit[i]` are the difficulty and the profit of the `ith` job, and
- `worker[j]` is the ability of `jth` worker (i.e., the `jth` worker can only complete a job with difficulty at most `worker[j]`).

Every worker can be assigned **at most one job**, but one job can be **completed multiple times**.
- For example, if three workers attempt the same job that pays `$1`, then the total profit will be `$3`. If a worker cannot complete any job, their profit is `$0`.

Return the maximum profit we can achieve after assigning the workers to the jobs.

#### Main Idea
- We want to match each worker with the highest paying job that they can handle
- Sorting by difficulty lets use use binary search for each worker's task
- We can set the pay for each job equal to the highest pay someone can get at that difficulty **or less** in-case a job pays well and is low-difficulty 

#### Approach
- Zip the difficulty and pay lists together and sort by difficulty
- Set the pay to the best for that difficulty for each job
- Do binary search for each worker to find highest difficulty 
- Once job is found update total pay

#### Edge Cases
- Empty inputs
- Cases where the job may pay more, but be less difficult, there is no correlation between the two

#### Solution
```python 
class Solution:
    def maxProfitAssignment(self, difficulty: List[int], profit: List[int], worker: List[int]) -> int:
        jobs = sorted(zip(difficulty, profit))
        best_profit = ans = 0
        for i in range(len(jobs)):
            best_profit = max(best_profit, jobs[i][1])
            jobs[i] = (jobs[i][0], best_profit)
        # Do binary search to find largest diff < target
        for ability in worker:
            l, r = 0, len(jobs)-1
            while l <= r:
                mid = (l + r)//2
                # If valid update midpoint
                if (mid == len(jobs)-1 and jobs[mid][0] <= ability) or jobs[mid][0] <= ability and jobs[mid+1][0] > ability:
                    ans += jobs[mid][1]
                    l = r
                    l += 1
                elif jobs[mid][0] <= ability:
                    l = mid+1
                else:
                    r = mid-1
        return ans
```

#### Time Complexity
`O(mlog(m) + nlog(n))` since we sort the input and then do a binary search `log(n)` for `n` workers

#### Space Complexity
`O(m)` where `m == len(jobs)`
