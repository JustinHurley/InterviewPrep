---
tags: [trie, tree, array, string, dictionary, design]
---

### 208. Implement Trie (Prefix Tree)

Link: [here](https://leetcode.com/problems/implement-trie-prefix-tree/description/)

#### Problem
A trie (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

- `Trie()` Initializes the trie object.
- `void insert(String word)` Inserts the string word into the trie.
- `boolean search(String word)` Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
- `boolean startsWith(String prefix)` Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.

#### Approach
The most important part of the approach to this problem is to create a TrieNode object. This will allow us to navigate the Trie like a tree. The TrieNode needs 2 things, a dictionary that holds child values, and a boolean to determine if this is the end of a word, or just a prefix.

**Insert**
Insert starts at the root node, which is the only attribute of the Trie object. All we do is iterate through the given word to insert, and we check the current node to see if it contains the current character we are on. If it does, we go to the child node and continue, and if it doesn't, we simply add the node to the child dict and keep going.

**Search**
Search starts at the root node, and iterates through the children at each level to confirm the current character exists at that level, and if it doesn't we return false. Finally, now that we are at the node corresponding to the last letter in the word (if we didn't return false already), we ensure that the boolean to represent if the current character is at the end of the word is true.

**StartsWith**
The same as search, we just don't check the to see if the last character is also the end of the word. 


#### Solution
```python 
class TrieNode:
    def __init__(self):
        self.children = {}
        self.end = False
        
class Trie:

    def __init__(self):
        self.root = TrieNode()
        

    def insert(self, word: str) -> None:
        curr = self.root

        for c in word:
            if c not in curr.children:
                curr.children[c] = TrieNode()
            curr = curr.children[c]
        curr.end = True


    def search(self, word: str) -> bool:
        curr = self.root

        for c in word:
            if c not in curr.children:
                return False
            curr = curr.children[c]
        return curr.end
        
    def startsWith(self, prefix: str) -> bool:
        curr = self.root
        for c in prefix:
            if c not in curr.children:
                return False
            curr = curr.children[c]
        return True
```

#### Time Complexity
Insert: `O(n)` where `n` is the number of characters in the string.
Search: `O(n)` where `n` is the number of characters in the string.
StartsWith: `O(n)` where `n` is the number of characters in the string.

#### Space Complexity
`O(n)` you make one entry or less per char, so worst case the size of the tree is `O(n)` where `n` is all the chars. 

