---
tags: [math, backtracking, depth_first_search, recursion]
---

### 17. Letter Combinations of a Phone Number

Link: [here](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

#### Problem
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

#### Main Idea
- Use backtracking to navigate all the possibilities

#### Approach
- Make a map to hold all the number and letter matchings
- Handle the edge case of when `digits` is `""`
- If at the end of `digits` add the current word to `ans`
- Otherwise make recursive calls for all the possible values for that digit


#### Solution
```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
		# Edge case
        if len(digits) == 0:
            return []

        numMap = {
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",
            "8": "tuv",
            "9": "wxyz"
        }

        ans = []

        def backtrack(i, curr):
            # If at end, stop and add curr
            if i >= len(digits):
                ans.append(curr)
                return
            # Otherwise, go through options
            for j in range(len(numMap[digits[i]])):
                backtrack(i+1, curr + numMap[digits[i]][j])
        
        backtrack(0, "")
        return ans
```

#### Time Complexity
`O(2^n)` 

#### Space Complexity
`O(n)` for the call stack space, we aren't considering the output as part of space complexity.