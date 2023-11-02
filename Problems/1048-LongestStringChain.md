---
tags:
- array
- dictionary
- dynamic_programming
- string
- recursion
---
### 1048. Longest String Chain

Link: [here](https://leetcode.com/problems/longest-string-chain/description/)

#### Problem
You are given an array of `words` where each word consists of lowercase English letters.

`wordA` is a **predecessor** of `wordB` if and only if we can insert **exactly one** letter anywhere in `wordA` **without changing the order of the other characters** to make it equal to `wordB`.

- For example, `"abc"` is a **predecessor** of `"abac"`, while `"cba"` is not a **predecessor** of `"bcad"`.

A **word chain** is a sequence of words `[word1, word2, ..., wordk]` with `k >= 1`, where `word1` is a **predecessor** of `word2`, `word2` is a **predecessor** of `word3`, and so on. A single word is trivially a **word chain** with `k == 1`.

Return _the **length** of the **longest possible word chain** with words chosen from the given list of_ `words`.

#### Main Idea
The main idea is a given word's string chain is as long as the longest predecessor string chain +1. So we just need to find all the processors for each string.

#### Approach
We're going to want to put the words in a set so that we can easily find if a potential predecessor actually exists given the input. We are also going to want to use a dictionary to track computed predecessor longest chains, i.e. [[memoize]] the computed outputs to avoid duplicate processing. 
To find the string chain length of a given string, we just need to look at all permutations of potential predecessor strings, and then see if they exist in the input. If they do, we can find the longest string chain between all potential predecessors, and then return that +1. 
In our recursive methods, we will set the base cases to just either check if we are down to 1 char string (so there is no valid predecessor), or if we have already computed this value and stored the value in the memoization table.
If not, then we have to go in and consider all the potential predecessors, and determine if they exist, what their longest string chains are, and which one is the longest. 

#### Edge Cases
- Input is `[]`
- Word has no predecessors and is also longer than 1.

#### Solution
```python 
class Solution:
    def longestStrChain(self, words: List[str]) -> int:
        word_set = set(words)
        word_chain = {}

        if len(words) == 0:
            return 0

        # Function that checks for words 
        def wordSearch(word):
            # If word is 1 char, return 1
            if len(word) == 1:
                word_chain[word] = 1
                return 1
            # If already added, return val
            if word in word_chain:
                return word_chain[word]
            # Otherwise we should return max kid len
            max_len = 1
            for i in range(len(word)):
                # If word slice is a predecessor
                piece = word[:i]+word[i+1:]
                if piece in word_set:
                    max_len = max(max_len, wordSearch(piece)+1)
            # Update and return
            word_chain[word] = max_len
            return max_len

        # Now go through and start computing 
        for word in words:
            wordSearch(word)
        
        # Now return the longest
        return max(word_chain.values())
```

#### Time Complexity
`O(n*l^2)` where `l` is the length of the word.

#### Space Complexity
`O(n)` unless the strings count, in which case it becomes `O(n*l^2)` to instantiate each string from the slices. 

