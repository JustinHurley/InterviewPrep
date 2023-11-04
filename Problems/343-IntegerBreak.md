---
tags: [recursion, design, dynamic_progreamming]
---
### 343. Integer Break

Link: [here](https://leetcode.com/problems/integer-break/description)

#### Problem
Given an integer `n`, break it into the sum of `k` positive integers, where `k >= 2`, and maximize the product of those integers.
Return the maximum product you can get.

#### Main Idea
We need to use [[dynamic_programming|dyanmic programming]] to determine what the best split case is for each value less than `n`.

#### Approach
So first off, we need to split the number into at least 2, so we'll need to handle the edge cases where `n` is 2 or 3, since we'll have to split here, although we don't want to, since `2 => 1*1`, and `3 => 2*1`.
Then, we'll want to use DFS to navigate the decision tree of splitting up numbers. We also want to use a memo table to store the values that we determine, to prevent repeat work.
So first we check the base-case, when `n` is 3 or 2, since we want to stop splitting there. If it's not, then we check to see if it's in the memo table already. 
If we need to calculate it, we want to determine the maximum possible split of numbers. To do that we just iterate through a for loop to compare the current `n` to `i * helper(n-i)` and choose whichever is larger.

#### Edge Cases
- Negative numbers
- When the input is less than 4
- When the input is 0 or 1

#### Solution
```python 
class Solution:
    def __init__(self):
        # Adding memo table
        self.memo = {}

    def integerBreak(self, n: int) -> int:
        # Handle edge case if 3 or 2
        if n == 3:
            return 2
        if n == 2:
            return 1
        return self.helper(n)
        
    def helper(self, n):
        # If n <= 3, stop breaking down since it will make it worse
        if n <= 3:
            return n
        # Check memo table
        if n in self.map:
            return self.memo[n]
        ans = n
        # See if we can do better by breaking down
        for i in range (2, n):
            ans = max(ans, i*self.helper(n - i))
        # Set ans for n and return
        self.memo[n] = ans
        return ans
```

#### Time Complexity
`O(n^2)`, since each `helper` call could run the for loop, and `helper` is called `n` times.

#### Space Complexity
`O(n)`
