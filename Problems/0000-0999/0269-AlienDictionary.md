---
tags: [hard, graph, graph_traversal, depth_first_search, string]
---
# 269. Alien Dictionary 
Link: [here](https://leetcode.com/problems/alien-dictionary/)
## Problem
There is a new alien language that uses the English alphabet. However, the order of the letters is unknown to you.
You are given a list of strings `words` from the alien language's dictionary. Now it is claimed that the strings in `words` are **sorted lexicographically** by the rules of this new language.
If this claim is incorrect, and the given arrangement of string in `words` cannot correspond to any order of letters, return `"".`
Otherwise, return _a string of the unique letters in the new alien language sorted in **lexicographically increasing order** by the new language's rules__._ If there are multiple solutions, return _**any of them**_.
## Main Idea
- Express lexicographical ordering as a graph
- DFS on graph
- Use post-order-traversal to process the last node alphabetically first
- Reverse the result
## Approach
- First built a dictionary to use as the graph, each character will have a set that holds neighbor nodes
- Now we need to build the graph, go through and compare all adjacent alphabetical words in the input array
- Make sure to handle invalid cases
- Find the first char that differs between the two words, and that can be used as an edge in the graph
- Once graph is built, you'll need to [[DepthFirstSearch|DFS]] on it
- For each DFS call
	- If the node has already been seen, return True
	- Otherwise, mark the seen node as True and keep going
	- Continue traversing the path (call DFS on all neighbors)
	- If any return True, it means we found a cycle and should return True 
	- Once done processing neighbors, set seen to False as we no longer are on that DFS path
	- Finally, since it's a post order traversal, once we call all child nodes, process the original node (add char to answer list)
- Make the DFS call on each node, since we are doing a post-order-traversal, we can start from anywhere in the graph, but if any DFS calls return true, return `''` as that means we have an invalid lexicographical order
- Finally reverse the array since we did a post-order-traversal so we will have built the answer from Z to A if it was English ordering.
## Edge Cases/Gotchas 
- Need to handle the case where there are cycles (e.g. `a < b < a`) which imply an invalid ordering of characters
- Handle case where a prefix word comes before the word (e.g. `dogs < dog`) should never happen
## Solution
```python 
class Solution:
    def alienOrder(self, words: List[str]) -> str:
        # Set up graph, make a set for each char
        graph = {c: set() for word in words for c in word}
        # Build graph from input
        for i in range(len(words)-1):
            w1, w2 = words[i], words[i+1]
            # Find max len to compare chars at
            minLen = min(len(w1), len(w2))
            # Handle prefix edge case (e.g. dogs < dog)
            if len(w1) > len(w2) and w1[:minLen] == w2[:minLen]:
                return ''
            # Find first diff char
            for j in range(minLen):
                if w1[j] != w2[j]:
                    graph[w1[j]].add(w2[j])
                    break # Since we want to stop after first mismatch

        # Now DFS on all nodes without parents, need to do post-order-traversal
        ans = []
        seen = {} # False = visited, True = on the current path
        def dfs(node):
            if node in seen:
                # If this is true there is a cycle 
                return seen[node]
            # Mark node as True as it's in the path
            seen[node] = True
            for nei in graph[node]:
                # If True a loop was detected 
                if dfs(nei):
                    return True
            # Node is no longer in the path set to False
            seen[node] = False
            # Append at the end for post-order-traversal
            ans.append(node)
        # Do DFS can start anywhere
        for c in graph:
            # If True a loop was detected
            if dfs(c):
                return ''
        # Did post-order-traversal so reverse answer
        ans.reverse()
        return ''.join(ans)
```
## Time Complexity
$O(C)$ where `C` is the number of characters, we scan each one at the beginning to build the graph, the actual traversal would visit the same or fewer characters than that
## Space Complexity
$O(n+k)$ where `n` is number of words and `k` is length of words since we store potentially every char in the graph
