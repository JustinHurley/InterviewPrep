### 76. Minimum Window Substring

Link: [here](https://leetcode.com/problems/minimum-window-substring/description/)

#### Topics
- Sliding window
- String
- Hash map

#### Problem
Given two strings `s` and `t` of lengths m and n respectively, return the minimum window 
substring of `s` such that every character in `t` (including duplicates) is included in the window. If there is no such substring, return the empty string `""`.

The testcases will be generated such that the answer is unique.

#### Approach
Like almost all sliding window problems, this one will involve two pointers where one moves when we are in a valid case, and the other moves when we are in an invalid case. 
In this problem, we want to find the minimum length valid substring, so it makes sense to move the right pointer until we end up with a valid substring (invalid case), and then move the left pointer to make the substring as small as possible while still being valid (valid case).
To do this, we first convert the target string `t` to a dict, where we count each item in the string. This gives us a dict: `mapT` to compare the map we build with `s`. 
Once the map is built, next we iterate through the string and try to build a valid map. In each iteration we do a few things:
1. If the current char we are looking at with `r` is in `mapT`, we want to update `mapS`. This saves us from dealing with chars we don't care about and is what we use to see if we have a valid substring.
2. We then do a check with a helper method `isValid`. This function compares `mapT` and `mapS` to see if all values in `mapS` are `>=` values in `mapT`. The reason we can't just compare them directly is because there could be duplicate values, e.g. `t = 'abc'` and `s = 'abbbbbbbbc'` we could not just do `if mapT == mapS`.
When we are in a valid case, this is when we want to move up the left pointer, to see if we can make the substring smaller without making an invalid case. To do this check we use a while loop, and in the while loop, the first thing we do is compare the current valid substring to the best substring (or become the first one if there is none present). If we do find a new best, we also keep track of the `l` and `r` pointers, so that we can use them to build the answer string later, instead of storing it each time. Once we update the best smallest substring, we then update `mapS` if the current `l` pointer is also in `mapT` by decrementing the current `l` char from the map. This happens until we are no longer in a valid state, and then we go back to moving then `r` pointer.
This will continue until `r` hits the end of the string, and we stop.
Once we are done, we then look at the best value. If it was never updated from it's original value, we never found a best substring so return `""`. If it was updated, we grab the corresponding `l` and `r` pointers and build the string from `s` using `s[l:r+1]`.

**Optimizations**
While from a complexity standpoint the solution is optimal, we still have to deal with the extra potential `O(26)` operation required to ensure that all the chars in `t` are present in the current `s` substring. We can actually reduce this to `O(1)` by keeping track of how many valid chars we have so far, and comparing that to how many valid chars we need. 
The gist of the approach is that when we update the map with a new char, we don't actually need to check for the validity of the whole maps, we only actually need to check for the validity of the given char, and if it has become valid or has become invalid. To do this we simply look at if `mapS[i] >= mapT[i]` and if the condition is true, we can update some counter variable like `validS` and then compare it to some other variable like `validT` that is defined originally as the number of unique chars present in `t`. Once `validS == validT`, we know that every char is accounted for and we are looking at a valid solution. We basically optimize the `mapS` and `mapT` comparison to just `validS == validT` instead, which is an `O(1)` operation instead of `O(|s|)`.

#### Solution
**Solution Without Optimization**
```
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        lenS = len(s)
        lenT = len(t)

        # Edge case
        if lenS < lenT:
            return ""
        
        # Make a map of t
        mapT = {}
        for i in range(lenT):
            mapT[t[i]] = mapT.get(t[i], 0) + 1
        
        # Iterate through s
        mapS = {}
        best = -1
        bestPos = (0, 0)
        l = 0
        for r in range(lenS):
            # If curr in mapT, we want to also update mapS
            if s[r] in mapT:
                mapS[s[r]] = mapS.get(s[r], 0) + 1
            # If the maps are valid, we should try to move up l
            while self.isValid(mapT, mapS):
                # Compare to best and update if needed
                if (r - l + 1) < best or best == -1:
                    bestPos = (l, r)
                    best = r - l + 1
                # Then update map and move up left ptr
                if s[l] in mapT:
                    mapS[s[l]] = mapS[s[l]] - 1
                l += 1
    
            # At this point we're no longer valid
        # Once loop is done we have best range, need to convert to str
        if best == -1:
            return ""
        else: 
            return s[bestPos[0]:bestPos[1]+1]
    
    def isValid(self, mapS, mapT):
        # Every t val should be >= every s val
        for key in mapS:
            if key not in mapT or mapT[key] < mapS[key]:
                return False
        return True
```
**Solution with Optimization**
```
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        lenS = len(s)
        lenT = len(t)

        # Edge case
        if lenS < lenT:
            return ""
        
        # Make a map of t
        mapT = {}
        for i in range(lenT):
            mapT[t[i]] = mapT.get(t[i], 0) + 1
        countT = len(mapT)
        
        # Iterate through s
        mapS = {}
        best = -1
        bestPos = (0, 0)
        countS = 0
        l = 0
        for r in range(lenS):
            # If curr in mapT, we want to also update mapS
            if s[r] in mapT:
                mapS[s[r]] = mapS.get(s[r], 0) + 1
                # If we have a valid char, inc. count
                # Only do it when equal to prevent dupes
                if mapS[s[r]] == mapT[s[r]]:
                    countS += 1
            # If the maps are valid, we should try to move up l
            while countS == countT:
                # Compare to best and update if needed
                if (r - l + 1) < best or best == -1:
                    bestPos = (l, r)
                    best = r - l + 1
                # Then update map and move up left ptr
                if s[l] in mapT:
                    mapS[s[l]] = mapS[s[l]] - 1
                    if mapS[s[l]] < mapT[s[l]]:
                        countS -= 1
                l += 1
    
            # At this point we're no longer valid
        # Once loop is done we have best range, need to convert to str
        if best == -1:
            return ""
        else: 
            return s[bestPos[0]:bestPos[1]+1]
```

#### Time Complexity
If we convert the psuedocode to time complexity, we end up with this:
```
    O(lenT) -> building map

    O(lenS) -> iterating through s:
        O(26) -> isValidCheck
    
    O(lenS) -> build string
```
so ultimately, we end up with a time complexity of `O(|s|+|t|)`, but if `|s| < |t|` then the answer will always be `""` which is a `O(1)` operation, so we only care when `|s| >= |t|` which means worst case `|s| == |t|` which means we can say `O(2|s|) => O(|s|)`. We can ignore the while loop since it will also run at most `|s|` times since `l` is also constrained by the size of `s` and if `l == r` then the length would be 0 and we would definitely not be in a valid state.

