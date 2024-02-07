---
tags: [algorithm, sorting]
aliases: [counting sort, bucket sort, Bucket Sort, Counting Sort, countingsort, CountingSort]
---
# Algorithm
Counting sort is a way to sort and store information for inputs of relatively small n, in less than the conventional `nlog(n)` sorting time. This is because for counting sort we use the input values to designate the array index locations, similar to a hash-map.
Counting sort really only works on numbers, as we need a direct 1:1 mapping between an input value and an integer.
The only downside is that it can only be used with "reasonably small" input sizes as there is no way to protect against hash-collisions, meaning that every tracked element must be unique. 
- Great to use when the input is only alphanumeric characters, especially when we want to get the frequency 
## General Idea
- Pass through the input data
- For each element transform the input (if necessary) to the index slot in a way that maintains order and increment the index in some way to indicate that the current element is present
- Once finished, the array can be scanned in order to get the sorted output
## Code
```python
input = [1,4,2,8,3]
arr = []*max_input_value

for num in input:
	arr[num] += 1

# Can also use with chars
input = ['a', 'b', 'c', 'c']
arr = []*26 # For each letter of the alphabet

for char in arr:
	arr[ord(char)] += 1
```
## Time Complexity
`O(n)`
## Space Complexity 
`O(n)`