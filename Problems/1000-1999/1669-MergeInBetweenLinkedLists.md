---
tags:
  - medium
  - linked_list
---
# 1669. Merge In Between Linked Lists
Link: [here](https://leetcode.com/problems/merge-in-between-linked-lists/)
## Problem
You are given two linked lists: `list1` and `list2` of sizes `n` and `m` respectively.

Remove `list1`'s nodes from the `ath` node to the `bth` node, and put `list2` in their place.
## Main Idea
- Use a prev and curr pointer to get the right nodes
- Get the end of list2 to connect to the linked list
## Approach
- See above
## Edge Cases/Gotchas 
- `a` can equal `b` so you can't use an `elif` when comparing to `i`
- Use a dummy node incase you are removing the 0th node
## Solution
```python 
class Solution:
    def mergeInBetween(self, list1: ListNode, a: int, b: int, list2: ListNode) -> ListNode:
        i = 0
        curr = list1
        dummy = prev = ListNode(0, curr)
        # Get end of list2
        end2 = list2
        while end2.next:
            end2 = end2.next
        
        while curr:
            # If at start of remove 
            if i == a:
                # Set prev node to head of 2
                prev.next = list2
            # If at end of remove
            if i == b:
                # Set end of 2 to next of end
                end2.next = curr.next
            # Update pointers
            prev = curr
            curr = curr.next
            i += 1
        return dummy.next```
## Time Complexity
$O(n+m)$
## Space Complexity
$O(1)$
