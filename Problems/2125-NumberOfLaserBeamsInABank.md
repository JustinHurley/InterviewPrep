---
tags:
  - medium
  - math
  - matrix
  - greedy
  - array
---
### 2125. Number of Laser Beams in a Bank

Link: [here](https://leetcode.com/problems/number-of-laser-beams-in-a-bank/description/)

#### Problem
Anti-theft security devices are activated inside a bank. You are given a **0-indexed** binary string array `bank` representing the floor plan of the bank, which is an `m x n` 2D matrix. `bank[i]` represents the `ith` row, consisting of `'0'`s and `'1'`s. `'0'` means the cell is empty, while`'1'` means the cell has a security device.

There is **one** laser beam between any **two** security devices **if both** conditions are met:

- The two devices are located on two **different rows**: `r1` and `r2`, where `r1 < r2`.
- For **each** row `i` where `r1 < i < r2`, there are **no security devices** in the `ith` row.

Laser beams are independent, i.e., one beam does not interfere nor join with another.

Return _the total number of laser beams in the bank_.

#### Main Idea
- Number of beams between rows = number of 1's in top row * number of 1's in bottom row
- You can just skip empty rows

#### Approach
- Solve the problem in one pass with a for loop by keeping track of the last seen row with more than 0 1's
- Update the answer when there was a previous row with a positive sum of 1's, and the current row's count is more than 0

#### Edge Cases
- Invalid data
- Empty dataset

#### Solution
```python 
class Solution:
    def numberOfBeams(self, bank: List[str]) -> int:
        rows = len(bank)
        ans = 0
        prevRow = 0
        for row in bank:
            currRow = row.count('1')
            # If lasers, check if we should count
            if currRow > 0 and prevRow > 0:
                ans += currRow * prevRow
                prevRow = currRow
            # If we don't need to count, just update prev
            elif currRow > 0:
                prevRow = currRow
        return ans```

#### Time Complexity
`O(m*n)` since we visit each char in each row

#### Space Complexity
`O(1)`

