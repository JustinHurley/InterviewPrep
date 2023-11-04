---
tags: [bit_manipulation, array]
---
### 2433. Find the Original Array of Prefix Xor

Link: [here](https://leetcode.com/problems/find-the-original-array-of-prefix-xor/description/)

#### Problem
You are given an **integer** array `pref` of size `n`. Find and return _the array_ `arr` _of size_ `n` _that satisfies_:
- `pref[i] = arr[0] ^ arr[1] ^ ... ^ arr[i]`.

Note that `^` denotes the **bitwise-xor** operation.

It can be proven that the answer is **unique**.
```
Example 1:

**Input:** pref = [5,2,0,3,1]
**Output:** [5,7,2,3,2]
**Explanation:** From the array [5,7,2,3,2] we have the following:
- pref[0] = 5.
- pref[1] = 5 ^ 7 = 2.
- pref[2] = 5 ^ 7 ^ 2 = 0.
- pref[3] = 5 ^ 7 ^ 2 ^ 3 = 3.
- pref[4] = 5 ^ 7 ^ 2 ^ 3 ^ 2 = 1.

Example 2:

**Input:** pref = [13]
**Output:** [13]
**Explanation:** We have pref[0] = arr[0] = 13.
```
#### Main Idea
- Xor operations are commutative and associative
- `a ^ a = 0`
- Most of the xor prefix will cancel out

#### Approach
We know `pref[i] = ans[0] ^ ans[1] ^ ... ^ ans[i]` and `pref[i+1] = ans[0] ^ ans[1] ^ ... ^ ans[i] ^ ans[i+1]`.

#### Edge Cases

#### Solution
```python 

```

#### Time Complexity
`O()`

#### Space Complexity
`O()`

