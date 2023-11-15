---
tags: [permutation, string, set, two_pointer]
---
### 1930. Unique Length-3 Palindromic Subsequences

Link: [here](https://leetcode.com/problems/unique-length-3-palindromic-subsequences)

#### Problem
Given a string `s`, return _the number of **unique palindromes of length three** that are a **subsequence** of_ `s`.

Note that even if there are multiple ways to obtain the same subsequence, it is still only counted **once**.

A **palindrome** is a string that reads the same forwards and backwards.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.
- For example, `"ace"` is a subsequence of `"abcde"`.

#### Main Idea
- 3-length palindromes must have the first and last char match, and the middle char doesn't matter
- We can maximize the potential unique palindrome subsequences by looking for the left-most and right-most instances of characters, and making palindromes with all the chars in-between

#### Approach
- Convert the input `s` to a set
- Iterate through each unique set char value
- Find the left and right-most occurrence of the char
- Find all the unique chars between the left and right pointer for that char
- Keep total for all potential left and right palindrome values
#### Edge Cases
- `len(s) < 3`
- There are no palindromes
- Invalid input

#### Solution
```python 
class Solution:
    def countPalindromicSubsequence(self, s: str) -> int:
        letters = set(s)
        ans = 0
        # For each letter, find the left and right-most for that char
        for letter in letters:
            left, right = s.index(letter), s.rindex(letter)
            between = set()
            # We now get the # of unique chars between left and right
            for mid in range(left+1, right):
                between.add(s[mid])
            # Now update the global total
            ans += len(between)
        return ans
```

#### Time Complexity
`O(n)` since `len(letters)` is technically bounded to 26 as there are 26 letters, thus the outer for-loop technically runs in constant time.
#### Space Complexity
`O(1)` since only using letters means constant space for the generated sets, but if there were `k` unique chars, it would be a `O(k)`.

