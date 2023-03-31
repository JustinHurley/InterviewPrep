### 143. Reorder List

Link: [here](https://leetcode.com/problems/reorder-list/description/)

#### Topics
- Linked List

#### Problem
You are given the head of a singly linked-list. The list can be represented as:
```
L0 → L1 → … → Ln - 1 → Ln
```
Reorder the list to be on the following form:
```
L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
```
You may not modify the values in the list's nodes. Only nodes themselves may be changed.

#### Approach
We want to change the list so that it alternated between first and last nodes, moving to the middle. While this may seem hard to do, you can just break the problem down into much easier subproblems to build your answer. Really to solve this you need to do these things:
1. Find the middle node in the list.
2. Reverse the second half of the list.
3. Merge the two lists together.
4. Return the answer.
To find the middle node in the list, we can just use two pointers, a fast and a slow one, where the fast pointer moves 2 nodes at a time, and the slow pointer moves one node at a time. The slow pointer will then be equivalent to the middle node of the list when the fast pointer reaches the end of the list. It is important that while we move the fast pointer through the list, we don't just check that the fast pointer node exists, but also the next node after as well. This is because we need to do `fast.next.next` and if `fast.next` is null, we'll get a NullPointerException.
To reverse the second half of the list, we simply use a `prev` and `curr` pointer, swapping the `curr.next` and moving up each pointer by one node per iteration.
To merge the 2 lists together, we can use a flag variable that oscillated between `True` and `False`, adding a node from either list until we reach the end of one of the lists. A more efficient way to do this, is to just add the first node then the second node at each iteration.
The final consideration is when we are dealing with a case where `len(firstHalf) != len(secondHalf)`. To do this, we just check the first half of the list, to see if there is still a node present, and if so we add it to the answer list. It is important to also overwrite the next node when you add this node as it will still be pointing to the first node in the second half of the list, so you need to also append a `None` as the final node afterwards, otherwise you will end up with a cycle. 

#### Solution
```
class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        # Find middle of list
        fast = head
        head2 = head
        
        while fast and fast.next:
           head2 = head2.next
           fast = fast.next.next

        # Reverse second half of list
        prev = None
        curr = head2

        while curr:
            nextCurr = curr.next
            curr.next = prev
            prev = curr
            curr = nextCurr
        
        head2 = prev

        # Merge lists
        dummy = ListNode()
        tail = dummy
        useHead = True
        while head and head2:
            if useHead:
                tail.next = head
                head = head.next
            else: 
                tail.next = head2
                head2 = head2.next
            tail = tail.next
            useHead = not useHead
        
        # Account for when lists aren't even
        if head:
            tail.next = head
            tail = tail.next
            tail.next = None
        
        return dummy.next
```

#### Time Complexity
`O(3n) = O(n)`, we iterate over the list 3 times. 