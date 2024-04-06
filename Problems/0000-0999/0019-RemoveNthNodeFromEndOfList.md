---
tags: [medium, linked_list, two_pointer]
---
# 19. Remove Nth Node From End of List
Link: [here]()
## Problem
Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.
## Main Idea
- Use two pointers or recursively find the end of the list
- Set a dummy node before the head node so you can return `head.next` in the case that the node to remove is the head
## Approach
- Set a dummy node and have it's next be the head
- Make a DFS method that returns a node's depth from the last node and takes in the current and previous nodes as arguments 
- When that node is found, set the previous node's pointer to the current node's next
- Return `dummy.next` 
## Edge Cases/Gotchas 
- Need to handle the case where the node we are removing is the head
## Solution
```python 
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy = ListNode(0, head)
        def dfs(root, prev):
            if not root:
                return 0
            dist = dfs(root.next, root) + 1
            if dist == n:
                prev.next = root.next
            return dist
        dfs(head, dummy)
        return dummy.next
```
## Time Complexity
$O(n)$
## Space Complexity
$O(n)$ technically due to stack space but can be done with two pointers
