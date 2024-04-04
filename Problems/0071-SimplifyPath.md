---
tags:
  - medium
  - string
  - heap
---
# 71. Simplify Path
Link: [here](https://leetcode.com/problems/simplify-path/description/)
## Problem
Given a string `path`, which is an **absolute path** (starting with a slash `'/'`) to a file or directory in a Unix-style file system, convert it to the simplified **canonical path**.

In a Unix-style file system, a period `'.'` refers to the current directory, a double period `'..'` refers to the directory up a level, and any multiple consecutive slashes (i.e. `'//'`) are treated as a single slash `'/'`. For this problem, any other format of periods such as `'...'` are treated as file/directory names.

The **canonical path** should have the following format:
- The path starts with a single slash `'/'`.
- Any two directories are separated by a single slash `'/'`.
- The path does not end with a trailing `'/'`.
- The path only contains the directories on the path from the root directory to the target file or directory (i.e., no period `'.'` or double period `'..'`)
Return _the simplified **canonical path**_.
## Intuition
- We need to clean the input, and also handle `.` and `..`
- We can ignore `.` since it means same directory
- If we see `..` we should just remove the last directory added 
- When we want to remove the last thing, we should be using a stack
## Approach
- Split the string with `/`
- Go through each string made by the split
- If we have `..` and files in our answer, pop the last file in the path
- If we have `.` or `''`, we can just do nothing
- If we have a normal directory, we can just add `/${directory_name}` to our answer
- Finally we build the answer string and return it
- If we are at the root we still want to return a `/`
## Edge Cases/Gotchas 
- If we call `..` from the root we should just return the root directory
- If we have no path at the end, we should return the root directory 
## Solution
```python 
class Solution:
    def simplifyPath(self, path: str) -> str:
        # First pull out items
        paths = path.split('/')
        ans = []
        for folder in paths:
            # If we want to go up, pop last file
            if folder == '..':
                if ans:
                    ans.pop()
            # If we don't have . or none, add the file to the ans
            elif folder != '.' and folder != '':
                ans.append('/'+folder)
        # Either make path or we are at root
        return ''.join(ans) or '/'
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$
## Takeaways 
- Cleaning the data can make a problem much easier 
