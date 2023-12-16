---
tags:
  - combinations
  - backtracking
  - depth_first_search
  - string
  - medium
---

### 22. Generate Parentheses

Link: [here](https://leetcode.com/problems/generate-parentheses/)

#### Problem
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

#### Approach
**First Solve**
This problem can be solved with [[DynamicProgramming|dynamic programming]], but I elected to do the [[Categories/Backtracking|backtracking]] approach. Basically the way it works is you use a dictionary to keep track of how many parentheses are left. 
While we could permute every combination of parentheses, many of them would be invalid and we would waste time checking to see if every solution is valid. An ideal solution would be one that never makes an invalid solution in the first place. This can be done by ensuring that there are always an equal or greater number of left parentheses than right ones. We want to keep this in mind when we go into our recursive method.
Recursive method logic:
- If there are no more parentheses, stop and add to answer set.
- If there are more right parentheses than left parentheses, we must be at the end of the list because otherwise this would be impossible (will make more sense based on the other cases).
- If there are an equal number of left and right parentheses, we need to add a left parenthesis. This is because when there are an equal number of left and right parentheses left, we can assume that the current built solution is valid, and adding a right parenthesis would make it invalid.
  - Example: `() -> ())`, there is no way to fix this by adding more parentheses. 
- If there are more left than right, then we should recursively call on both, as both can lead to valid answers.
  
**Second Solve**
The beginning to the approach for this problem is to break down the problem to determine when you can validly add open or closed parentheses. We can keep track of the counts of each type of paren, and determine validity based on that. We know that for an input of `n` we would want `n` open parens and `n` closed. 
We can always put an open paren as long as we haven't hit `n` open parens. This is because you can't make an invalid answer by adding open parens (when it's n or less), and can only make an invalid paren by adding a closed one. For example `(((((` could be fixed by adding 5 closed parens, but `)))))` can never be fixed by adding more open parens. 
So now that we have determined that, when can we add closed parens? We can only do so when open > closed. Consider the example `()` where `open == closed`. Adding a closed paren here would make an invalid case. What about `closed > open`? Well that clearly won't work, consider `)`. So we can only add a closed paren when `open > closed` as we need an open paren to pair up a closed paren with. 
This gives us our cases. 
1. If `open == closed == n` then we have a finished paren answer and can add it to the answer list.
2. If `open < n`, then we can add an open paren and know it will be valid because of the next case.
3. If `closed < open` then we can add a closed paren.
We know that these rules will keep things valid because we don't add more than `n` parens, we never add a closed paren unless `closed < open` i.e. we have an open paren to pair the closed one with, and we don't add an open paren if we have already added `n` parens.

Now the next part of the problem comes up. How do we actually generate all of these permutations?
The answer in this case is to use [[Categories/Backtracking|backtracking]] (or DP). We basically use a method that takes in the current count of the parens in the answer we are generating, and it makes the next case(s) based on the current case.
We make 1 recursive call if `open < n` and `closed == open` and we make 2 calls if `open < n` and `closed < open` since adding either a closed or open paren will generate a valid arrangement. 
Now this is where the [[Categories/Backtracking|backtracking]] comes into play. The basic way [[Categories/Backtracking|backtracking]] works is like this:
```
do work on ans being built
recursive call
reverse work on ans being built
```
so in this case it would look like:
```
currAns.append('(') 
backtrack(open+1, closed)
currAns.pop()
```
and then we do the same thing for the closed paren if valid. This allows us to build permutations of solutions without mutating the partially-built answer in a way that prevents multiple answer approaches.

Note: In python, you can convert a list of strings or chars to a string by doing `"".join(array)`.

#### Solution
**First Solve**
```python
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
**Second Solve**
```python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        ans = []
        stack = []

        def backtrack(o, c):
            # If all equal, we have a valid soln and can append to ans
            if o == n == c:
                ans.append("".join(stack))
            # If o < n, then we can add item we're building
            if o < n:
                stack.append('(')
                backtrack(o+1,c)
                stack.pop()
            # If c < o, we can add a closed paren
            if c < o:
                stack.append(')')
                backtrack(o,c+1)
                stack.pop()
        # Build the soln starting from nothing but could start with open paren
        backtrack(0,0)
        return ans
```

#### Time Complexity 
Technically the time complexity is `O((4^n)/sqrt(n))` which is a simplification of the nth Catalan number, which is asymptotically bounded by the above. It is unexpected for you to know this.
