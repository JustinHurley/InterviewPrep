---
tags:
- dynamic_programming
aliases:
- 'dynamic programming'
- 'DP'
---

### When to Use
Dynamic programming should be used for problems that have optimal substructure, meaning the solution can be made from the solutions to subproblems, and overlapping subproblems, meaning we build the next state with information from the current state. For example in [[House Robber]], we determine our best sum at the current house from the optimal sums of the previous sums, depending on if we chose to skip the house or not.
Typically, if you can use an alternate approach instead of DP it's preferred, since DP should really only be used when you need to consider a permutation of outcomes. 

### Problems
```query
tag:#dynamic_programming
```
