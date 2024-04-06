---
tags:
  - backtracking
  - recursion
  - string
  - medium
---
### 131. Palindrome Partitioning

Link: [here](https://leetcode.com/problems/palindrome-partitioning/description/)

#### Problem
Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return _all possible palindrome partitioning of_ `s`.

#### Main Idea
- We will need to permute through each potential option
- Use backtracking to iterate over all subset permutations

#### Approach
We know this is a backtracking problem because we are asked to evaluate all permutations of subset combinations where the items are palindromes. This means we will have to navigate the decision tree of possible answers to determine if we have the right one.
With that in mind, we can create a helper-DFS-like method to better navigate the string in a recursive manner. The method is broken down into a few parts:
1. Base Case: if `i >= len(s)` then we made it through the word with a valid answer, and can update the return list.
2. We aren't at the end of the list, so want to check and see if any substrings starting from the current index `i` to the end of the list are actually palindromes
We also need to make a helper palindrome verification method so that we don't waste time evaluating scenarios where already added subsets are invalid i.e. don't waste time building a bad solution, fail as soon as you know it won't work for maximum efficiency. 

#### Edge Cases
- `s` is not a string or `None`

#### Solution
```python 
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        ans = []
        curr = []

        def backtrack(i):
            # If OOB stop add to ans and stop
            if i >= len(s):
                ans.append(curr[:])
                return
            # If the curr str, is a pal, we should add to curr and keep moving
            for j in range(i, len(s)):
                if self.isPalindrome(s[i:j+1]):
                    curr.append(s[i:j+1])
                    backtrack(j+1)
                    curr.pop()

        backtrack(0)
        return ans
```

#### Time Complexity
`O(2^n)` since we could technically permute every possible subset, however it is of note that we will not traverse any parts of the decision tree where we already have an invalid substring. 

#### Space Complexity
`O(n)` since we keep track of the existing substring. 

