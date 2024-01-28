---
tags:
  - medium
  - design
  - dictionary
  - trie
---
### 1166. Design File System

Link: [here](https://leetcode.com/problems/design-file-system/description/)

#### Problem
You are asked to design a file system that allows you to create new paths and associate them with different values.

The format of a path is one or more concatenated strings of the form: `/` followed by one or more lowercase English letters. For example, "`/leetcode"` and "`/leetcode/problems"` are valid paths while an empty string `""` and `"/"` are not.

Implement the `FileSystem` class:
- `bool createPath(string path, int value)` Creates a new `path` and associates a `value` to it if possible and returns `true`. Returns `false` if the path **already exists** or its parent path **doesn't exist**.
- `int get(string path)` Returns the value associated with `path` or returns `-1` if the path doesn't exist.

#### Main Idea
- File paths are very similar to the trie, data structure, so we should build a TrieNode to store values, but also have children

#### Approach
 - We will use the help of a TrieNode object, which will be helpful since the object can have a value like a node, but also have many children at the same time, as opposed to a binary TreeNode.
 - Our TrieNode will have a `map` that corresponds to the child file paths present for that given node, `name` that corresponds to the directory name the TrieNode is called, and finally the `value` that holds the actual value for that node/directory.
 - To make the root directory, we just initialize a TrieNode (similar to making the root directory which has no name)
 - For `createPath`, we can split the incoming string by `/`s and then iterate over the non-slash elements to "drill-down" to the actual directory we are looking for
 - If we can't find the parent directory, we return false
 - We must also make sure that the value for the node we are trying to add hasn't been already set, which we can do by checking to see if we are still using the default value `-1`.
 - For the get function, we similarly split up the path and try to access the directory while checking to ensure that the child directories exist first before setting `curr`
 - Once we have traversed the path, we return the value of that TrieNode, which will be -1 if never assigned.

#### Edge Cases
- Invalid input (dealing with negative number values)

#### Solution
```python 
class TrieNode:
    def __init__(self, name):
        self.map = defaultdict(TrieNode)
        self.name = name 
        self.value = -1

class FileSystem:
    def __init__(self):
        self.root = TrieNode('')
        
    # Makes new path
    # If path already exists or parent path does not return fal
    def createPath(self, path: str, value: int) -> bool:
        paths = path.split('/')[1:]
        curr = self.root
        for i in range(len(paths)):
            # If path not already created
            if paths[i] not in curr.map:
                # If at the last path, add new dir
                if i == len(paths)-1:
                    curr.map[paths[i]] = TrieNode(paths[i])
                else: 
                    return False
            curr = curr.map[paths[i]]
        # Check if val was already added
        if curr.value != -1:
            return False

        curr.value = value
        return True

    def get(self, path: str) -> int:
        paths = path.split('/')[1:]
        curr = self.root
        for i in range(len(paths)):
            if paths[i] not in curr.map:
                return -1
            curr = curr.map[paths[i]]
        return curr.value
```

#### Time Complexity
`createPath: O(k)` where `k` is the length of the path string
`get: O(k)` where `k` is the length of the path string

#### Space Complexity
`O(k)` where `k` is the length of the largest string, worst case scenario is the object is called once, and the path is very-long thus after we init the object, our space complexity scales with the paths added. 