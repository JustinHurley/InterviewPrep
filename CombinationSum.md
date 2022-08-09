### 39. Combination Sum
Link: [here](https://leetcode.com/problems/combination-sum/)

#### Topics
- Permutation
- Recursion
- Depth-first-search
- Arrays
- Backtracking

#### Problem 
Given an array of distinct integers `candidates` and a target integer `target`, return a list of all unique combinations of `candidates` where the chosen numbers sum to `target`. You may return the combinations in any order.

The same number may be chosen from `candidates` an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is guaranteed that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

#### Approach
The general approach to this problem is to treat it like a DFS problem. We want to permute each combination and then see if it's larger than the target. We can do this with a recursive method that recurs until either the sum has been reached, the sum matches the target, or you've checked every element in the array.
The way the recursive statement works is this:
- Is our current sum equal to the target? If so return the list of the sum.
- Is our current sum greater than the target? If so stop the recursion.
- If neither of these things happen, we iterate through all the items in the array starting from the element we are currently looking at. 
  - The reason we start from the node we are currently on and only look at elements to the right is to avoid double counting. Consider `[1,2,3]`. When we evaluate `1` we get the expression `1+3`. Now if we didn't only look to the right, when we were evaluating `3` we would get the expression `3+1` in the for loop. Now we are double counting.
- In the for loop that we are running, we iterate through the array, add the current element to the solution set and then recursively call to run the same method again. After, we remove the element we just added so that we can add the next element in the for loop without having to create a new list every iteration.
- <b>Note</b> it is important to make a deep copy when adding in your solution to the answer set otherwise it will get overwritten.

#### Solution
```
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        n = len(candidates)
        ans = []
        
        # Helper method to find the sum
        def findSum(idx, currSum, currSol):
            # Now check if it's equal to target
            if currSum == target:
                # Need to sort so you don't get 1,2 and 2,1 
                ans.append(currSol.copy())
                return
            # If it's greater we want to stop
            elif currSum > target:
                return
            
            for i in range(idx, n):
                currSol.append(candidates[i])
                findSum(i, currSum + candidates[i], list(currSol))
                currSol.pop()
                
        # Want to start at each point
        findSum(0, 0, [])
        
        return ans
```