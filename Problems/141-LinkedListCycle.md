---
tags:
- linked_list
---

### 141. Linked List Cycle

Link: [here](https://leetcode.com/problems/linked-list-cycle/description/)

#### Problem
Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, `pos `is used to denote the index of the node that tail's `next` pointer is connected to. Note that `pos` is not passed as a parameter.

Return `true` if there is a cycle in the linked list. Otherwise, return `false`.

#### Approach
This is a pretty trivial problem once you know the approach. All you have to do is make a fast pointer and a slow pointer, and have the fast pointer move 2 nodes at a time, and the slow pointer move 1 node at a time. If there is a cycle, the fast pointer will eventually lap the slow pointer and catch up to it, where they will be equal. 
It is important to consider edge cases in this problem, as we need to init `fast = head.next` and `slow = head` because otherwise the pointers would be equal on the first iteration. This means if `head == None` we need to stop and just return False, as there is no way there can be a cycle with just an empty node. 

#### Solution
```python 
class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        # Edge case check for if head == None so we can't init fast
        if not head:
            return False

        fast = head.next
        slow = head

        while fast and fast.next:
            if fast == slow:
                return True
            fast = fast.next.next
            slow = slow.next
        
        return False
```

#### Time Complexity
`O(n)`, as even if there is a cycle and the fast pointer skips over the slow pointer on the first time it "laps" the slow pointer, it would at worse case end up going around the cycle 3 times, and fast is moving 2 nodes at a time, so `1.5n` iterations which is `O(n)`.