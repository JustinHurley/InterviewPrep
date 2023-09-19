---
tags:
- sliding_window
- string 
- dictionary
---

### 424. Longest Repeating Character Replacement

Link: [here](https://leetcode.com/problems/longest-repeating-character-replacement/description/)

#### Problem
You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

#### Approach
The general approach is to apply a sliding algorithm to the problem, we don't actually need to do any replacement since the question only asks for the max length, we just need to return that value. 
To keep track of the number of times a character shows up, you can use a dict. Technically since we are working with a string, the max size of the dict is worst case 26, so we don't need to worry about dict scans from a time complexity standpoint. 
Like all [[Categories/SlidingWindow|sliding window]] problems, we get our left and right pointers, and move right until we violate some principle, then move the left pointer up until the principle is no longer violated. In this case, the principle is when the number of replaced characters exceeds `k`. Another way to think of this is using the equation:
```
length of window - count of most frequent character in window <= k
(r - l + 1) - max(dict.values()) <= k
```
We don't want to replace the most frequent character, since that wouldn't give us an optimal solution. 
It's important in this problem to keep track of when we are adding items to the dict, and when we are evaluating the validity of the dict. If we check then add a char, we run the risk of validating, adding a char that makes it invalid, then hitting the last char in the string and returning, thus giving an incorrect solution. While the code solution given takes this into account, there is an easier way to do this without the need for an if statement. 

#### Solution
```python 
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        length = len(s)

        l, r = 0, 0
        seen = {}
        best = 0

        while r < length:
            # First we add the curr elt
            seen[s[r]] = seen.get(s[r],0) + 1
            # If everything is valid, add curr item to dict and keep going
            if (r - l + 1) - max(seen.values()) <= k:
                best = max(best, r - l + 1)
                r += 1
            # If not, move up the left pointer until it is
            else:
                while not (r - l + 1) - max(seen.values()) <= k:
                    seen[s[l]] = seen.get(s[l]) - 1
                    l += 1
                # Increment r so we don't double count where r left off
                r += 1
        return best
```

