---
tags: [sliding_window, dynamic_programming, array]
---

### 121. Best Time to Buy and Sell Stock

Link: [here](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

#### Problem
You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return `0`.

#### Approach
**Dynamic Programming**
This first approach is based on [[DynamicProgramming|dynamic programming]] and memoization to solve the problem. We basically want to figure out what the best future prices is relative to the current `prices[i]` that we're looking at. 
One way to go about doing this is to use an array where we mark at each index `i` what the highest future price will be for that given index. To do this, we can simply iterate through the array backwards, and keep track of what the highest value seen so far is. We also need to offset by 1, since the highest value at a given location should not be itself, it should be only considering the values after it.
We then can do a forward pass through `prices[i]` and at each index, calculate the difference between `prices[i]` and the future max sell price for that given position, then just return the max difference between buy and sell found.

**Sliding Window**
A different approach is to iterate through the array using a [[Categories/SlidingWindow|sliding window]]. Basically we have a left and right pointer, set them to the 1st and 2nd index positions, `0` and `1` respectively, and then start moving the window. 
The window follows a greedy algorithm as such:
1. If the difference is negative, start over from the next index.
2. If the difference is positive, keep moving the right pointer to see if a better sell price can be found.
3. If the right pointer reaches the end of the list, stop.
The reason why we follow the 1st point is because any negative difference means we should start after that. For example, say we are buying at 5 and selling at 3, and looking at future dates to see if we should sell later. We would just be better off buying at 3 no matter what since we will always end up making $2 more by starting at 3 instead of 5, even if we find a better sell price in the future.
The reason why we follow the second rule is because we want to keep looking relative to the buy date. If we happen to find a cheaper buy date in the future, we would end up with a negative profit and rule 1 would kick in, thus starting the cycle over again.

#### Solution
This is the [[DynamicProgramming|dynamic programming]] solution:
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        length = len(prices)
        futureMax = [0]*length

        # Calculates the max selling point after a given index
        currMax = prices[length-1]
        for i in range(length-2,-1,-1):
            futureMax[i] = currMax
            currMax = max(currMax,prices[i])
        
        # Now we just need 1 pass to get the best difference
        best = 0
        for i in range(length):
            best = max(best, futureMax[i] - prices[i])
        
        return best
```
