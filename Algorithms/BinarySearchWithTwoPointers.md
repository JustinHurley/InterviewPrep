---
tags: [algorithm, two_pointer, binary_search]
---
### Binary Search Using the Two Pointer Approach

Binary search is a very useful algorithm that can be used when needing to find a value in a **sorted** array. The two pointer approach is one of the ways to achieve this algorithm. The other approach is using recursive functions, but will not be detailed in this file.

#### General Idea
The general idea of this algorithm is that we can move the left and the right items to find a value in a data set by taking the middle of the current left and right values and seeing how it compares to the target value. This allows us to half the data set each time, leading to `O(log(n))` performance time, instead of a normal scan operation that would take `O(n)` time.
Consider the array: 
```
arr = [1,2,3,3,4,5,6,7]
```
We can set the left index to be `0` and the right index to be `6` or (`len(arr)-1`). 
Now, let's say we want to see if the number `2` exists in the array. To get an idea of what half of the array we need to look at, let's look at the value at the middle index of the left and right indexes. We can calculate this by doing 
```
mid = (l+r)//2 => (0+6)//2 => 3
``` 
We need to do an integer divide so we don't end up with a float index.
The next step is to look at the index, in this case `3`, which corresponds to 3. Now we know the array is sorted, and if we're looking at the midpoint with a value of 3, we know that the value we want has to be to the left of the midpoint, and just like that we have halved the number of potential answers.
So we then move `l = mid-1` since we know that the value has to be between the left index and the midpoint minus 1, since we already checked the midpoint.
Now we are only looking at `[1,2,3]`, where `l = 0` and `r = 2`. We take the midpoint which is `1`, then check the value at that index, which is `2`, our target value, and are done!

#### Code
```python
# Input
nums = [1,2,3,3,4,5,6,7]
target = 2

l = 0
r  = len(nums) - 1

while l <= r:
    mid = (l+r)//2

    # If the midpoint val is equal, return the index
    if nums[mid] == target:
        return mid
    # If the midpoint val is greater than the target, look on the left half
    elif nums[mid] > target:
        r = mid - 1
    # Otherwise look on the right half
    else:
        l = mid + 1

```