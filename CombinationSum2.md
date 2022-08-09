### 40. Combination Sum II

Link: [here](https://leetcode.com/problems/combination-sum-ii/)

#### Topics
- Backtracking
- Depth-first-search
- Recursion
- Permutation

#### Problem
Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target.

Each number in candidates may only be used once in the combination.

Note: The solution set must not contain duplicate combinations.

#### Approach
So the general approach to this problem is to solve it like a DFS problem. We can think of each permutation as a path on a decision tree, and we are trying to visit all the potential nodes. 
We need a recursive function, that checks if you are at the target sum and if so, makes a *deep copy* of the current built solution set. If it's over the target then you can stop, and if it's under you need to recursively call the method with all the other options.
For this problem, preventing duplicates is important. This is why we sort the candidate array at the beginning and also check for duplicates while iterating. We check for duplicates by ensuring that we aren't about to add the same number we are currently looking at into the solution set. We also look at the previous element to make sure it's not the same. We also start each sequence from the current index we are on, so that we "move right" with each recursive call, preventing duplicates. If we didn't do that, we could get `[candidates[i],candidates[j]]` and `[candidates[j],candidates[i]]`.

#### Solution
```
class Solution:
    def combinationSum2(self, candidates, target):
        candidates.sort()
        ans = []
        
        def dfs(remainder, idx, path):
            # If we overshot
            if remainder < 0:
                return  # Backtrack (Not a valid path)
            # If we hit target
            if remainder == 0:
                ans.append(path)
                return
            # If we can still add values
            for i in range(idx, len(candidates)):
                c = candidates[i]
                # If we're not on the same index and the currVal isn't a dupe
                if i != idx and c == candidates[i-1]: continue  # Eliminates duplicates
                dfs(remainder-c, i+1, path+[c])

        dfs(target, 0 , [])
        return ans
```
