### Sliding Window

The sliding window algorithm is useful when we are given a situation in which we are looking for a contiguous subset of a list, that can be a variable length. Sliding windows allow for `O(n)` time complexity, as we are able to pass through an array only once per left and right pointer.

#### General Idea
The general idea of the algorithm is that we can move the right pointer through the list until some condition is violated, and then we move the left pointer up until the condition is no longer violated. This saves us the trouble of needing to check all the possible options starting at each element in the data set, which would leave us with a `O(n^2)` time complexity, and instead can greedily change the size of the "window" we are currently looking at based on if we can better our solution or not without violating any criteria. 

#### Optimizations
A lot of times these problems involve looking at a string or some similar item, where you end up with a hash map that you iterate through to check for validity. Often it means adding `O(26)` work which is trivial from a time-complexity standpoint but is 26 times slower than a truly `O(1)` operation. 
We can work around this by not directly comparing two hash maps and instead keeping track of how many valid items we have.
Say for example we are trying to find the shortest contiguous substring `abc` in `aaaabbbcccccc`. Instead of making hash maps and iterating through to make sure all the values are equal to or greater than the values in the target map, we could instead keep count. We know that there are 3 chars that need to be validated, and every time we find that `map1[i] == map2[i]` we cna just increment the "valid" counter. We can also decrement the valid counter as the left pointer moves up, and this allows us to determine if we're in a valid state in `O(1)` time instead of `O(26)` time or `O(n)` time if there are no constraints to the values being scanned, e.g. looking at numbers instead of chars. 

#### Psuedocode
```
l = 0
r = 0
input = [a,b,c,b,b,c,a,b]

while r < len(input):
    # If no condition is violated, business as usual
    if (condition is NOT violated):
        do: build current solution 
        r += 1
    # If condition is violated
    else:
        while (condition is violated):
            do: work to remove input[l] from current answer
            if l >= length:
                return ans 
            l += 1
    return ans
    
```
alternatively
```
l = 0
input = [a,b,c,b,b,c,a,b]

for r in range(input):
    do: work

    while isValid() == False:
        do: remove input[l] from current tracking 
        l += 1

    best = getWhichIsBetter(best, currBest)

return best
```