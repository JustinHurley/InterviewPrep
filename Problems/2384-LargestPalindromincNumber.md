---
tags:
  - medium
  - string
  - math
  - greedy
---
# 2384. Largest Palindromic Number
Link: [here]()
## Problem
You are given a string `num` consisting of digits only.
Return _the **largest palindromic** integer (in the form of a string) that can be formed using digits taken from_ `num`. It should not contain **leading zeroes**.

**Notes:**
- You do **not** need to use all the digits of `num`, but you must use **at least** one digit.
- The digits can be reordered.
## Main Idea
- We want to maximize the output number, so we should build the palindrome starting with the largest numbers to smallest e.g. `321 > 123`
- We can greedily add to the palindrome
- We should also keep track of the the largest non-pair value, since the palindrome can have a non-paired middle value
- We can use a HashMap to easily determine the frequency of each number
## Approach
- Get the frequency of each char
- Build variables to hold half the palindrome and the middle character
- Move through key in the frequency map for the characters, ensuring that you want to go from largest to smallest `9 -> 0`
	- If you have more than 2 of a digit, you can add to the permutation and then keep moving
	- If you only have one of a digit, you can add to the midpoint and keep going
- Now that we have half the palindrome and the middle element, we should strip the 0s from the left half of the array
- Return `half + mid +half[::-1]` as the answer, or 0 if there were only 0s.
## Edge Cases/Gotchas 
- We can't have numbers with leading zeros, so we need to use `lstrip('0')` to remove all the leading 0s (since we are only looking at half the palindrome this will also remove the trailing zeros)
- If only 0s in the input array, return 0 
## Solution
```python 
class Solution:
    def largestPalindromic(self, num: str) -> str:
        freqs = Counter(num)
        half = ''
        mid = ''
        # Add pairs to half
        for key in sorted(freqs.keys(), reverse=True):
            # Find pairs for palindrome
            if freqs[key] >= 2:
                # Pull out pairs and add to half
                half += key*(freqs[key]//2)
                # Update count
                freqs[key] = freqs[key]%2
            # Find the middle for palindrome 
            if freqs[key] >= 1:
                mid = max(key, mid)
        # Now if odd we need to add mid elt
        half = half.lstrip('0')
        return half + mid + half[::-1] or '0'
```
## Time Complexity
$O(n)$ since we go through each char in `num` once to build `freqs`, we do sort the keys but those can only have values ranging from `[0, 9]` so we can say that the sorting took constant time
## Space Complexity
$O(1)$ for the same reason as above, we hypothetically could use 10 variables to keep track of the counts of each variable and it would be easier to see how this is done in constant space.
