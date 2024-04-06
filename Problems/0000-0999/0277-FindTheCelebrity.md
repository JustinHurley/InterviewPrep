---
tags:
  - medium
  - graph
---
# 277. Find the Celebrity
Link: [here]()
## Problem
Suppose you are at a party with `n` people labeled from `0` to `n - 1` and among them, there may exist one celebrity. The definition of a celebrity is that all the other `n - 1` people know the celebrity, but the celebrity does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. You are only allowed to ask questions like: "Hi, A. Do you know B?" to get information about whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function `bool knows(a, b)` that tells you whether `a` knows `b`. Implement a function `int findCelebrity(n)`. There will be exactly one celebrity if they are at the party.

Return _the celebrity's label if there is a celebrity at the party_. If there is no celebrity, return `-1`.
## Main Idea
- Want to minimize the number of `knows()` calls we have to make
- With each `knows()` call we can determine if a given node is a celebrity or not:
	- If `knows(a,b) == True` we know `a` cannot be the celebrity since the celebrity can't know anybody
	- If `knows(a,b) == False` we know `b` cannot be the celebrity since everyone must know the celebrity 
## Approach
- Set the current candidate node to 0
- Iterate through all other nodes `[1, n)`
	- If the current candidate knows the other node, switch the other node to the given candidate, as it is known by a node, and we know the current node isn't a celebrity
	- If the current candidate does not, we know the other candidate is not a celebrity so we keep checking our current candidate and don't revisit the node
- Once we have our final candidate, we should ensure it actually is the celebrity by making sure they know nobody and are known by everybody, otherwise return -1
## Edge Cases/Gotchas 
- We need to offset iterating through candidate nodes by 1, since we start with 0 and thus should not check `knows(0, 0)`
## Solution
```python 
class Solution:
    def findCelebrity(self, n: int) -> int:
        cand = 0
        for i in range(1,n):
            # If cand knows i, no longer cand
            if knows(cand, i):
                cand = i
        # Check cand
        for i in range(n):
            # Skip self comparison
            if i == cand:
                continue
            if knows(cand, i) or not knows(i, cand):
                return -1
        return cand
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
