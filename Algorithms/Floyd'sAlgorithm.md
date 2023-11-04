---
tags: [linked_list, two_pointer, cycle, algorithm]
aliases: ["Floyd's Algorithm"]
---
### Floyd's Algorithm
Floyd's algorithm is a way to detect where the first node in a cycle is, using a combination of fast and slow pointers.

#### General Idea
The algorithm relies on using the fast pointer to get the slow pointer an equidistant distance away from the first pointer in the cycle, as the distance from the starting point in the graph is to the first node in the cycle.

To implement, you start a fast and slow pointer from a node, where the fast pointer moves 2 nodes per iteration, and the slow pointer only moves one. Move the pointers until they intersect. Then at this point, start another slow pointer from the original starting position, and where the new slow pointer and old slow pointer intersect is the first node in the cycle.

#### Code
```python
def floyds(head: Node):
	fast, slow = head, head
	fast = fast.next.next
	slow = slow.next

	# Find fast and slow intersection point
	while fast != slow:
		fast = fast.next.next
		slow = slow.next

	# Now find slow and new slow intersection point
	new_slow = head
	while new_slow != slow:
		new_slow = new_slow.next
		slow = slow.next

	return slow.val


```

#### Time Complexity
`O(n)`

#### Space Complexity 
`O(1)`