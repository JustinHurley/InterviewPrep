---
tags:
- bit_manipulation
- dynamic_programming
---

### 338. Counting Bits

Link: [here](https://leetcode.com/problems/counting-bits/description/)

#### Problem
Given an integer `n`, return an array ans of length `n + 1` such that for each `i (0 <= i <= n)`, `ans[i]` is the number of 1's in the binary representation of `i`.

#### Approach
So the brute force approach would be to just use the `bin()` function, and then count the ones for each value from `0...n`. This is fine, but we can get a more optional solution.
The main logical leap in this problem is understanding the similarities between binary values who differ by a power of 2. What that means is if we look at the binary representation for 7: `111` we can see that it's the same as the binary representation for 3: `11` but with a leading bit. So if we have already calculated the values from `[0, 7]`, we should then be able to get the values for `[8, 15]`, since it will be the same as `[0, 7]`, just with an extra 1 in the most significant bit.

#### Solution
```python 
class Solution:
    def countBits(self, n: int) -> List[int]:
        # Build our DP array
        dp = [0]*(n+1)
        # Establish an offset
        offset = 1
        for i in range(1,n+1):
            # If offset*2 == i, i.e. is there a new bit 
            if offset * 2 == i:
                # Then we update the offset
                offset *= 2
            # To get the value we look at the value offset by the most significant bit + 1
            dp[i] = dp[i-offset]+1
        return dp
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)`

