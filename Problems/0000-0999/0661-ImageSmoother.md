---
tags:
  - "#easy"
  - math
  - matrix
  - bit_manipulation
---
### 661. Image Smoother

Link: [here](https://leetcode.com/problems/image-smoother/description/)

#### Problem
An **image smoother** is a filter of the size `3 x 3` that can be applied to each cell of an image by rounding down the average of the cell and the eight surrounding cells (i.e., the average of the nine cells in the blue smoother). If one or more of the surrounding cells of a cell is not present, we do not consider it in the average (i.e., the average of the four cells in the red smoother).

#### Main Idea
- Calculate the floor of the average of every cell and save in a matrix

#### Approach
- See above, the approach is pretty straightforward
- You can make a matrix of a set size in Python using:
  `matrix = [[0 for _ in range(cols)] for _ in range(rows)]`
- Make sure to not take averages of OOB coordinates

#### Space-Optimized Approach
- Instead of making an answer array to hold all the values, there is a way we can encode both the old and new numbers in the same value
- Because cell values range from `[0-255]` it means we have a lot of room to work with w.r.t. allocated bits for each integer
- We can calculate the smoothed value, bit shift by 8 bits, then `OR` it with the original number. This means the least sig. 8 bits are the original value, and the most sig. 8 bits are the most sig. value
- To extract the smoothed value, bit shift by 8 bits to get the smoothed operator
- To extract the original value, if we do a bitwise `AND` operator with 255 (which has all 1's for the first 8 bits) it ensures that we only preserve those least sig. 8 bits and set everything else to 0
- This approach can also be done without bit shifting, by storing the original number normally, but multiplying the smoothed number by a factor of 256, and using `% 256` when you want to extract it, this is the same idea as bit shifting, just without the actual bit shifts

#### Edge Cases

#### Solution
```python 
class Solution:
    def imageSmoother(self, img: List[List[int]]) -> List[List[int]]:
        coords = [(-1, -1), (-1, 0), (-1, 1), (0, -1), (0, 0), (0, 1), (1, -1), (1, 0), (1, 1)]
        rows, cols = len(img), len(img[0])
        ans = [[0 for _ in range(cols)] for _ in range(rows)]

        for r in range(rows):
            for c in range(cols):
                total, count = 0, 0
                for dr, dc in coords:
                    newr, newc = r+dr, c+dc
                    if newr >= 0 and newr < rows and newc >= 0 and newc < cols:
                        total += img[newr][newc]
                        count += 1
                ans[r][c] = floor(total/count) if count != 0 else 0
        return ans```

#### Time Complexity
`O(m*n)`

#### Space Complexity
`O(m*n)`

