---
tags: [medium, graph, depth_first_search, set, cycle, backtracking]
---
### 210. Course Schedule II

Link: [here](https://leetcode.com/problems/course-schedule-ii/description/)

#### Problem
There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return _the ordering of courses you should take to finish all courses_. If there are many valid answers, return **any** of them. If it is impossible to finish all courses, return **an empty array**.

#### Main Idea
- Turn the prerequisites into a graph, where the course points to it's prerequisites
- DFS and check for cycles using a set

#### Approach
- We need 2 sets, one to track visited and one to track for cycles
- Build an adjacency list
- Make a [[DepthFirstSearch|DFS]] method that takes a given class ID/index as a function parameter
- Use the sets to check if the current class has been taken already, or if we are in a cycle
- If not, then check all the prerequisite classes
- Once that's been done, we can take the current class and update the answer
- We must also remove the current class from the cycle detection set, we use backtracking with the set to keep track of all nodes visited on the current DFS path
- If we know we are in a cycle, we need to return `False` so we can return the default answer, otherwise just return `True`

#### Edge Cases
- Cycles are present
- Class has itself as prerequisite 
- Data is formatted improperly

#### Solution
```python 
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        graph = [[] for _ in range(numCourses)]
        ans = []
        taken, cycle = set(), set()

        for edge in prerequisites:
            graph[edge[0]].append(edge[1])

        def take(i):
            if i in cycle:
                return False
            if i in taken:
                return True
            cycle.add(i)
            # Make sure child classes are taken
            children = graph[i]
            for child in children:
                if not take(child):
                    return False
            cycle.remove(i)
            taken.add(i)
            ans.append(i)
            return True
            
        for i in range(numCourses):
            if i not in taken:
                if not take(i):
                    return []
        
        return ans
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)`

