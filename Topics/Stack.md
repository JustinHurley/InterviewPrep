# Stack
A stack is a simple data structure, made up of a list that follows the First-In, First-Out (FIFO), principle. This data structure is good at storing and recollecting the last used element, and is good at taking elements that are in a sorted order from a given dataset, like with a [[MonotonicStack|monotonic stack]]. 

### Example 
```
input = [a, b, c, d]
stack = []
for inp in input:
	stack.append(inp)
while stack:
	 stack.pop()
################
output: d c b a
```

```query
tag:#stack -tag:#algorithm
```
