---
tags:
- design
- heap
- sorting
---
### 295. Find Median from Data Stream

Link: [here](https://leetcode.com/problems/find-median-from-data-stream/description/)

#### Problem
The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.
- For example, for `arr = [2,3,4]`, the median is `3`.
- For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.
Implement the `MedianFinder` class:
- `MedianFinder()` initializes the `MedianFinder` object.
- `void addNum(int num)` adds the integer `num` from the data stream to the data structure.
- `double findMedian()` returns the median of all elements so far. Answers within `10-5` of the actual answer will be accepted.

#### Main Idea
The main idea of this problem is using a 2 [[heap]] approach to keep track of the middlemost values, using a minHeap and a maxHeap that is rebalanced to ensure that the top of each heap has the middlemost value.

#### Approach
The real meat of the problem is implementing the `addNum` method. This is where you'll need to add the num to the heap, and balance the heap if needed. The first part of the process is knowing which heap to choose. This is done by looking at the minHeap (which stores the larger values), and seeing if the current num is larger than the smallest value in the heap of larger values, i.e. is this a larger value. If not, it goes in the set of smaller values.
Now the other part of the problem is rebalancing, which is done by checking to see if the length-difference between the 2 heaps is more than 1. If so, the heap with more values pops the heap with fewer values, and moves it into the other heap.

#### Edge Cases
- Negative numbers means you need to be careful of converting or directly accessing top-of-heap values.
- Remembering to switch the sign when adding to the maxHeap, since Python only natively supports minHeaps.
- Asking for medians when there are no elements.
- Checking heaps before trying to access them.

#### Solution
```python 
class MedianFinder:

    def __init__(self):
        # Left
        self.maxHeap = []
        # Right
        self.minHeap = []
        

    def addNum(self, num: int) -> None:
        # If larger than smallest in minHeap, add to minHeap
        if self.minHeap and self.minHeap[0] < num:
            heapq.heappush(self.minHeap, num)
        # Otherwise we need to add to maxHeap
        else:
            heapq.heappush(self.maxHeap, -num)
        # Now we need to rebalance if necessary
        if abs(len(self.minHeap) - len(self.maxHeap)) > 1:
            self.rebalance()

    def rebalance(self):
        if len(self.minHeap) > len(self.maxHeap):
            moveNum = heapq.heappop(self.minHeap)
            heapq.heappush(self.maxHeap, -moveNum)
        else:
            moveNum = heapq.heappop(self.maxHeap)
            heapq.heappush(self.minHeap, -moveNum)
        
    def findMedian(self) -> float:
        if len(self.minHeap) == len(self.maxHeap):
            return (self.minHeap[0] - self.maxHeap[0])/2
        else:
            return self.minHeap[0] if len(self.minHeap) > len(self.maxHeap) else -self.maxHeap[0]
```

#### Time Complexity
```
__init__: O(1)
addNum: O(log(n))
findMedian: O(1)
Overall: O(nlog(n))
```

#### Space Complexity
```
Overall: O(n)
```

