---
tags: [category, "#python"]
---
## Python
Python can do a lot of the heavy lifting for some of these algorithm problems, which is why it's the language of choice for many in interviews. This file will contain Python code that will be good to remember for future problems. Understand how these methods work, it's fair game for an interviewer to ask you to do the process yourself.

#### Counter
```python
dict = Counter(array)
```
You can use the above line to automatically turn an array into a dictionary of elements, where the key corresponds to the elements in the array, and the value corresponds to the frequency of each element in the original array.

#### Sort
```python
array.sort(key=lambda x: x*x, reverse=True)

sorted_array = sorted(array, key=lambda x: x*x, reverse=True)
```
The above code sorts all the elements in the array based on their squares. It also is sorting from the largest square to the smallest because `reverse` is `True`.

#### Items
```python
for key, val in dict.items():
	ans += key*val
```
The above code unzips the dictionary key-value-pairs from `dict` where they can be used as for loop variables. 

#### List
```python
new_list = list(i*i for i in range(n))
```
The above code creates a list of sequential squares, `[0, n)` using a for loop in a list constructor. 

#### Deque 
```python
Q = deque()
Q.append(elt) # Adds element to queue
Q.popleft() # Pops leftmost element
```
A doubly-ended queue (deque) is a list that allows you to treat either end like a queue, polling and adding to the front or the back.

#### Max and Min
```python
list = None
a = max(1,2,3,4,5)
b = min(list, default=3)
return a + b # 5 + 3 = 8
```
Takes the max and min of a set of numbers, accepts arrays as input and also allows you to set default values if the input is `None`.
#### Enumerate
```python
arr = ['a','b','c','d']

for i, val in enumerate(arr):
	print(i + " " + val)
```
Enumerate lets you access the index of an element during a for loop, but also creates a variable for each iteration of the loop, as opposed to using the index to access the value from the list. 

#### Shallow Copy
```python
arr = [1,2,3]
shallow_copy = arr.copy()
shallow_copy = arr[:]
```
Creates a shallow copy, this copy object can be mutated, and the only issues where a shallow copy sees changes when the original is mutated, is if the original copy has mutable objects passed by reference e.g. a list of lists. 

#### Del
```python
dict = {'a': 1, 'b': 2, 'c': 3}
del dict['a']
dict -> {'b': 2, 'c': 3}
```
This deletes an element from a dictionary. The time complexity is effectively `O(1)`, however due to the HashTable implementation the worst case-scenario runtime would be `O(n)` if every entry in the dictionary had a hash-collision. 

#### Bisect
```python
arr = [1,2,4,4,5]
next_index_left = bisect.bisect_left(arr, 4) -> 2
next_index_right = bisect.bisect_right(arr, 4) -> 4
```
The bisect method uses binary search to find an insertion index for the requested element in a given array. In the case of `bisect_left` it finds the index to insert an element on the left side of equivalent values, and `bisect_right` gives the index to insert on the right side of equivalent values. 
If the given element we are looking to insert doesn't exist in the array, both `bisect_left` and `bisect_right` will have the same behavior. 

#### MinHeap
```python
heap = [6,3,9,2,5,1]
heapq.heapify(heap)
heapq.heappop(heap) -> 1
heapq.heappush(heap, 1)
```
Python has a built in min-heap library, that allows users to `heapify` lists in `O(n)` time, and to pop and push from the min-heap in `log(n)` time. If you want to use a max-heap, you must negate all the values. 
If you add tuples to the heap, it will resolve duplicate entries in the order of tuple elements, i.e. for `(1,2,3) < (1,2,4) = ?` the heap would say the first element is smaller.