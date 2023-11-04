---
tags: [queue, category]
---
# Queues

A **queue** is a data structure that stores elements in a First-In First-Out (FIFO) methodology. So think of a queue exactly like a queue that people wait in, where the first person in line is served first, then the second, etc.

### Visual Representation 

![img](https://media.geeksforgeeks.org/wp-content/cdn-uploads/gq/2014/02/Queue.png)

### Operations
- **Enqueue**: Adds an element to the queue in `O(1)` time, this operation is similar to "getting in line" `Q.append(elt)`
- **Dequeue**: Pops an element from the queue from the front of the line (end of the array) in `O(1)` time as well, analogous to being at the front of a line and being served `Q.popleft()`
- **Front**: Allows you to look at the the next element to be dequeued in `O(1)` time, analogous to seeing who is at the front of the line. `Q[0]`
- **Back**: Allows you to look at the last element to be added to the queue in `O(1)` time, analogous to seeing who was the last person to get in line. `Q[-1]`

![[Python#Deque]]

### Problems
```query
tag:#queue -tag:#algorithm -tag:#category
```