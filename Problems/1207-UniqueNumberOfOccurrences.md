---
tags: [easy, set, python]
---
### 1207. Unique Number of Occurrences

Link: [here](https://leetcode.com/problems/unique-number-of-occurrences/description/)

#### Problem
Given an array of integers `arr`, return `true` _if the number of occurrences of each value in the array is **unique** or_ `false` _otherwise_.

#### Main Idea
- Get the list of frequencies for each number and check to see if it's the same size as if when it's a set

#### Approach
- Use `Counter`

#### Edge Cases
- Invalid input

#### Solution
```python 
class Solution:
    def uniqueOccurrences(self, arr: List[int]) -> bool:
        return len(set(Counter(arr).values())) == len(Counter(arr).values())
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)`

