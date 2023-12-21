---
tags: [easy, array, "#greedy"]
---
### #. Can Place Flowers

Link: [here](https://leetcode.com/problems/can-place-flowers/description/)

#### Problem
You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in **adjacent** plots.

Given an integer array `flowerbed` containing `0`'s and `1`'s, where `0` means empty and `1` means not empty, and an integer `n`, return `true` _if_ `n` _new flowers can be planted in the_ `flowerbed` _without violating the no-adjacent-flowers rule and_ `false` _otherwise_.

#### Main Idea
- Determine if the left and right sides are valid, if they are, then place a 1

#### Approach
- See above

#### Edge Cases
- Checking the leftmost and rightmost indicies 

#### Solution
```python 
class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        count = 0
        for i in range(len(flowerbed)):
            if flowerbed[i] == 0:
                left = (i == 0) or (flowerbed[i-1] == 0)
                right = (i == len(flowerbed)-1) or (flowerbed[i+1] == 0)
                if left and right:
                    flowerbed[i] = 1
                    count += 1
        return count >= n
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(1)`
