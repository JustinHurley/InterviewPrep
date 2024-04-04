---
tags:
  - template
---
# Count Pairs

## Use Cases
- When you want to count pairs more efficiently than checking all of them
## General Idea
- Use a hashmap to count the frequency of each number
- If you see a number that has been seen before, it can form a pair with itself and with all of the previously seen 
## Code
```python
seen = {}
pairs = 0
for num in nums:
	if num in seen:
		pairs += seen[num]
	seen[num] += seen.get(num, 0) + 1
return pairs
```
## Time Complexity
$O(n)$
## Space Complexity 
$O(n)$