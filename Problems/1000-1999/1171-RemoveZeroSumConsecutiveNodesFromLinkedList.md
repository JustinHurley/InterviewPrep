---
tags:
  - medium
  - linked_list
  - prefix_sum
  - dictionary
---
# 1171. Remove Zero Sum Consecutive Nodes from Linked List
Link: [here](https://leetcode.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/)
## Problem
Given the `head` of a linked list, we repeatedly delete consecutive sequences of nodes that sum to `0` until there are no such sequences.

After doing so, return the head of the final linked list.  You may return any such answer.
## Main Idea
- Use a dictionary to keep track of the given prefix sum
- If we already see the sum in the dictionary, it means that between that sum and now, we had a zero sum (since we are back at the amount we started at)
- Use a dummy node to handle dealing with removing the head node 
## Approach
- Set a `curr` node equal to the head
- Set a dummy node with `next` being the head node
- Make a hash map with the dummy node and a prefix sum of 0
- Now have a while loop that runs while the current node is next
	- If we see a sum that is already in the dict, we want to remove all nodes from the node with that prefix sum up to and including the current node, i.e. `prev.next. = curr.next`
	- We now need to look at the new linked list so we reset all the values associated with finding the prefix sum and start over from `dummy.next` or the new head node
	- As we move through the linked list we update the hash map with updated sums and the corresponding nodes associated with that given prefix sum
- Return `dummy.next` for the list
## Edge Cases/Gotchas 
- Need a dummy node to handle if the `head` node is removed
## Solution
```python 
class Solution:
    def removeZeroSumSublists(self, head: Optional[ListNode]) -> Optional[ListNode]:
        curr = head
        dummy = ListNode(0, head)
        nodes = {0: dummy}
        curr_sum = 0

        while curr:
            # Update sum
            curr_sum += curr.val
            # If we already saw the sum remove from prev to curr
            if curr_sum in nodes:
                # Remove subseq and reset
                prev = nodes[curr_sum]
                prev.next = curr.next
                curr = dummy
                curr_sum = 0
                nodes = {}
            # Otherwise update sum
            nodes[curr_sum] = curr
            curr = curr.next
        
        return dummy.next
```
## Time Complexity
$O(n)$ since we only start over when nodes are removed, meaning we never process the "same" linked list twice
## Space Complexity
$O(n)$ for the dictionary 
