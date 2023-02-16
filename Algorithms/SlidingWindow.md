### Sliding Window

The sliding window algorithm is useful when we are given a situation in which we are looking for a contiguous subset of a list, that can be a variable length. Sliding windows allow for `O(n)` time complexity, as we are able to pass through an array only once per left and right pointer.

#### General Idea
The general idea of the algorithm is that we can move the right pointer through the list until some condition is violated, and then we move the left pointer up until the condition is no longer violated. This saves us the trouble of needing to check all the possible options starting at each element in the data set, which would leave us with a `O(n^2)` time complexity, and instead can greedily change the size of the "window" we are currently looking at based on if we can better our solution or not without violating any criteria. 

#### Psuedocode
```
l = 0
r = 0
input = [a,b,c,b,b,c,a,b]

while r < len(input):
    # If no condition is violated, business as usual
    if (condition is NOT violated):
        build best solution 
        r += 1
    # If condition is violated
    else:
        while (condition is violated):
            do work to remove input[l] from current answer
            if l >= length:
                return ans 
            l += 1
    return ans
    
```