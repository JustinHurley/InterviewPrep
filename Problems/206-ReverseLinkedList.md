---
tags:
  - linked_list
  - easy
---

### 206. Reverse Linked List

Link: [here](https://leetcode.com/problems/reverse-linked-list/description)

#### Problem
Given the `head` of a singly linked list, reverse the list, and return the reversed list.

#### Approach
The approach is fairly straightforward, it just requires you to pass through the list, and change the `.next` properties of the nodes as you go along. The first part of the approach is to establish the pointers for the list you are going to pass through. There will be 2 pointers, a `curr` and a `prev` node, one to track the current node we're looking at, and one to track the previous node, as singly-linked lists only have one pointer, which points to the next node.
So we set `curr` to `head` and then we set `prev` to `None` as that the `head` node will become the last node in the list, and in a singly-linked list, `lastNode.next == None`.
When we move through the list, we need to do a swap, and it involves an extra node. We want to have `curr` point to `prev`, but we want to store `curr.next` first, or we wouldn't know what node is next. So we save `curr.next` and then set `curr.next = prev`. Now that we have reversed the direction of the pointer, we need to update the `curr` and `prev` nodes for the next iteration in the list. To do this we just set `prev` to `curr` and then we set `curr` to the node that we had save earlier, before we reversed the direction of the pointer.
Finally we return `prev` as our answer, as our loop stops when `curr` is null, so we don't want to return `None` but the new head of the list which in this case is `prev`.

#### Solution
```python 
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev = None
        curr = head

        while curr:
            # Save what node will be next (since we overwrite curr.next)
            nextCurr = curr.next
            # Set the curr.next node to prev
            curr.next = prev
            # Update prev node
            prev = curr
            # Update curr node 
            curr = nextCurr
        return prev
```

#### Time Complexity
`O(n)`, one pass through the array.
