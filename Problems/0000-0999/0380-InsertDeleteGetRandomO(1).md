---
tags: [medium, design, set, array, math]
---
### 380. Insert Delete GetRandom O(1)

Link: [here](https://leetcode.com/problems/insert-delete-getrandom-o1/description/)

#### Problem
Implement the `RandomizedSet` class:
- `RandomizedSet()` Initializes the `RandomizedSet` object.
- `bool insert(int val)` Inserts an item `val` into the set if not present. Returns `true` if the item was not present, `false` otherwise.
- `bool remove(int val)` Removes an item `val` from the set if present. Returns `true` if the item was present, `false` otherwise.
- `int getRandom()` Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the **same probability** of being returned.
You must implement the functions of the class such that each function works in **average** `O(1)` time complexity.

#### Main Idea
- Use a dictionary to handle `O(1)` insert and remove (not including worst-case)
- Use an array to handle the `getRandom` function
- Swap and `pop()` elements to the last element in the array to remove in `O(1)` time (as opposed to calling remove which would be a `O(n)` operation)

#### Approach
- See above

#### Edge Cases
- Method calls aren't valid
- Invalid elements

#### Solution
```python 
class RandomizedSet:

    def __init__(self):
        self.indexMap = {}
        self.elements = []
        

    def insert(self, val: int) -> bool:
        if val in self.indexMap:
            return False
        # Add element to element array
        self.elements.append(val)
        # Add index location to map
        self.indexMap[val] = len(self.elements) - 1
        return True
        
    def remove(self, val: int) -> bool:
        if val not in self.indexMap:
            return False
        # Swap node with last
        curr_i = self.indexMap[val]
        last_val = self.elements[-1]
        # Swap arr elts and pop
        self.elements[-1], self.elements[curr_i] = self.elements[curr_i], self.elements[-1]
        self.elements.pop()
        # Update map
        self.indexMap[last_val] = curr_i
        del self.indexMap[val]
        return True

    def getRandom(self) -> int:
        return choice(self.elements)
```

#### Time Complexity
`O(1)` for all operations except for `remove` which is on-average a constant operation, but worst-case is `O(n)` if all elements map to the same hashcode. 

#### Space Complexity
`O(n)` to hold incoming elements.

