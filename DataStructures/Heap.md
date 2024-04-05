---
aliases:
  - heap
  - min-heap
  - heaps
  - max-heap
  - minheap
tags:
  - heap
  - topic
---
## Heaps
Heaps, AKA priority queues, are tree-based data structures that satisfy the [[#heap property]]. They are particularly useful when you want to prioritize nodes on a given characteristic, typically max or min value. This is because the tree data structure allows for you to find the max or min value in constant time. 

### When to Use
Whenever you see "k-largest" or "k-smallest" you should immediately start thinking heap. Heaps are great at storing a certain amount of elements.

### Heap Property
The heap property ensures that the root node is the highest-priority node, where that priority is dependent on some relative characteristic.
**Example**
- Max-heap: parents > children
- Min-heap: parents < children
	- this one is the one made in Python by default

### Heap Operations
- `heappq.heapify(arr)` converts an array to a min-heap in `O(n)` time, since all the elements need to be inserted into the heap which takes `n` time.
	- Because it is an `O(n)` operation, you should try to add elements to a list before heapifying it.
- `heappq.heappop(arr)` pops the min element of the heap, which is stored at the root, but takes `O(log(n))` time to rebalance.
- `heappq.heappush(arr, item)` pushes `item` onto the stack, maintaining the heap property, in `O(log(n))` time. 

### Using Custom Sort Parameters
If you want to add elements to a heap, but also keep a value, you can push it onto the stack in a tuple like this:
```python
heapq.heappush(arr, (sort_key, value))
```
then just extract and pull out the value, this is effectively like a dictionary, but the benefit is order instead of fast access speed.

### Making a Heap that Prioritizes a kth Largest Element
Instead of using a heap to store the minimum value at the root, we can instead use it to store the kth largest item, by making a min-heap, and then popping every item until there are only k left. This results in a heap where the smallest element is at the root, which is the kth largest, making it easy to access. 
**Psuedocode**
```python
input: nums = [1,2,3,...]

heapq.heapify(nums)

while len(nums) > k:
	heapq.heappop(nums)

kth_largest_element = heappq.heappop(nums)
```


### Problems
```query
tag:#heap -tag:#algorithm -tag:#category
```
