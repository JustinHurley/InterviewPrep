### 17. Letter Combinations of a Phone Number

Link: [here](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

#### Topics
- Combinations
- Backtracking
- DFS
- Recursion
- String building

#### Problem
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

#### Approach
This is a pretty straightforward combination problem, there is no room for memoization as each combination will be unique. 
All you do is make a recursive method that tracks the current index and recursively accumulates an answer set.
For each digit, we call a recursive method for each letter. For example, if we saw the digit `"2"`, then we recursively run on `"a"`, `"b"`, and `"c"`.

#### Solution
```
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        ans = []
        n = len(digits)
        # Mapping of letters to phone button
        letters = {
            "2": "abc", 
            "3": "def", 
            "4": "ghi", 
            "5": "jkl", 
            "6": "mno", 
            "7": "pqrs", 
            "8": "tuv", 
            "9": "wxyz"
        }
        # Need to handle edge case when no digits are given
        if digits == "":
            return []
        
        # Helper function
        def dfs(currWord: str, currIndex, fullWord):
            # First check to see if at end of string
            if currIndex == n:
                ans.append(currWord)
                return
            # If not at the end, want to iterate on the current numbers children 
            currDigit = fullWord[currIndex]
            # Iterate through set of letters for number
            for c in letters[currDigit]:
                dfs(currWord+c, currIndex+1, fullWord)
        
        dfs("", 0, digits)
        
        return ans
```

