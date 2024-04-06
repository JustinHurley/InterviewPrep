---
tags: ["#math", "#easy", "#recursion"]
---
### 1688. Count of Matches in Tournament

Link: [here](https://leetcode.com/problems/count-of-matches-in-tournament/description/)

#### Problem
You are given an integer `n`, the number of teams in a tournament that has strange rules:

- If the current number of teams is **even**, each team gets paired with another team. A total of `n / 2` matches are played, and `n / 2` teams advance to the next round.
- If the current number of teams is **odd**, one team randomly advances in the tournament, and the rest gets paired. A total of `(n - 1) / 2` matches are played, and `(n - 1) / 2 + 1` teams advance to the next round.

Return _the number of matches played in the tournament until a winner is decided._

#### Main Idea
- We can use recursion to determine the value for the current round, and then process the next round
- We can also know that the answer will always be `n-1`

#### Approach
- See above

#### Edge Cases
- `n` is 0 or negative

#### Solution
```python 
class Solution:
    def numberOfMatches(self, n: int) -> int:
        if n <= 1:
            return 0
        elif n%2 == 0:
            return n//2 + self.numberOfMatches(n//2)
        else:
            return (n-1)//2 + self.numberOfMatches(((n-1)//2) + 1) s
```
One-Liner:
```python
class Solution:
    def numberOfMatches(self, n: int) -> int:
        return n-1
```
#### Time Complexity
`O(log(n))` since we reduce the problem space by half each time recursive call
`O(1)` for the one-line solution

#### Space Complexity
`O(n)` for call stack space only`
`O(1)` for the one-line solution

