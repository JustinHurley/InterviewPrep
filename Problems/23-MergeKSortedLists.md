### 23. Merge k Sorted Lists

Link: [here](https://leetcode.com/problems/merge-k-sorted-lists/description/)

#### Topics
- Linked List
- Recursion 
- Divide and Conquer
- Merge Sort

#### Problem
You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

#### Approach
Like many hard LC problems, this one involves taking an algorithm and applying it in a novel way. In this problem, we already know how to merge 2 linked lists, but now we want to merge k linked lists.
The brute force approach is to make pointers for all the linked lists, iterate over each pointer, store the minimum value, and then repeat the process until we have reached the end of each linked list in `lists`. This approach works but is not optimal, we have to iterate over `k` linked lists `n` times, so we end up with a time complexity of `O(k*n)`. In this approach, we end up looking over nodes a lot of times that we don't end up doing work on, this is because we need to find the minimum current node value at each iteration, which is a relatively expensive operation (costs `O(k)` each time).
The more optimal approach is similar to the brute force approach, but it breaks down the problem into smaller subproblems. In this case, we basically are implementing merge sort, on this problem. This way, each merge operation only happens between 2 lists, and not `k` lists. 
So how do we shrink the problem space? Well, we do that by using recursion and a helper method. Like all recursive methods, we have our base case, and out recursive case. In this scenario, the base case is when `len(lists) < 2`. When `len(list)` is 0, we have nothing to return so can just return `None`. When `len(list)` is 1, we only have one node to work with, so return `list[0]`. 
Now we need to consider the recursive case, since we want to reduce the problem space by half, we can call our 2 linked list merge method on each half, of the `k` lists we are presented with. We can do this using recursion. So it would look like:
```
return self.merge2Lists(self.mergeKLists(lists[0:k//2]), self.mergeKLists(lists[k//2:k]))
```
Now let's break down what exactly is going on in the above statement. So we want to merge two lists, which is why we call `merge2Lists`. But because we are given `k` lists, we need to recursively call `mergeKlists` on the half-slices of each list until we hit the base case. Exactly like merge sort, we break the problem down until we only have one element, and then build the sorted solution back up until we end up with the sorted result. 

#### Solution
```
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        k = len(lists)
        if len(lists) == 0:
            return None
        elif len(lists) == 1:
            return lists[0]
        else:
            return self.mergeLists(self.mergeKLists(lists[0:k//2]), self.mergeKLists(lists[k//2:k]))

    
    def mergeLists(self, a: List[Optional[ListNode]], b: List[Optional[ListNode]]) -> Optional[ListNode]:
        dummy = ListNode()
        tail = dummy

        while a and b:
            if a.val < b.val:
                tail.next = a
                a = a.next
            else:
                tail.next = b
                b = b.next
            tail = tail.next
        
        if a:
            tail.next = a
        if b:
            tail.next = b
        
        return dummy.next
```

#### Time Complexity
Because we are reducing the problem space by a factor of 2 each recursive iteration, we are now instead doing `log(k)` merges instead of `k` merges. Given a list length of `n`, this means that we end up with `O(nlog(k))` time complexity, which is the same as the time complexity of merge sort.