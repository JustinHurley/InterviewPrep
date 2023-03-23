### 853. Car Fleet

Link: [here](https://leetcode.com/problems/car-fleet/description/)

#### Topics
- Stack
- Monotonic stack
- Sorting
- Array

#### Problem
There are n cars going to the same destination along a one-lane road. The destination is target miles away.

You are given two integer array `position` and `speed`, both of length n, where `position[i]` is the position of the `ith` car and `speed[i]` is the speed of the `ith` car (in miles per hour).

A car can never pass another car ahead of it, but it can catch up to it and drive bumper to bumper at the same speed. The faster car will slow down to match the slower car's speed. The distance between these two cars is ignored (i.e., they are assumed to have the same position).

A car fleet is some non-empty set of cars driving at the same position and same speed. Note that a single car is also a car fleet.

If a car catches up to a car fleet right at the destination point, it will still be considered as one car fleet.

Return the number of car fleets that will arrive at the destination.

#### Approach
The general approach is to use a monotonic stack, where we keep track of all the cars that will meet up with one another. The general approach is this:
1. Check the stack for elements.
2. If it's not empty look at the TOS.
3. If the TOS car will meet with the current car, pop that car from the stack.
4. Do this until you reach a car in the stack that won't meet.
5. Add the current car to the stack.
6. Repeat for each element in the array.
It is also important that we sort the stack. This is because we want to avoid scenarios where cars can get buried in the stack. This can happen when you have a car in the stack that is buried under a car that is faster and closer to the target than the stack shown. So we end up with 2 cars in the stack that will never meet, but the further out car is buried under the closer car. So then when we look at the next car, we end up in a scenario where we won't catch the TOS car but would catch the first car in the stack. This is why we need to sort by distance to the target and work our way from the further cars in.
#### Solution
```
class Solution:
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        n = len(speed)
        stack = []

        sort = sorted(zip(position, speed))

        def carsWillMeet(i, j):
            dist1 = target - sort[i][0]
            dist2 = target - sort[j][0]

            time1 = dist1/sort[i][1]
            time2 = dist2/sort[j][1]

            if (time1 >= time2 and dist1 <= dist2) or (time1 <= time2 and dist1 >= dist2):
                return True
            return False

        for i in range(n):
            while stack and carsWillMeet(i,stack[-1]):
                stack.pop()
            stack.append(i)
        
        return len(stack)
```

#### Time Complexity

