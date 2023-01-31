### 47. Permutations II
Link: [here](https://leetcode.com/problems/permutations-ii/)

#### Topics
- Permutation
- Recursion 
- Sets
- Deep copying 
- Lists

#### Problem 
Given a collection of numbers, `nums`, that might contain duplicates, return all possible unique permutations in any order.

#### Approach
The general approach to this problem is to use recursion in a DFS-esque setup. Basically what we do is iterate through each element in `nums` and we want to run a permutation on it (you can skip duplicates to make your code non-asymptotically faster). 
The way the recursion works is we send in the index of the node we want to check, and then we add that node to the solution array that is being built. If our solution length is the same as the length of `nums`, we have a valid permutation and can add it to the answer list.
If the lengths are not equal, we have to recur on all the other elements in the list that haven't been visited yet. 
It is important to make deep copies of lists and sets that you are using as shallow copies can cause a lot of problems where solution sets aren't updated, which leads to missing answers.
Tips:
- You can wrap a list in `list()` to make a deep copy.
- You can wrap a set in `set()` to make a deep copy.
- If you want to put a list into a set or use it as a key in a dict, you need to convert it to a tuple first, which can be done by wrapping the list in a `tuple()`

#### Solution
```
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        n = len(nums)
        ans = set()
        
        # Helper function to work with permutation
        def permute(nums, idx, solution, visited):
            # First thing we do is add the current number to the solution and visited
            solution.append(nums[idx])
            visited.add(idx)
            # Now if our soln len == n, add to the ans
            if len(solution) == n:
                ans.add(tuple(solution))
                return 
            # If it doesn't we want to permute on all the other nodes
            else:
                for i in range(n):
                    # If we haven't already visited this index we should continue to build the set
                    if i not in visited:
                        permute(nums, i, list(solution), set(visited))
        
        # Now we want to run the algo on each elt in nums
        for i in range(n):
            # We want visited to be a set because there will be no collisions and it has fast lookup
            permute(nums, i, [], set())
        return ans
```