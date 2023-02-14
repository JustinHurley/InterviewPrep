### 42. Trapping Rain Water

Link: [here](https://leetcode.com/problems/trapping-rain-water/description/)

#### Topics
- Dynamic programming 
- Array
- Two pointers

#### Problem
Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

#### Approach
**Two Pointers**
The brute force solution would be to just find the furthest elements for a given height and then count all the spaces between the ends that are within the height. This would lead to an `O(n*maxHeight)` solution, as we need to do an iteration based on the height and also iterate through each element.

Another way to approach this problem is with two pointers, leveraging the known height of the other side and the max height of a given side.

The general approach is to set a left and right pointer at the start and end of the list, and to also keep track of the max left and max right heights. On a given iteration, we look to see which pointer is at a lower height, and to move that pointer in by 1. While we are doing this, we also see if we need to set a new max height for the left or right, and then compare the current max height for that side with the given height we are looking at. 

For example, let's consider the scenario where `height[l] = 11, height[r] = 14` and `maxLeftHeight = 13`. We first compare left and right, and see that we need to increment the left pointer. Before we do this though, we need to get the submerged area for the current left index. To do this we simply take `maxLeftHeight - height[l]` which is 2. Now it makes sense that because the left side has already passed a larger height, that the water will be contained in the container relative to the left side, but how can we "trust" that the right side will also contain the water? Well we know that because of how the algorithm chooses what side to increment. In this case, the left side is less than the right which is why it was incremented in the first place, which is basically filling in for the `min` part of taking the minimum of the max relative left and right values (see below formula if this doesn't make sense). 

**Dynamic Programming**
This is actually an approach that is less optimized than the above approach since it also uses `O(n)` memory, but I think it is a good example of using DP concepts.
So for a given problem, we can think about a single instance of `height[i]` and how to determine the amount of water it can hold in that index. This is simply done by taking the max height to the left and right, and then taking the min of that height, and then finally subtracting the current height from that value. So the water volume calculations would look like:
```
waterVolume = min(maxLeftHeight, maxRightHeight) - height[i]
```
Now we could calculate this for each element as we iterate through the array, but that would be time consuming and contribute to a time complexity of `O(n)`, which is no good. 
Alternatively, we can use the dynamic programming concept of memoization, which is to save already calculated work for later to help us with the problem. 
We can do 2 scans of the list, one scan left to right and the other in the opposite direction, to determine the max heights relative to each element, this is done by keeping track of the max and then having a corresponding array that keeps track of the left max and right max at each position. Once we do this, it's a matter of iterating through the arrays and at each position, grabbing the current height, and the relative left and right maxes, then finally using the defined formula above to get the water volume for that height, which gives us a `O(n)` solution. The two pointer approach basically uses the same logic, just keeps track of the maxes via variables instead of using a whole array. 
#### NeetCode Link
https://www.youtube.com/watch?v=ZI2z5pq0TqA&t=23s

#### Solution
```
class Solution:
    def trap(self, height: List[int]) -> int:
        length = len(height)
        ans = 0

        l,r = 0, length -1
        max_l, max_r = 0, 0

        # Start on either end
        while l < r:
            # Want to move whichever is smaller since we know it will be contained inside the other side at least
            if height[l] < height[r]:
                max_l = max(max_l, height[l])
                ans += max_l - height[l]
                l += 1
            else:
                max_r = max(max_r, height[r])
                ans += max_r - height[r]
                r -= 1
        return ans
```

