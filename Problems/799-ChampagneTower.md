---
tags: [array, dynamic_programming]
---
### 799. Champagne Tower

Link: [here](https://leetcode.com/problems/champagne-tower/description/)

#### Problem
We stack glasses in a pyramid, where the **first** row has `1` glass, the **second** row has `2` glasses, and so on until the 100th row.  Each glass holds one cup of champagne.

Then, some champagne is poured into the first glass at the top.  When the topmost glass is full, any excess liquid poured will fall equally to the glass immediately to the left and right of it.  When those glasses become full, any excess champagne will fall equally to the left and right of those glasses, and so on.  (A glass at the bottom row has its excess champagne fall on the floor.)

For example, after one cup of champagne is poured, the top most glass is full.  After two cups of champagne are poured, the two glasses on the second row are half full.  After three cups of champagne are poured, those two cups become full - there are 3 full glasses total now.  After four cups of champagne are poured, the third row has the middle glass half full, and the two outside glasses are a quarter full, as pictured below.
![[champagne.png|300]]
Now after pouring some non-negative integer cups of champagne, return how full the `jth` glass in the `ith` row is (both `i` and `j` are 0-indexed.)

#### Main Idea
The main idea is that we can simulate the total number of pours for each row, to determine how much will end up in the next row. This is technically dynamic programming.

#### Approach
We make a matrix to represent the total number of rows that we need. Then we set the first row to the total number of bottles poured. Now it is time to iterate through the array. At each glass we look at how much is in it, and then split the extra in two, and add the overflow into the next level's glass. 
The trickiest part of this problem is dealing with array indicies. 

We can optimize the solution to use fewer rows, since we don't care about the amount of champagne flowing into glasses that will never spill in the glass we care about. 

#### Edge Cases
- A cup coordinate that makes no sense or is null.

#### Solution
```python 
class Solution:
    def champagneTower(self, poured: int, query_row: int, query_glass: int) -> float:
        # First we init the tower
        tower = [[0] * k for k in range(1, query_row+2)]
        # Then we add poured amount of drink to the top
        tower[0][0] = poured
        # Now we go through each row in the tower
        for row in range(query_row):
            # For each cell, check overflow
            for col in range(row+1):
                overflow = (tower[row][col] - 1.0) / 2.0
                # If there is overflow, add it to the cups below
                if overflow > 0:
                    tower[row+1][col] += overflow
                    tower[row+1][col+1] += overflow
        # Finally we return the glass or 1 if it would overflow
        return min(1, tower[query_row][query_glass])
```

#### Time Complexity
`O(n^2)` where `n` is the `query_glass` value.

#### Space Complexity
`O(n^2)` since we fill in the entire array.

