---
tags: [medium, two_pointer, sorting, greedy]
---
# 948. Bag of Tokens
Link: [here](https://leetcode.com/problems/bag-of-tokens/description/)
## Problem
You start with an initial **power** of `power`, an initial **score** of `0`, and a bag of tokens given as an integer array `tokens`, where each `tokens[i]` donates the value of token_i_.

Your goal is to **maximize** the total **score** by strategically playing these tokens. In one move, you can play an **unplayed** token in one of the two ways (but not both for the same token):
- **Face-up**: If your current power is **at least** `tokens[i]`, you may play token_i_, losing `tokens[i]` power and gaining `1` score.
- **Face-down**: If your current score is **at least** `1`, you may play token_i_, gaining `tokens[i]` power and losing `1` score.
Return _the **maximum** possible score you can achieve after playing **any** number of tokens_.
## Main Idea
- You can greedily always try and use the smallest token and if not enough power, then get the largest power 
- Since we get one point for each token, we don't care about saving tokens with more power
## Approach
- Make two pointers for the left and right edges of the array
- While the left != the right pointer we will move through the sorted input array
- If the smallest current token is less than the power, we will leave it heads up
- If we don't have enough power we'll get as much power as possible 
## Edge Cases/Gotchas 
- Make sure to handle when there is only one item in the adjusted array (pointer distance is 1)
## Solution
```python 
class Solution:
    def bagOfTokensScore(self, tokens: List[int], power: int) -> int:
        tokens.sort()
        score = 0
        left, right = 0, len(tokens)
        while left < right:
            # If we can use token, use smallest
            if tokens[left] <= power:
                score += 1
                power -= tokens[left]
                left += 1 
            # If we can't and not on last token grab largest
            elif score >= 1 and right-left > 1:
                score -= 1
                power += tokens[right-1]
                right -= 1
            # If we can't do either then we should stop
            else:
                return score
        return score 
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
