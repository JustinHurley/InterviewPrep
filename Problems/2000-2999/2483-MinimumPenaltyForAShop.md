---
tags:
  - medium
  - prefix_sum
---
# 2483. Minimum Penalty for a Shop
Link: [here](https://leetcode.com/problems/minimum-penalty-for-a-shop/description/)
## Problem
You are given the customer visit log of a shop represented by a **0-indexed** string `customers` consisting only of characters `'N'` and `'Y'`:
- if the `ith` character is `'Y'`, it means that customers come at the `ith` hour
- whereas `'N'` indicates that no customers come at the `ith` hour.

If the shop closes at the `jth` hour (`0 <= j <= n`), the **penalty** is calculated as follows:
- For every hour when the shop is open and no customers come, the penalty increases by `1`.
- For every hour when the shop is closed and customers come, the penalty increases by `1`.

Return _the **earliest** hour at which the shop must be closed to incur a **minimum** penalty._
**Note** that if a shop closes at the `jth` hour, it means the shop is closed at the hour `j`.
## Main Idea
- Start with the count of `Y`, which will tell us the penalty for a fully closed shop
- Move through the array, calculating the penalty if the shop closes at time `i`, this will compute the penalty at any given opening and closing time as you iterate
## Approach
- See above
## Edge Cases/Gotchas 
- Since range is `[1, n]`, we need to `-1` going from the number to the index value
## Solution
```python 
class Solution:
    def bestClosingTime(self, customers: str) -> int:
        # Assume shop closes on first iteration
        curr = mins = customers.count('Y')
        ans = 0

        # Move through the array
        for i, c in enumerate(customers):
            # If we see Y, update count as if we were open
            if c == 'Y':
                curr -= 1
            else:
                curr += 1
            # If we have a new min penalty, update 
            if curr < mins:
                ans = i+1
                mins = curr
        return ans
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
