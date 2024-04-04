---
tags:
  - medium
---
# 846. Hand of Straights
Link: [here](https://leetcode.com/problems/hand-of-straights/description/)
## Problem
Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size `groupSize`, and consists of `groupSize` consecutive cards.
Given an integer array `hand` where `hand[i]` is the value written on the `ith` card and an integer `groupSize`, return `true` if she can rearrange the cards, or `false` otherwise.
## Main Idea
- We can greedily try to build straights starting from the smallest number ranging to the smallest + `groupSize` . 
- If we make a straight where the smallest value is `k` and the group size is `g`, we must have all values from `k` to `k+g` to be a valid answer
## Approach
- First check to see if we can evenly split the input data into groups of size `groupSize`.
- Get the frequency of each number from the input
- Sort the keys from smallest to largest
- Using the sorted keys array, iterate through those elements and for each element:
	- Check to see if that number still needs to be added to a group
	- If so find how many groups need to be made starting at that number
	- Iterate through the range of the number to the number + `groupSize`, where we check to see if there are still numbers to use in a group, and then we remove the required amount that number is needed for, and if we need a number more times than we can give it, then we return false as it will be impossible to make all the straights starting at the given number
- If we make it through each node and don't end up with any invalid groups, we can return True
## Edge Cases/Gotchas 
- Handle cases where the input cannot be evenly split up into groups of `groupSize`
- You may need to handle cases where you are looking for a number that doesn't exist in the input array, so be wary of `KeyErrors`
## Solution
```python 
class Solution:
    def isNStraightHand(self, hand: List[int], groupSize: int) -> bool:
        # Edge case where hand can't be split up to groupSize 
        if len(hand)%groupSize != 0:
            return False
        # Get freq
        counts = Counter(hand)
        # Get list of sorted keys to iterate through
        keys = sorted(counts.keys())
        # Now go through all vals
        for key in keys:
            # Check to see if we should be making groups
            if counts[key] > 0:
                # Make a group from val -> val+groupSize
                target = counts[key]
                for i in range(key, key+groupSize):
                    # If i doesn't exist, then we stop
                    if i not in counts:
                        return False
                    # Otherwise incr counter 
                    counts[i] -= target
                    # Make sure we didn't over remove
                    if counts[i] < 0:
                        return False
        return True
```
## Time Complexity
$O(n*log(n))$ because of the sorting of the keys
## Space Complexity
$O(n)$ 
