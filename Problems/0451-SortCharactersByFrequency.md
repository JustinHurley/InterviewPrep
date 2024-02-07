---
tags: [medium, sorting, dictionary]
---
# 451. Sort Characters by Frequency
Link: [here](https://leetcode.com/problems/sort-characters-by-frequency/description/)
## Problem
Given a string `s`, sort it in **decreasing order** based on the **frequency** of the characters. The **frequency** of a character is the number of times it appears in the string.

Return _the sorted string_. If there are multiple answers, return _any of them_.

## Main Idea
- Use a dictionary to get the frequency of all the letters
- Sort by frequency and then build the string answer
- You can also use [[Algorithms/BucketSort|bucket sort]] to solve the problem
## Approach
- Use a `Counter` to make a frequency map of characters
- `zip` the dictionary keys and items together, sorting in reverse so that the most frequent elements go first
- Build the answer and return
## Edge Cases/Gotchas 
- N/A there could be a follow up question asking how to handle if the answer did need to be in some sorted order alphabetically for duplicates, but that is not the case in this question
## Solution
```python 
class Solution:
    def frequencySort(self, s: str) -> str:
        # Get freqs
        freqs = Counter(s)
        # Convert to better format and sort
        sorted_freqs = sorted(list(zip(freqs.values(), freqs.keys())), reverse=True)
        # Make seed for ans
        ans = []
        # Go through each freq
        for count, val in sorted_freqs:
            # Add char to ans count times
            ans.append(val * count)
        return ''.join(ans)```
## Time Complexity
`O(nlong(n))` since we sort 
## Space Complexity
`O(n)`
