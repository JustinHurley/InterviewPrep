---
tags:
- algorithm
- stack
- monotonic_stack
---
### Monotonic Stack
A monotonic stack is a stack where each value is either the same or bigger (increasing), or the same or less (decreasing), relative to the current value. This means that the stack remains sorted as work is done on it.
Monotonic stacks are useful when we need to "remember" values for later and only care when they see a value less or greater than that value.

#### General Idea
Consider the scenario where we are given an int array and want to find the distance from our current value to the next larger value. 
Because a larger value could be 1,2,3,... elts away or not even exist as a solution, we need a way to keep track of all the values that haven't "found" a larger value yet. 
This is when the monotonic stack (decreasing) in this case comes into play. The general idea behind it is that we add values to the stack when they are less than the top of the stack, but when they are larger than the top of the stack, we could pop the elements of the stack until we find a value that is larger than the current value that we have. 
So consider the example stack `[(7,0),(7,1),(6,2),(5,3),(4,4),(3,5)]` where the 1st val is the value, and the 2nd val is the index. So far, every val has been `<=` the next one, so we just add it to the stack. We are moving through our input array and when `i == 6` we have a val of 6. 
So now we look at the top of the stack and see if `6 > 3` which it is, so we pop 3 and compare the diff in indexes, and end up with 1, so we know that at index 5, it is one away from a higher val. But we aren't done, we need to keep looking at the top of the stack, so we look at `(4,4)` which is also less, so we take the diff in indexes (2) and now know how far away that val is from the current val. We repeat this process until we hit 6. Now `6 == 6` so we stop popping and add our val to the stack, which now looks like: `[(7,0),(7,1),(6,2),(6,6)]`. Now if the next val is 8, we can process the difference in vals for each of the remaining elts in the stack. 
So as you can see, we are able to keep track of values who haven't had a condition met, and immediately process the ones that have had a condition met, without having to scan the input array over and over. This approach ensures that each val is added to the stack at most once, and therefore is only ever accessed twice, which gives us a general runtime complexity of `O(2n) ->  O(n)`.

#### Psuedocode for Monotonically Decreasing Stack
```
input = [...]
stack = []
ans = [?]

for index, val in input:
    while stack is not empty and val > stack[-1]:
        popped = stack.pop()
        do some work to get an ans and capture it
    stack.add([index,val])

return ans
```