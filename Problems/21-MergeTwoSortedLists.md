### 21. Merge Two Sorted Lists

Link: [here](https://leetcode.com/problems/merge-two-sorted-lists/description/)

#### Topics
- Linked List

#### Problem
You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

#### Approach
This is a pretty simple problem, but there are some tricks that should be used to make this problem easier. The general approach is this:
1. Have a pointer for each list, starting at the heads.
2. Compare the values of the current node for each list, and add the smaller one to the answer linked list.
3. Advance the pointer on the list that had the node added to the answer.
4. Continue until we reach the end of one of the lists.
5. If the other list has nodes left, append the rest of that list the end of the answer list.
The other important part of this approach is determining how to start the list? We could use if statments to get the head of the list, but then we would also need to determine which list we need to advance to the next node on, and basically manually run the first iteration of this algorithm. Instead of that, we can just use a dummy node, instantiated with `dummy = ListNode()`. This allows us to just append whatever first node we want, and then at the end of the problem, we just return `dummy.next` as the answer, because we don't want to return the empty node we instantiated, we just want to use it to provide an "anchor point" to start our linked list.

#### Solution
```
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode()
        curr = dummy

        # Runs while both lists have nodes
        while list1 and list2:
            if list1.val < list2.val:
                curr.next = list1
                list1 = list1.next
            else:
                curr.next = list2
                list2 = list2.next
            curr = curr.next
        
        # Check when lists are not same length
        if list1:
            curr.next = list1
        if list2: 
            curr.next = list2
        
        return dummy.next
```

#### Time Complexity
`O(min(n,m))`, we iterate through the lists, and stop when we reach the end of the smaller list, appending the rest of the longer list is a `O(1)` operation.