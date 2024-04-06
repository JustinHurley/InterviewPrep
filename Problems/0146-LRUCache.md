---
tags: [medium, design, linked_list, dictionary]
---
# 146. LRU Cache
Link: [here](https://leetcode.com/problems/lru-cache/description/)
## Problem
Design a data structure that follows the constraints of a **[Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU)**.

Implement the `LRUCache` class:
- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.
The functions `get` and `put` must each run in `O(1)` average time complexity.
## Intuition
- We want fast lookup, so something like a dictionary will allow for an easy way to store and get key values
- We also need to keep track of the front and back of the array
- This seems like a good scenario to choose a `deque` but that would still cost $O(n)$ time every time we needed to move/remove an element that is in the middle of the deque (which would happen when we `get` a node, or when we `put` a node that already exists in the list)
- The workaround for this is to use a doubly-linked list with a dictionary, which allows for $O(1)$ lookup for any value, $O(1)$ insert since we just add to the end, and removing a node from the list can also be done in $O(1)$ time since we can get the node from the dictionary
## Approach
- We need to make a node class, that stores a key, a value, and then pointers to the previous and next nodes
- To `init` our LRU class, we are going to want to use a dictionary to store the key to node mappings, and to also store the cap on the size of the LRU.
- To keep track of the least and most recently used nodes, we will be using two pointers to keep track of left and right nodes, these won't hold any values but just sit there 
- We want to define `insert` and `remove` methods, to make adding and removing from the doubly linked list
- Insert removes the old value if present, then adds the new node between the last node and the right pointer
- Remove just finds the old node and takes it out
- For `get`, if the node is present we want to move it to the front of the list and then return the value, otherwise return `-1`
- For `put`, first we remove the node if it's already in the list, then we add the updated value. The important extra part is to then check the size of the cache, and to remove the LRU node if there are too many values 
## Edge Cases/Pitfalls
- Knowing to pop the LRU node when the size of the cache is larger than the cap
- When removing a node fully and not replacing it, we need to `del` the node from the dictionary
## Solution
```python 
class Node:
    def __init__(self, key, val):
        self.key, self.val = key, val
        self.prev = self.next = None

class LRUCache:
    def __init__(self, capacity: int):
        self.cap = capacity
        # For O(1) lookup
        self.cache = {}
        # Pointerst to keep track of order
        self.left, self.right = Node(0, 0),  Node(0, 0)
        self.left.next = self.right
        self.right.prev = self.left
        
    # Get if possible and update elt to MRU
    def get(self, key: int) -> int:
        if key in self.cache:
            self.remove(self.cache[key])
            self.insert(self.cache[key])
            return self.cache[key].val
        return -1

    # Put node in DS, remove LRU if over cap
    def put(self, key: int, value: int) -> None:
        # Remove old element if present 
        if key in self.cache:
            self.remove(self.cache[key])
        self.cache[key] = Node(key, value)
        self.insert(self.cache[key])
        # Remove LRU if added element put us over cap
        if len(self.cache) > self.cap:
            to_del = self.left.next
            self.remove(to_del)
            del self.cache[to_del.key]
    
    # Insert node in list between last node and right pointer
    def insert(self, node):
        prev, nex = self.right.prev, self.right
        prev.next = nex.prev = node
        node.prev, node.next = prev, nex

    # Remove node from list
    def remove(self, node):
        prev, nex = node.prev, node.next
        prev.next, nex.prev = nex, prev```
## Time Complexity
$O(1)$ for all operations so $O(n)$ for the total number of operations called
## Space Complexity
$O(k)$ where `k` is the capacity of the list
## Takeaways 
- If they want $O(1)$ then you will very likely need a dictionary or set
- Using dummy nodes makes adding and removing easier 
