---
tags: [queue, heap, array, sorting]
---
### 621. Task Scheduler

Link: [here](https://leetcode.com/problems/task-scheduler/description/)

#### Problem
Given a characters array `tasks`, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle.

However, there is a non-negative integer `n` that represents the cooldown period between two **same tasks** (the same letter in the array), that is that there must be at least `n` units of time between any two same tasks.

Return _the least number of units of times that the CPU will take to finish all the given tasks_.

#### Main Idea
You want to do the most frequent tasks first, which can be stored with a heap. When tasks are on cooldown, use a queue to track which ones will be ready to be added back to the heap and executed. 
#### Approach
The solution to this task is to simulate it. We first want to determine which task should be done first. The strategy for choosing a task is based on wanting to avoid the worst case scenario: only being able to do 1 task at a time. To avoid this, we want to proactively do the most frequent tasks first, to ensure we avoid the worst case scenario if possible. To do this, we want a way to easily access and track the most frequent tasks, which we can do with a heap. 
So to get frequency, we count the frequency of each char in the `tasks` array, and then with that we can discard the chars and put the frequencies into a [[heap]] and heapify it. Then we basically just simulate the tasks with a while loop, keeping track of time with a counter. Once that is completed, and all tasks have been fully completed, you can return the time as the answer. 

#### Edge Cases
No edge cases for this problem on LC, but some to think about were dealing with an empty array, getting a negative cooldown time, and getting a time close to the integer overflow value.

#### Solution
```python 

class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        # Count the tasks
        counts = Counter(tasks)
        # Now convert to heap
        heap = [-e for e in counts.values()]
        # Finally heapify
        heapq.heapify(heap)
        # Now make queue
        Q = deque()
        t = 0

        while heap or Q:
            # Advance time w/ task
            t += 1
            # Pop heap if possible
            if heap:
                curr = 1 + heapq.heappop(heap)
                # "Invoke" and add to cooldown if valid
                if curr < 0:
                    # Remove the count by 1 and mark cooldown time
                    Q.append([curr, t+n])
            # Check Q to see if you can do anything
            if Q and Q[0][1] <= t:
                heapq.heappush(heap, Q.popleft()[0]) 
        return t
```
#### Time Complexity
`O(t*n)`, since you could have a very large count of only one task. There is slight optimization around this, by using counts and the queue to determine the final time. 

#### Space Complexity
`O(n)` since you make a heap and a queue, and use the counter. 

