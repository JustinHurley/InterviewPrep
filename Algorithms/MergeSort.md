### Merge Sort
Merge sort is one of the most efficient sorting algorithms, slightly behind quick sort but complexity-wise the same. Merge sort is good to use when you can break your problem down into smaller subproblems, and then recursively rebuild the solution from the smaller subproblems.

#### General Idea
The general idea is that we want to break our existing problem space that we want to sort into smaller problems, until we end up with a binary decision, and then build our solution up from those binary decisions.
The motivation behind doing this is that a binary decision is a constant time operation, while if we didn't break down the problem space, we would have to choose what element we wanted to sort from `n` different options. Because merge sort splits the problem space by a factor of 2 each time, instead we end up shrinking `n` merges to `log(n)` merges. 
It's a recursive algorithm, where we have our base case (`when len(input) < 2`) and we return either the only element or `None`. Otherwise, we need to do the recursive case, where we call our binary merge function on half of the current problem set.
#### Code
```
mergeSort(input):
    if input == 0:
        return None
    elif input == 1:
        return input[0]
    else:
        binaryMerge(mergeSort(input[0:half]), mergeSort(input[half,end]))

binaryMerge(a, b):
    ans = []
    while a and b:
        if a < b:
            ans.append(a)
        else:
            ans.append(b)
    
    if a:
        ans.append(a)
    if b: 
        ans.append(b)
    
    return ans
```

#### Time Complexity
The time complexity is `O(nlog(k))`, where `k` is the number of starting items, and `n` is the size of each item. In a merge sort example, where the input is an array of size `n`, this would just be `O(nlog(n))`, but if the input is an array of several lists, `k` would be the number of lists, and `n` would be the length of the lists. 