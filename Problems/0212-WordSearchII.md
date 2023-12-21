---
tags:
  - trie
  - recursion
  - depth_first_search
  - backtracking
  - matrix
  - string_array
  - hard
---

### 212. Word Search

Link: [here](https://leetcode.com/problems/word-search-ii/description/)

#### Problem
Given an `m x n` `board` of characters and a list of strings `words`, return all words on the board.

Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

#### Approach
This problem is actually just a combination of implementing a Trie along with a [[Categories/Backtracking|backtracking]] algorithm. The general approach is this:
1. Generate a trie from the list of words. This will make it faster to search for a word, and allow us to keep track of the current letter we are on.
2. Iterate over every element in the matrix, searching for words at each position, using the trie to help.

Generating the trie is trivial, we want to store the children at each node, and also keep track of where the ends of words are. This is done by creating a `TrieNode` object and initializing a root node, which at the beginning, has no children and is not the end of the word.
Once we have made the trie, we are ready to iterate through the matrix. This is done using a simple DFS from each coordinate and running recursively. Each recursive call will look like this:
- Base case: if the current coordinates are out of bounds, or the current node has already been visited, or we have reached the end of the trie and not found any words, we can just `return`.
- Recursive case: we visit the current coordinate, add the current char to the current word, update the trie to be the `TrieNode` associated with the current coordinate. We also then check to see if the current `TrieNode` represents the end of the word, if so, we add it to the global answer list that we are tracking, and then make the recursive calls. We will make 4 recursive calls, one for each orthogonal direction from the current coordinate. Note that we must "unvisit" the coordinates after we make all the recursive calls, as we are sharing a visited set amongst all the iterations. This also ensures we use much less space, and don't need to make a new set for `m*n` elements.
  
**NOTE**: This approach will fail on the `Time Limit Exceeded` test cases, as the problem expects you to remove already found words from the trie as you go, however this will not affect the overall time complexity.

#### Solution
```python 
class TrieNode: 
    def __init__(self):
        self.children = {}
        self.end = False
    
    def addWord(self, word):
        curr = self
        for char in word:
            # If new char, add child
            if char not in curr.children:
                curr.children[char] = TrieNode()
            # Always iterate through Trie
            curr = curr.children[char]
        # Mark end of word
        curr.end = True
        

class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        root = TrieNode()
        rows = len(board)
        cols = len(board[0])
        ans = set()
        board = board
        visited = set()
        # First build trie
        for word in words:
            root.addWord(word)
            

        def searchWords(currTrie: TrieNode, row: int, col: int, currWord: str): 
            # If OOB, visited, or no children, stop
            if row < 0 or col < 0 or rows <= row or cols <= col or (row, col) in visited or board[row][col] not in currTrie.children:
                return 

            currChar = board[row][col]
            visited.add((row,col))
            # If we are here, process next iteration
            currWord += currChar
            currTrie = currTrie.children[currChar]

            # Check if we have a valid word
            if currTrie.end:
                ans.add(currWord)

            # Otherwise start next iter
            searchWords(currTrie, row-1, col, currWord)
            searchWords(currTrie, row+1, col, currWord)
            searchWords(currTrie, row, col-1, currWord)
            searchWords(currTrie, row, col+1, currWord)
            visited.remove((row,col))


        # Now that trie is made we need to start searching words
        # Will have to start at every starting point
        for row in range(rows):
            for col in range(cols):
                searchWords(root, row, col, "")
        return ans
```

#### Time Complexity
`O(m*n)^2` since we visit each node, and at each node, could potentially visit every other node. 

#### Space Complexity
`O(m*n)` we create a few objects to track visited nodes and the answer set, which are dependent on the size of the board.

