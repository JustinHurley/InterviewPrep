### 11. Container With Most Water
Link [here](https://leetcode.com/problems/container-with-most-water/)

Topics
- 2 pointer
- Greedy choice
- Arrays

To solve this problem, we simply use 2 pointers on each end. First we calculate the area, compare with the current global max area and replace if necessary. After we calculate area, we move one of the pointers in, based on which pointer has the lower height. This greedy choice ensures that we maximize height as we look for a solution.

Logic
- Compute area and compare to global max
- If h[l] < h[r] then move the left pointer up
- Otherwise move the right side up

Solution
```
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