### 155. Min Stack

Link: [here](https://leetcode.com/problems/min-stack/description/)

#### Topics
- Stack
- Array
- Design

#### Problem
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the MinStack class:

`MinStack()` initializes the stack object.
`void push(int val)` pushes the element `val` onto the stack.
`void pop()` removes the element on the top of the stack.
`int top()` gets the top element of the stack.
`int getMin()` retrieves the minimum element in the stack.
You must implement a solution with `O(1)` time complexity for each function.

#### Approach
So this problem is relatively trivial (especially in Python), as most of the operations involve adding to an array or looking at a last element. The only tricky part of the problem is implementing the `getMin()` function and having it run in `O(1)` time, as our stack will not inherently keep track of the min and based on the time constraint, we can't afford to look through the entire array without running too slowly.
The solution to this issue is to keep track of the current minimum for each element added to the stack in a separate stack. This allows us to add and pop elements without having to worry about updating the minimum, or finding out that there was a duplicate somewhere that we didn't account for. 
To implement this we use 2 stacks, the actual stack and the minimum stack. The actual stack works as expected, but the minimum stack will instead compare itself with the top-of-stack value to see if it is smaller than the TOS value, or if it isn't smaller and the new TOS value will remain the same. 
Let's consider the example of having the input: `[put 4, put 2, pop, put 3, put 5]`. We will have 2 stacks the actual and the min. So first we add `4` to the stack, and since it's the only val, the minimum is also `4`. Then we add `2` so we end up with `[4,2]` in our stack, but we have a new min. So we compare the current value, `2` with the TOS, `4` and see which is smaller. Since `2` is smaller it goes at the TOS so the min stack is also `[4,2]`. Now we get a pop operation, so stack goes to `[4]` and we also just pop the min so it's now `[4]`. Next we add `3` which is smaller so we end up with `[4,3]` for both. Finally we add `5` so the regular stack is `[4,3,5]` but we see that `5 > 3` so when we look to the TOS min stack to see, we want to maintain that `3` is still smaller so we add `3` to the min stack. Ending up with `[4,3,5]` for the regular stack and `[4,3,3]` for the min stack. The reason this works is because we are adding the minimum relative value to the stack at the time, but we don't remove previous values so as we pop from the stack, we effectively "go back an iteration" to before we had just seen that number and potential new min.

#### Solution
```
class MinStack:
    def __init__(self):
        self.stack = []
        self.minimum = []
        
    def push(self, val: int) -> None:
        self.stack.append(val)
        # Set the val to the min of itself or the top of the min stack
        # But only if there is a val in the min stack already
        val = min(val, self.minimum[-1] if self.minimum val)
        self.minimum.append(val)
        
    def pop(self) -> None:
        self.stack.pop()
        self.minimum.pop()
        
    def top(self) -> int:
        return self.stack[-1]

    def getMin(self) -> int:
        return self.minimum[-1]
```
Note: `array[-1]` is how you get the last element in a list.

#### Time Complexity
`O(1)` for all operations, `O(n)` to process all operations where `n` is the number of operations.

