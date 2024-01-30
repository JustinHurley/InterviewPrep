# Pitfalls
This file includes some common logical traps/pitfalls I would fall into when solving LeetCode problems that would waste time and lead to suboptimal solutions. 
### Graphs
- If you are checking whether the children nodes exist in a DFS problem, you are approaching it wrong.
- Don't box yourself into a solution by refusing to make a helper method for extra parameters to use during DFS.
- Trying to work with a weird graph representation when you could just build a `Node` class yourself 