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