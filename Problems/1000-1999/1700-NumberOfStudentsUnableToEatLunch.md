---
tags:
  - easy
  - queue
---
# 1700. Number of Students Unable to Eat Lunch
Link: [here](https://leetcode.com/problems/number-of-students-unable-to-eat-lunch/)
## Problem
The school cafeteria offers circular and square sandwiches at lunch break, referred to by numbers `0` and `1` respectively. All students stand in a queue. Each student either prefers square or circular sandwiches.

The number of sandwiches in the cafeteria is equal to the number of students. The sandwiches are placed in a **stack**. At each step:
- If the student at the front of the queue **prefers** the sandwich on the top of the stack, they will **take it** and leave the queue.
- Otherwise, they will **leave it** and go to the queue's end.

This continues until none of the queue students want to take the top sandwich and are thus unable to eat.

You are given two integer arrays `students` and `sandwiches` where `sandwiches[i]` is the type of the `i​​​​​​th` sandwich in the stack (`i = 0` is the top of the stack) and `students[j]` is the preference of the `j​​​​​​th` student in the initial queue (`j = 0` is the front of the queue). Return _the number of students that are unable to eat._
## Intuition
- We could just simulate this, and wait for either all the students to be served, or for all students still in line to pass up the sandwich
- What causes an end condition? If all the students in the queue don't want the top sandwich, then we stop
- This means if we serve all of one preference, and then get one more of those types of sandwiches, we can just return the amount of the queue/count of students who weren't served
## Approach
- Get the count of the number of students who want circles and squares
- Iterate through the sandwich array
- Every time you see a 0 or 1, decrement the appropriate student counter to simulate them being given the sandwich
- If we reach a point where we want to serve a sandwich, but there are no more students who want that sandwich, it means that we are done, since none of the other students will remove the sandwich at the top of the stack, so just return the remaining count
- If we made it through all the sandwiches then return 0
## Edge Cases/Pitfalls
- It's important to know that everyone won't get served in some cases and the people who aren't served can be more than just the difference of the counts and sandwiches 
## Solution
```python 
class Solution:
    def countStudents(self, students: List[int], sandwiches: List[int]) -> int:
        freq = Counter(students)
        stud_c, stud_s = freq[0], freq[1]

        for sand in sandwiches:
            if sand == 0:
                if stud_c == 0:
                    return stud_s
                stud_c -= 1
            else:
                if stud_s == 0:
                    return stud_c
                stud_s -= 1
        return 0
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
## Takeaways 
- Think about edge cases in the problem, maybe walk through the example to see how your logic might be wrong before jumping into the problem 
