---
tags: [medium, dynamic_programming, math, greedy]
---
# 134. Gas Station
Link: [here](https://leetcode.com/problems/gas-station/description/)
## Problem
There are `n` gas stations along a circular route, where the amount of gas at the `ith` station is `gas[i]`.
You have a car with an unlimited gas tank and it costs `cost[i]` of gas to travel from the `ith` station to its next `(i + 1)th` station. You begin the journey with an empty tank at one of the gas stations.
Given two integer arrays `gas` and `cost`, return _the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return_ `-1`. If there exists a solution, it is **guaranteed** to be **unique**
## Main Idea
- Start at a point and try to visit all other nodes
- If the gas total is negative, we should start over from the next node
- Since we checked at the beginning, and since the problem only has one valid solution, any time we can make it from the current node to the end of the array, we have found the target node
- The main idea revolves around understanding that we can narrow down the problem space to only valid solutions by checking the `gas` and `cost` arrays, and then we don't need to find the brute force solution
- We know that a solution must exist, so if it's not in the first `i` points, then the next logical cell to check is the `i+1`th one.
## Approach
- Check if `sum(gas) < sum(cost)` which would mean no route is possible 
- Set the start and total variables
- Now iterate through each cell of the graph
	- Update the total by adding the gas at the current stop, and subtracting the cost it takes to make it to the next town
	- If the gas total ever goes negative, we know it's impossible to make it from that stop, so we should start over and try at the next cell, since we know there has to be a solution and it's not at any of the already visited nodes
- Return the index value that was stored, it will reflect the valid starting node
## Edge Cases/Gotchas 
- Handling the case when the overall cost is larger than the amount of gas you can get makes the problem easier, since then you just have to find the one solution
## Solution
```python 
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        # Check if no solution possible
        if sum(gas) < sum(cost):
            return -1
        n = len(gas)
        start = 0
        total = 0
        # For each starting point
        for i in range(n):
            # Fill up and take cost of curr
            total += gas[i] - cost[i]
            # If total < 0, then no gas
            if total < 0:
                total = 0
                start = i+1
        return start
```
## Time Complexity
$O(n)$
## Space Complexity
$O(1)$
