---
tags:
  - hard
  - string
  - breadth_first_search
  - graph
  - dictionary
---
### 127. Word Ladder

Link: [here](https://leetcode.com/problems/word-ladder/description/)

#### Problem
A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return _the **number of words** in the **shortest transformation sequence** from_ `beginWord` _to_ `endWord`_, or_ `0` _if no such sequence exists._

#### Main Idea
- Convert the input into a graph, where words are edges, and wildcard string patterns are keys
- [[BreadthFirstSearch|BFS]] on the graph until you find the answer, and return distance

#### Approach
The hardest part of this problem is knowing the best way to go about building the adjacency list. To do this, we will be creating pattern strings to represent potential strings that can go from word A to word B. For example "dog" and "log" are one letter off, so to go from one to the other, we do "dog" -> "\*og" -> "log".
Doing this will allow us to generate a pattern for a given word, and find a collection of all the words that have that same pattern. All words that share a pattern are one letter swap away from each other.
Now that we have built the list, it's just a matter of starting with the `beginWord`, and then adding it to a queue and running BFS.
Since we want to track the overall distance in BFS, it's important that we process all the elements in the queue as a batch process, instead of continually processing the nodes in just a `while` loop.
For each word, we generate potential patterns, see if they have been visited yet, and if not, then add all the words that are 1 char off to the queue.

#### Edge Cases
- When `beginWord == endWord`
- When input elements are missing 
- Invalid input

#### Solution
```python 
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        # Edge case
        if beginWord == endWord:
            return 0

        # Process word list into dict
        wordMap = defaultdict(list)
        wordList.append(beginWord)
        for word in wordList:
            for i in range(len(word)):
                pattern = word[:i]+'*'+word[i+1:]
                wordMap[pattern].append(word)
        
        # Now run BFS
        Q = deque([beginWord])
        visited = set([beginWord])
        ans = 0
        while Q:
            ans += 1
            for _ in range(len(Q)):
                curr = Q.pop()
                if curr == endWord:
                    return ans
                else:
                    for i in range(len(curr)):
                        pattern = curr[:i] + '*' + curr[i+1:]
                        for neighbor in wordMap[pattern]:
                            if neighbor not in visited:
                                Q.appendleft(neighbor)
                                visited.add(neighbor)
        return 0```

#### Time Complexity
`O(m^2 * n)` where `m` is the length of the words and `n` is the number of words

#### Space Complexity
`O(m^2 * n)`

