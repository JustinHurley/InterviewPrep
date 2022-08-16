### 22. Generate Parentheses

Link: [here](https://leetcode.com/problems/generate-parentheses/)

#### Topics
- Combinations
- Backtracking
- DFS
- Strings

#### Problem
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

#### Approach
This problem can be solved with dynamic programming, but I elected to do the backtracking approach. Basically the way it works is you use a dictionary to keep track of how many parentheses are left. 
While we could permute every combination of parentheses, many of them would be invalid and we would waste time checking to see if every solution is valid. An ideal solution would be one that never makes an invalid solution in the first place. This can be done by ensuring that there are always an equal or greater number of left parentheses than right ones. We want to keep this in mind when we go into our recursive method.
Recursive method logic:
- If there are no more parentheses, stop and add to answer set.
- If there are more right parentheses than left parentheses, we must be at the end of the list because otherwise this would be impossible (will make more sense based on the other cases).
- If there are an equal number of left and right parentheses, we need to add a left parenthesis. This is because when there are an equal number of left and right parentheses left, we can assume that the current built solution is valid, and adding a right parenthesis would make it invalid.
  - Example: `() -> ())`, there is no way to fix this by adding more parentheses. 
- If there are more left than right, then we should recursively call on both, as both can lead to valid answers.

#### Solution
```
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        # We have n pairs so add each paren n times
        parens = {'(' : n, ')' : n}
        ans = []    
        # Want to do a dfs-like operation to make each set
        def dfs(currParen, parenMap):
            # If the map is empty, we have placed all parens
            if sum(parenMap.values()) == 0:
                ans.append(currParen)
                return
            # If the map is not empty, we need to keep adding parens
            else:
                lParen = parenMap['(']
                rParen = parenMap[')']
                
                # If there are more rights than lefts, we're on the last one
                if rParen < lParen:
                    parenMap[')'] -= 1
                    dfs(currParen+')', parenMap)
                    parenMap[')'] += 1
                # If there are equal left and right, place left
                elif rParen == lParen:
                    parenMap['('] -= 1
                    dfs(currParen+'(', parenMap)
                    parenMap['('] += 1
                # Otherwise you can place either
                else:
                    if lParen != 0:
                        parenMap['('] -= 1
                        dfs(currParen+'(', parenMap)
                        parenMap['('] += 1
                    if rParen != 0:
                        parenMap[')'] -= 1
                        dfs(currParen+')', parenMap)
                        parenMap[')'] += 1
        dfs('', parens)
        return ans
```
