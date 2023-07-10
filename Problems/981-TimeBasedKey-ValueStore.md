---
tags:
- dictionary
- string_array
- binary_search
---

### 981. Time Based Key-Value Store

Link: [here](https://leetcode.com/problems/time-based-key-value-store/description/)

#### Problem
Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.

Implement the `TimeMap` class:

`TimeMap()` Initializes the object of the data structure.
`void set(String key, String value, int timestamp)` Stores the key key with the value value at the given time timestamp.
`String get(String key, int timestamp)` Returns a value such that set was called previously, with `timestamp_prev <= timestamp`. If there are multiple such values, it returns the value associated with the largest `timestamp_prev`. If there are no values, it returns `""`.

#### Approach
For this problem, the init method is straightforward, just make a list that stores the keys with the values, and given that one key can be associated with multiple values, it is important to keep a list of the multiple values. Because the values are also associated with timestamps, we want to keep the values stored with their timestamps. So the data structure would look like: `{key: [[value, timestamp]]}`. 
To set a value in the array, we just see if the key exists already, if it doesn't we create a blank list and append our value to it, otherwise we just append the value to the existing list.
The tricky part of the question is coming up with an approach to the `get` method, as we want to quickly access the value associated with the closest timestamp at the same time or before our target time. We don't want to get a value after our target time. 
Because the problem states that the set method will remain strictly increasing, we can use binary search to find the closest smaller timestamp to our target time. 
To do so, we set up the problem like binary search normally, the only difference is that when we go left, we update the current answer with the midpoint timestamp. This is because we only go left when the midpoint timestamp is greater than the target timestamp (so we know it's valid), and then we move the search window to the left, so we know we will only evaluate timestamps smaller than the current timestamp we just looked at. This ensures we don't accidentally assign a larger timestamp to the target that is also valid.

#### Solution
```python 

```

#### Time Complexity