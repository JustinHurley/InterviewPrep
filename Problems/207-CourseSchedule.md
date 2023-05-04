### 207. Course Schedule

Link: [here](https://leetcode.com/problems/course-schedule/description/)

#### Topics
- Depth-first Search
- Graph
- Array
- Hash Table

#### Problem
There are a total of numCourses courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array prerequisites where `prerequisites[i] = [ai, bi]` indicates that you must take course bi first if you want to take course ai.

For example, the pair `[0, 1]`, indicates that to take course 0 you have to first take course 1.
Return `true` if you can finish all courses. Otherwise, return `false`.

#### Approach
If we look at how the information is given, we can see how the `prerequisites` list can easily be translated to a adjacency list. We can then use that list to traverse the array to determine validity. The only time a course is not possible to take, is when there is a cycle in the graph, as a person would then need to take a course as a prerequisite for itself.
So this problem is really a cycle detection problem, we don't need to worry about the edge case of a prerequisite course not existing or being out of the range of courses. 
So to detect cycles, all we need to do is build the adjacency list into a dictionary, with the values being the list of prerequisite courses, and then run DFS on each node to determine that there isn't a cycle.
This is done by first checking to see if we are looking at an already visited node, which would imply a cycle, and we would return false. We also want to check to see if the current node has no children, because if it has no children, then we are looking at a valid course path.
Once the base case checks are done, we visit the current node, call DFS on the children, and return false if any of them are false. If not, we can unvisit the current node (since we are using one set to track this), and return true.
Finally in the main method, we iterate through every course to determine if there are cycles present for any of them. We need to iterate through every node because we don't necessarily know if the graph is fully connected, and don't want to miss any nodes. 

#### Solution
```
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        courses = { i:[] for i in range(numCourses)}

        # Now populate adj list
        for preq in prerequisites:
            # Set the postreq course for this item
            courses[preq[0]].append(preq[1])
        
        visit = set()

        def dfs(curr):
            # If visited, we're in a loop
            if curr in visit:
                return False
            # If no preqs then we can take course
            if courses[curr] == []:
                return True
            # Visit curr
            visit.add(curr)
            # Now dfs on prereqs
            for item in courses[curr]:
                # If any of prereqs are false, so is curr
                if not dfs(item):
                    return False
            # Remove curr for backtracking purposes 
            visit.remove(curr)
            # We can memoize using the fact that curr's prereqs are valid so curr is valid
            courses[curr] = []
            return True

        for crs in range(numCourses):
            # Check to see if each
            if not dfs(crs):
                return False
        
        return True
```

#### Time Complexity
`O(V + E)` where `V` is the number of courses, and `E` is the number of items in the prerequisite list.

#### Space Complexity
`O(V + E)` since we translate the info into a map, and also track visited in a set.