---
tags:
  - linked_list
  - dictionary
  - medium
---
### 138. Copy List With Random Pointer

Link: [here](https://leetcode.com/problems/copy-list-with-random-pointer/description/)

#### Problem
A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the list. The deep copy should consist of exactly `n` **brand new** nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list**.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return _the head of the copied linked list_.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

- `val`: an integer representing `Node.val`
- `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.

Your code will **only** be given the `head` of the original linked list.

#### Approach
For this problem, it helps to break it down into steps, and to not try to solve everything at once. This problem can be simply solved in 2 steps.
1. Create a dictionary that maps from old nodes to new, unlinked nodes with copied values.
2. Pass through the old list, and use the `next` and `random` pointer values, to see what old nodes map to new nodes in the dictionary. 
Once you do that, then just return the value that the `head` maps to, and you're done.

#### Solution
```python 
class Solution:
    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':
        # Edge case where head is None
        if not head:
            return None
        # Init head pointing to nothing
        nodes = {}
        curr = head
        # First, fill the dict
        while curr:
            nodes[curr] = Node(curr.val)
            curr = curr.next
        # Now that we have all the nodes, let's build the nexts and rands
        curr = head
        while curr:
            nextNode = curr.next
            nextRand = curr.random
            # If there's a next, map it
            if nextNode:
                nodes[curr].next = nodes[nextNode]
            # If there's a rand, map it
            if nextRand:
                nodes[curr].random = nodes[nextRand]
            
            curr = curr.next
        
        return nodes[head]
```

#### Time Complexity
`O(n)` since it's solved in 2 passes.

#### Space Complexity
`O(n)` if you count the answer as part of the space complexity, otherwise `O(1)`.