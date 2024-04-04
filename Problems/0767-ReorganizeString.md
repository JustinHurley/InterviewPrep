---
tags:
  - medium
  - heap
  - dictionary
---
# 767. Reorganize String
Link: [here](https://leetcode.com/problems/reorganize-string/description/)
## Problem
Given a string `s`, rearrange the characters of `s` so that any two adjacent characters are not the same.

Return _any possible rearrangement of_ `s` _or return_ `""` _if not possible_.
## Intuition
- We want to deal with the most frequent characters first since they will have the largest constraints for the problem
- We'll need to get the frequency of all characters in the problem
- **Conclusion 1** (Easier to make): We can use a heap to ensure that we always add the character with the highest current frequency
- **Conclusion 2** (Harder to make): We can add the elements from most frequent to least frequent in all the even cells then all the odd cells 
## Approach 1
- Get the frequency of all the chars
- Go through and convert into a heap with `(frequency, char)`
- Pop elements in the heap, adding the most frequent element
- When you add an element to the answer, store it for an iteration before adding back into the heap so you don't add the same char twice 
## Approach 2
- Instead of needing to use a heap to get all the values, we can instead just add the most frequent element starting from 0 in all the even spots until we're done with it, then add other elements in the array in no particular order
- This works since the main issue we need to worry about is the most frequent element needing more than `len(s)//2` space, which would mean it can't be placed not next to itself
- This approach has better time complexity, since we don't need to deal with the `log(n)` heap pop and push operations 
## Edge Cases/Pitfalls
- You need to store the most frequent char for an iteration with the heap approach 
- If the length of the built string is not the same as the input string, it means we couldn't build a valid answer 
- Otherwise keep adding stuff to all the even-number indexed cells, then the odd indexed cells
## Solution
```python 
class Solution:
    def reorganizeString(self, s: str) -> str:
        freqs = Counter(s)
        heap = [(-val, key) for key, val in freqs.items()]
        heapq.heapify(heap)
        ans = []
        last = None
        while heap:
            # Pop curr
            curr = heapq.heappop(heap)
            count, key = -curr[0], curr[1]
            # Add to answer
            ans.append(key)
            # Add last value if one exists
            if last:
                heapq.heappush(heap, last)
            # Replace last if necessary
            last = (-(count-1), key) if count-1 != 0 else None
        if len(ans) != len(s):
            return ''
        return ''.join(ans)
```
## Time Complexity
$O(n*log(k))$ where `k` is the number of distinct chars
## Space Complexity
$O(n)$ for the heap
## Takeaways 
- Heaps are good but see if you can do better 
- Think about what makes a valid solution, and what makes an efficient valid solution
