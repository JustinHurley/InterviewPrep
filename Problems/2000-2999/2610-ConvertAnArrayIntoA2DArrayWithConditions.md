---
tags: [medium, matrix, array]
---
### 2610. Convert an Array Into a 2D Array with Conditions

Link: [here](https://leetcode.com/problems/convert-an-array-into-a-2d-array-with-conditions/description/)

#### Problem
You are given an integer array `nums`. You need to create a 2D array from `nums` satisfying the following conditions:

- The 2D array should contain **only** the elements of the array `nums`.
- Each row in the 2D array contains **distinct** integers.
- The number of rows in the 2D array should be **minimal**.

Return _the resulting array_. If there are multiple answers, return any of them.

**Note** that the 2D array can have a different number of elements on each row.

#### Main Idea
- The max number of rows will be equal to the maximum frequency of a number
- We can use this to know how many rows will have a given number

#### Approach
- Count all elements in the array
- Use list comprehension to build the answer matrix
- For each element, add it to the first `k` rows of the answer matrix where `k` is the frequency of a given element
- Starting with the first row ensures we minimize the number of rows when possible

#### Edge Cases
- Invalid elements
- Empty input data

#### Solution
```python 
class Solution:
    def findMatrix(self, nums: List[int]) -> List[List[int]]:
        # Get counts of all elts
        counts = Counter(nums)
        rows = max(counts.values())
        ans = [[] for _ in range(rows)]

        for num, count in counts.items():
            for i in range(count):
                ans[i].append(num)
        
        return ans
```

#### Time Complexity
`O(n)`

#### Space Complexity
`O(n)`

