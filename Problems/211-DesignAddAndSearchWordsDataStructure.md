---
tags:
- trie
- tree
- recursion
- design
- string 
- depth_first_search
---

### 211. Design Add and Search Words Data Structure

Link: [here](https://leetcode.com/problems/design-add-and-search-words-data-structure/description/)

#### Problem
Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the `WordDictionary` class:

`WordDictionary()` Initializes the object.
- `void addWord(word)` Adds word to the data structure, it can be matched later.
- `bool search(word)` Returns true if there is any string in the data structure that matches word or false otherwise. word may contain dots `'.'` where dots can be matched with any letter.

#### Approach
Given that we are creating a data structure that is able to efficiently search words, we should automatically be thinking that this is a Trie problem. So the next step is to then create a `TrieNode` class, that we can add children nodes to along with a way of tracking if we're at the end of a word. This makes it much easier to navigate the Trie, just the same way we would a Binary tree.
The next step is to then implement the given functions. The first one `addWord`, is fairly trivial, as all we have to do is iterate through the word, adding letters/TrieNodes at each level, and continuing until we reach the end of the word, where we just need to set the end-of-the-word flag to `True`.
Finally, we have to implement the `search` function. The tricky part about this function is that we need to account for wildcard characters, in this case `'.'`. To do this, we need to handle the normal case, which is iterating through the Trie and the word, ensuring that the letters exist, and returning `False` if not. The special case is when we see a `'.'` character. In this case, we need to check all the children of the node that corresponds with the wildcard since any letter in that position could be valid. When that happens we run DFS on each child node present, passing along the current node and how far along we have gotten, then recursively call the DFS function, which if it returns `True`, the search function will as well. So this problem is like half an iterative solution and half a recursive solution, for when we need to handle the wildcard case.

#### Solution
```python 
class TrieNode: 
    def __init__(self):
        self.children = {}
        self.end = False
class WordDictionary:

    def __init__(self):
        self.root = TrieNode()
        

    def addWord(self, word: str) -> None:
        curr = self.root

        for c in word:
            if c not in curr.children:
                curr.children[c] = TrieNode()
            curr = curr.children[c]
        curr.end = True
        

    def search(self, word: str) -> bool:
        curr = self.root
        def dfs(j, root):
            curr = root
            for i in range(j, len(word)):
                c = word[i]
                if c == '.':
                    # Check all possible children
                    for child in curr.children.values():
                        if dfs(i+1, child):
                            return True
                    return False
                else:
                    if c not in curr.children:
                        return False
                    curr = curr.children[c]
            return curr.end
    
        return dfs(0, self.root)
```

#### Time Complexity
`O(n)` for both `addWord` and `search`.

#### Space Complexity
`O(n)` for `addWord` and `O(1)` for `search`.

