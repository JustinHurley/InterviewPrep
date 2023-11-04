---
tags: [array, stack]
---
### 1441. Build an Array with Stack Operations

Link: [here](https://leetcode.com/problems/build-an-array-with-stack-operations/)

#### Problem
You are given an integer array `target` and an integer `n`.

You have an empty stack with the two following operations:

- **`"Push"`**: pushes an integer to the top of the stack.
- **`"Pop"`**: removes the integer on the top of the stack.

You also have a stream of the integers in the range `[1, n]`.

Use the two stack operations to make the numbers in the stack (from the bottom to the top) equal to `target`. You should follow the following rules:

- If the stream of the integers is not empty, pick the next integer from the stream and push it to the top of the stack.
- If the stack is not empty, pop the integer at the top of the stack.
- If, at any moment, the elements in the stack (from the bottom to the top) are equal to `target`, do not read new integers from the stream and do not do more operations on the stack.

Return _the stack operations needed to build_ `target` following the mentioned rules. If there are multiple valid answers, return **any of them**.

#### Main Idea
- `target` should be sequential, when it's not we know elements were popped

#### Approach
You just need to iterate through the given array, when there is a gap between the previous and current number, it means that all the numbers between current and previous were pushed and then popped, which we must account for in the answer array.

#### Edge Cases
- the input array is larger than `n`
- the target is invalid
- `n` is negative

#### Solution
```python 
class Solution:
    def buildArray(self, target: List[int], n: int) -> List[str]:
        ans = []
        prev = 0

        for num in target:
            # If prev+1 != num, then something was popped
            if num > prev+1:
                # Account for popped items
                ans += ["Push", "Pop"]*(num-prev-1)
                prev = num-1
            # Now add num to stack
            ans.append("Push")
            prev += 1
        return ans
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)` if you aren't counting the answer array, which is required by the problem

