---
tags: [linked_list]
---
### 25. Reverse Nodes in k-Group

Link: [here](https://leetcode.com/problems/reverse-nodes-in-k-group/description/)

#### Problem
Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return _the modified list_.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

#### Main Idea
- Move k nodes and then reverse from there to the previous head.
- Connect the updated new front and end of the reversed group accordingly

#### Approach
- Put a dummy node in-front of the list to make it easier to reverse
- Move k nodes forward
- Reverse from there to the end of the previous group (which will be the dummy node on the first iteration)
- Connect the last node in the previous group to the node that was originally k nodes away, and the node that was originally the beginning of the group to the first node in the next group.
- Repeat this until you run out of k-groups
- Return `dummy.next`

#### Edge Cases
- No nodes given
- `k` is an invalid value
- Linked list has cycles

#### Solution
```python 
class Solution:
    def reverseKGroup(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        dummy = ListNode(0, head)
        # We set prevGroup to dummy as a default val of sorts
        prevGroup = dummy

        while True:
            kth = self.getKth(prevGroup, k)
            # To break out of the while loop once we can't get a k-group
            if not kth:
                break
            nextGroup = kth.next
            # Set prev and curr to start reversing 
            prev, curr = kth.next, prevGroup.next
            while curr != nextGroup:
                temp = curr.next
                curr.next = prev
                prev = curr
                curr = temp
            # Once we reverse the group, we need to connect it
            temp = prevGroup.next
            # kth is now the head of the group since it was reversed
            prevGroup.next = kth
            prevGroup = temp
        return dummy.next
        
    def getKth(self, curr, k):
        while curr and k > 0:
            curr = curr.next
            k -= 1
        return curr```

#### Time Complexity
`O(n)` we reverse in one pass per node.

#### Space Complexity
`O(1)` since we only store variables used for reversing the current node.

