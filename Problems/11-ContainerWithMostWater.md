---
tags: [two_pointer, greedy_choice, array]
---
### 11. Container With Most Water
Link [here](https://leetcode.com/problems/container-with-most-water/)

To solve this problem, we simply use 2 pointers on each end. First we calculate the area, compare with the current global max area and replace if necessary. After we calculate area, we move one of the pointers in, based on which pointer has the lower height. This greedy choice ensures that we maximize height as we look for a solution.

**Why does the greedy choice work?**
Let's consider the binary choice of choosing either the left or right pointer to increment in a scenario where `height[l] = 5` and `height[r] = 3`. We don't know what the next new height could be and it could either be greater than the heights we currently have or it could be less, either way, we aren't sure which one is best. There is one thing we do know, if we increment the left pointer, we will still be limited by the min height of either pointer, so no matter what the next pointer is, it can have a max height of 3. If we increment the right pointer, we are limiting the max height of the water to 5, which is clearly better. 
The greedy choice works as it ensures we are maximizing the max potential height at east situation, and by starting with pointers at the beginning and end of the list, we ensure we are starting with the max possible width, and choosing the height that will maximize the area when provided the binary choice.

Logic
- Compute area and compare to global max
- If h[l] < h[r] then move the left pointer up
- Otherwise move the right side up

Solution
```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        n = len(height)
        l = 0
        r = n-1
        ans = 0
        # While left is on the left and right is on the right
        while l < r:
            # First lets get the area
            h = min(height[l],height[r])
            w = r - l
            area = w*h
            ans = max(ans, area)
            # Decide which pointer to move forward
            if height[l] < height[r]:
                l += 1
            else:
                r -= 1
        return ans
```