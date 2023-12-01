---
tags:
  - dynamic_programming
  - string_array
  - medium
---

### 139. Word Break

Link: [here](https://leetcode.com/problems/word-break/description/)

#### Problem
Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

#### Approach
In this problem we have to determine if the given string can be segmented into smaller strings that match the given dictionary. This means that each char can potentially be part of any word, and we need to assess each situation, because otherwise we could miss out on words within words. Consider `s = 'abcdef` and `wordDict = {'abcd', 'ef', 'abc'}`. Just because we are able to extract `abc` from `s` it doesn't necessarily mean that the remaining string will be a valid word, hence the need for permutations of the different possibilities, and when we see this we should start thinking: "[[DynamicProgramming|Dynamic programming]]."
The next step is to identify the base case, and how we should build that base case. For this problem we will start at the back of the string and work our way forward, but it doesn't really matter which way we iterate through the string as long as we accommodate for that in the inner code.
So the base case for this problem would be the string `""`, which we would say is `true` as there are no chars to match. We can think of this as when all the words have already been parsed out of the string, and we are at the end. So for a string of length `n`, this would require an array of length `n+1` to account for that base case at index `n`, where we set it to true.
Now that we have set the base case, we need to populate the rest of the DP array. To do this, we want to work our way backwards through the string and evaluate if we can find words from `i` to `n` inclusive that match the dictionary. We do this by looking at the current index and then iterating through all the possible words that it could be (by looping through `wordDict`). If we find that `s[i:len(currentWord)] == currentWord` then we know we found a valid word in the dictionary, and if we know that we are able to make valid words from the rest of the string, which would either be for the index `n` if this is the first word or just the index after the current one is true in the corresponding DP array. So this logic would look like this:
```python
# If the word fits and matches 
if len(word) + i <= n and s[i:i+len(word)] == word:
    # Set the curr word to the state of the prev word 
    dp[i] = dp[i+len(word)] 
# Once it's true we want to stop so we don't overwrite the True
if dp[i]:
    break
```
So basically we do a check to make sure that our word would be in bounds of the string length, and then we also make sure that we are actually looking at a word in the dictionary with the second half of the if statement. If those conditions are met, then we set the current DP array index to the DP array value of the index right after the current word. Meaning if we are able to make a word from the current index `i` and the next position is also true then the rest of the words after the word we are currently looking at are valid. We just need to make sure to break once we set the value to true, since we could also get cases where the word matches but it results in being left with a non-string word suffix, so we need to be aware that there are multiple valid words that can fit, but not all of them may lead to a valid answer.

#### Solution
```python 
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        n = len(s)
        dp = [False]*(n + 1)
        # Seed value to create state with no words, hence true
        dp[n] = True

        for i in range(n-1,-1,-1):
            for word in wordDict:
                # If the word fits and matches 
                if len(word) + i <= n and s[i:i+len(word)] == word:
                    # Set the curr word to the state of the prev word 
                    dp[i] = dp[i+len(word)] 
                # Once it's true we want to stop so we don't overwrite the True
                if dp[i]:
                    break
        return dp[0]
```

#### Time Complexity
`O(len(s)*len(wordDict))`

#### Space Complexity
`O(len(s))`
