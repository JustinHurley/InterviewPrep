---
tags:
- array
- string 
- Dynamic_Programming
---
### 5. Longest Palindromic Substring

Link: [here](https://leetcode.com/problems/longest-palindromic-substring/description/)

#### Problem
Given a string `s`, return the longest palindromic substring in `s`.

#### Approach
**Potentially Non-DP Approach**
To generate all the permutations of substrings is a `O(n^2)` operation. If we want to then check if each substring is a palindrome, that's an `O(n)` solution. So we end up with a total time complexity of `O(n^3)` which is no good.
So how can we reduce the work?
We can reduce the work by combining the palindrome finding step and the substring generation step. To do this we start at each char, and move a left and right pointer in each direction to see if we can make a substring that is in-bounds, and the left and right pointers have been equal for every iteration so far. This means on `s[0]` we can only check for substrings of length 1, but the `s[1]` would be able to check for substrings of length 3 (`s[0:2]` inclusive). This allows us to check for the palindrome as we build out the potential substring, and we stop checking substrings around that char once it breaks, which won't change the time complexity but does offer some addition optimization.

**DP Approach**
The DP approach is just optimizing the `O(n^3)` algorithm using a 2D array. What it does is seeds the array with all the palindromes that are just one char. So `"a"` is technically a 1 char long palindrome. So when we check a palindrome, we fill in the DP 2D array, using this line:
```
dp[i][j] = s[i] == s[j] and dp[i+1][j-1]
```
What this is saying, is that if `i` and `j` which represent the start and finish of the palindrome are equal, and the chars between the 2 are also equal, then we must have a valid palindrome. This saves us time of needing to recompute the substring each time, and we can just use the existing known DP values to fill in the table and then generate our answer.

#### Solution
**Non-DP Solution**
```
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)
        best = (0, "")

        
        for i in range(n):
            # Start left and right pointers at curr pos
            l, r = i, i
            # Check odd palis
            while 0 <= l and r < n and s[l] == s[r]:
                if best[0] < r - l + 1:
                    best = (r - l + 1, s[l:r+1])
                l -= 1
                r += 1
            # Check even palis
            l, r = i, i+1
            while 0 <= l and r < n and s[l] == s[r]:
                if best[0] < r - l + 1:
                    best = (r - l + 1, s[l:r+1])
                l -= 1
                r += 1
        
        return best[1]
```

**DP Solution** (from ChatGPT)
```
def longest_palindrome(s):
    n = len(s)
    dp = [[False] * n for _ in range(n)]
    start = 0
    max_len = 1

    # Initialize diagonal elements to True since a string of length 1 is a palindrome
    for i in range(n):
        dp[i][i] = True

    # Check for palindromes of length 2 and greater
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1

            if length == 2:
                dp[i][j] = s[i] == s[j]
            else:
                dp[i][j] = s[i] == s[j] and dp[i+1][j-1]

            if dp[i][j] and length > max_len:
                max_len = length
                start = i

    return s[start:start+max_len]
```

#### Time Complexity
`O(n^2)` for both cases.

#### Space Complexity
`O(n^2)` for DP and `O(1)` for non-DP.

