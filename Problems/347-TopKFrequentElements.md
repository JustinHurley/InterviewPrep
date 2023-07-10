---
tags:
- Dictionary
- sorting
- array
- bucketsort
---

### 347. Top K Frequent Elements

Link: [here](https://leetcode.com/problems/top-k-frequent-elements/description/)

#### Problem
Given an integer array `nums` and an integer `k`, return _the_ `k` _most frequent elements_. You may return the answer in **any order**.

#### Approach
The approach has 2 parts. The first part is to count how many times each item appears, using a dictionary to do so. The second part then uses the count of the number of times each element showed up as a index that is size `n`. So for example if `3` appeared `21` times, it would go in the 20th index of the list, this is known as **bucketSort**. The important thing to mention about this bucketSort is that we use a list of lists to store the counts, that way collisions are allowed and won't cause problems (if the counts are the same and it wants the same bucket).
We can then just count backwards in the answer set until we hit `k` items, as we know the indicies that we hit first that are populated appear more frequently.
This allows us to create an `O(n)` algorithm in time and space complexity. We do one pass of `nums`, `O(n)`, create the count list, `O(n)`, then a pass through each unique element to determine where in the count list it should go `< O(n)`. Finally, we just need to read from the list backwards and grab each element until we hit k elements. This is also an `O(n)` operation as we iterate through `n` items in the list of lists, and each element only appears once at most in the list, so we check `2n` items which is `O(n)`. 

#### Solution
```python 
class Solution:
	def topKFrequent(self, nums: List[int], k: int) -> List[int]:
		n = len(nums)
		# Dict to count vals
		counts = {}
		for num in nums: # O(n)
			# Adds elts to dict and defaults to adding 1 to 0 if not in dict yet
			counts[num] = counts.get(num, 0) + 1
			
		# List to keep each count, using index to store
		countList = [[] for i in range(n)] # O(n)

		# Iterate through the keySet and add to each count index
		for key in counts.keys(): # O(n)
			countList[counts[key]-1].append(key)
		# Iterate through the count list and populate the ans
		ans = []
		addedCount = 0
		for i in range(n-1,-1,-1): # O(n)
			# If count index is not empty
			if len(countList[i]) != 0:
				# Iterate through items in list
				for count in countList[i]: # O(n) but not really since counting each array will cost n, and there are n items in the array so really O(n+n)
					# Add item to ans
					ans.append(count)
					# Keep track of added items
					addedCount += 1
					# If we added k then we're done
					if addedCount == k:
						return ans
		return ans
```
