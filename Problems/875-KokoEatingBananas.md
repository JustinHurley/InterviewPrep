---
tags:
- binary_search
- array
---

### 875. Koko Eating Bananas

Link: [here](https://leetcode.com/problems/koko-eating-bananas/description/)

#### Problem
Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in h hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

#### Approach
The goal is to determine the minimum number where we can still finish all of the piles under `h` hours. To approach this problem we can first start with finding the range of values possible. In this case, the largest or worst-case value would be `max(piles)`. This is because this would allow us to finish each pile in 1 hour, but there may potentially be ways we can do it without needing to eat 1 pile of bananas per hour. The best case scenario in this problem would be `k = 1`, where we only need to eat one banana per hour. Now that we have defined our range of `k`, `[1, max(piles)]`, we now need to search this range to find the minimum `k` value. To do this, we can use binary search, choosing a `k`, seeing if it produces a valid answer (takes `h` or less hours to eat all the bananas), and then seeing if we can do better. Because we are working with a range, which is automatically sorted, we just do binary search, comparing valid answers as we go along to some best answer that we are tracking until the while loop terminates and we return the best answer.

#### Solution
```python 
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        l,r = 1, max(piles)
        ans = r
        
        while l <= r:
            k = (l+r)//2
            count = 0
            for pile in piles:
                count += math.ceil(pile/k)
            if count <= h:
                ans = min(ans, k)
                r = k-1
            else: 
                l = k+1
        return ans
```

#### Time Complexity
`O(n*max(piles))` since we iterate over all piles each iteration, and we do binary search over `max(piles)` elements.