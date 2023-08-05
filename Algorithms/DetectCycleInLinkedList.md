---
tags:
- algorithm
- linked_list
- two_pointer
---
### Detect Cycle in Linked List
This is a pretty specific algorithm, but can be useful in linked list problems where we need to find the cycles in a linked list. 

#### General Idea
The general idea is that if there's a cycle, we'll be stuck in it forever. So if we use two pointers, and they both end up in the same cycle, if we have a faster pointer and a slower pointer, the faster one will eventually catch up and "lap" the slower pointer. So once the pointers end up one the same node, we know we're in a cycle. Otherwise, the fast node would reach the end of the list, and we would know there are no cycles. 
We can also do an optimization on the algorithm by moving the pointers before we check the to see if they are equal, as that allows us to set the fast and slow pointers to `head` which saves us the initial check in the beginning to see if `head == None`.

#### Code
```
input: head (type ListNode)

slow = head
fast = head

while fast and fast.next:
    slow = slow.next
    fast = fast.next.next
     if fast == slow:
        return True

return False
```

#### Time Complexity
`O(n)`