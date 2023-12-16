---
tags:
  - backtracking
  - depth_first_search
  - recursion
  - medium
---
### 40. Combination Sum II

Link: [here](https://leetcode.com/problems/combination-sum-ii/description/)

#### Problem
Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:** The solution set must not contain duplicate combinations.

#### Main Idea
- Iterate through each combination with a recursive backtracking method
- Avoid duplicates by only adding numbers when the previous number doesn't match

#### Approach
- First we need to sort the array, which will make dealing with duplicates easier
- We then make the backtracking recursive method
- If the difference between the target and the current sum is less than 0, we overshot and should stop
- If it is 0, we reached the target and should add `curr` to the answer array via a shallow copy
- Otherwise, we want to look at all the combinations of the current value and the other elements in the array
- To avoid duplicates, it's important that we don't do the same logic twice and end up with duplicate answers. To get around this, we can keep track of a `prev` value that ensures that we don't make 2 recursive calls with the same element value added, as it will just lead to the same result. 

#### Edge Cases
- `candidates` is empty
- `target` is invalid

#### Solution
```python 
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()
        ans = []
        curr = []

        def dfs(i, diff):
            # If we are over the target stop
            if diff < 0:
                return
            # If we're at target we're done
            elif diff == 0:
                ans.append(curr[:])
            # Otherwise keep looking
            else:
                prev = -1
                for j in range(i+1, len(candidates)):
                    if candidates[j] != prev:
                        curr.append(candidates[j])
                        dfs(j, diff-candidates[j])
                        curr.pop()
                    prev = candidates[j]
        dfs(-1, target)
        return ans
```

#### Time Complexity
`O(2^n)`

#### Space Complexity
`O(n)`

