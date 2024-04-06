---
tags: ["#two_pointer", easy, sorting, greedy, array]
---
### 455. Assign Cookies

Link: [here](https://leetcode.com/problems/assign-cookies/)

#### Problem
Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie.

Each child `i` has a greed factor `g[i]`, which is the minimum size of a cookie that the child will be content with; and each cookie `j` has a size `s[j]`. If `s[j] >= g[i]`, we can assign the cookie `j` to the child `i`, and the child `i` will be content. Your goal is to maximize the number of your content children and output the maximum number.

#### Main Idea
- Give a cookie to the person with the highest greed index, but will still be content so the cookie is not wasted

#### Approach
- Sort the arrays first so we know where the largest items are
- Iterate through both arrays, if the cookie value is less than the kid greed value, skip the kid and try to find the most greedy kid that will be content with the cookie

#### Edge Cases
- Either input set is null
- Elements are not numbers

#### Solution
```python 
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        kids = sorted(g, reverse=True)
        cookies = sorted(s, reverse=True)
        ans, k_i, c_i = 0, 0, 0

        while k_i < len(kids) and c_i < len(cookies):
            # If kid is content with cookie
            if cookies[c_i] >= kids[k_i]:
                ans += 1
                c_i += 1
            k_i += 1
        
        return ans
```

#### Time Complexity
`O()`

#### Space Complexity
`O()`

