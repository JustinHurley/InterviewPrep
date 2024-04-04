---
tags: [medium, math, dynamic_programming, breadth_first_search]
---
# 279. Perfect Squares
Link: [here](https://leetcode.com/problems/perfect-squares/description)
## Problem
Given an integer `n`, return _the least number of perfect square numbers that sum to_ `n`.

A **perfect square** is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, `1`, `4`, `9`, and `16` are perfect squares while `3` and `11` are not.
## Main Idea
- We cannot be greedy (`12 = 4 + 4 + 4` not `9 + 1 + 1 + 1`)
- We should use [[DynamicProgramming|DP]] to find the best combination of coins
- Using DFS would be inefficient
- If we use BFS it ensures we explore all operations of 1 square, 2 squares, 3 squares, etc.
- Processing the decision tree layer by layer ensures we aren't checking 5 square combos before 2 square combos
## Approach
- We first build a list of all squares that are less than `n`
- We establish our queue, which only has `n` in it, and use a counter to track how many squares have been added so far
- For each element in the queue
	- Compare it to all square values, and if they are equal, we can stop as we have completed the problem
	- Otherwise we should see if we've overshot with the current square, or if it's a valid remainder to keep looking for
- Finally we swap out the old queue with the new queue we were building 
## Edge Cases/Gotchas 
- Understanding that the problem can't just be solved greedily is important
- Using BFS instead of DFS to navigate the decision tree is also important
- Using a set instead of a list to represent the queue saves some time with duplicate results being processed 
## Solution
```python 
class Solution:
    def numSquares(self, n: int) -> int:
        # Build all possible sqare values less than n
        squares = [i**2 for i in range(1, int(sqrt(n)) + 1)]
        # Set level for decision tree
        # Each new level == one more square removed
        level = 0
        # Use BFS for a more efficient search
        Q = {n}
        # BFS
        while Q:
            level += 1
            next_Q = set()
            # For each element in the Q
            for rem in Q:
                # Compare the remainder to existing squares
                for square in squares:
                    # Best case, we can stop here
                    if rem == square:
                        return level
                    # Otherwise we need to keep removing values from the Q
                    else:
                        if rem - square >= 0:
                            next_Q.add(rem - square)
            # Update the Q to the next set
            Q = next_Q
        # Return the level as level == num sqares used 
        return level 
```
## Time Complexity
`O(n)`
## Space Complexity
`O(n)`

