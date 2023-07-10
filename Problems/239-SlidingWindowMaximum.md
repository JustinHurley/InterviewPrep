---
tags:
- sliding_window
- queue
- heap 
- deque
---

### 239. Sliding Window Maximum

Link: [here](https://leetcode.com/problems/sliding-window-maximum/description/) 

#### Problem
You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

#### Approach
So the brute force approach would be to just take the window and calculate the max in each window. This would give us a time complexity of `O((n-k)*k)` since we need to do `k` operations for each valid window to get the max. 
While this is good, we can do a better solution by using a deque to keep track of the max items seen. The general approach is as follows: iterate through the array and keep track of the items in a given window with a deque. The important part of using the deque is the fact that we need to keep track of the values of the items going in. We want to keep the leftmost value to be the max current value within the window, and update as we go along. So the general flow per iteration of an element in `nums` will go as follows:
1. Pop all values in the deque that are smaller than the current value. Since we are looking for the maximum value we don't really care about lesser values in the deque, and would like to maintain that the leftmost value remains the max.
2. Add the current `nums[r]` value to the deque. This will place it positionally relative to the rest of the values from left to right in the deque, in order of largest to smallest.
3. Check to ensure that the left-most val is within the window. By doing a comparison between the index of the left-most value in the deque, and with the current left index, we can check to see if it's in bounds or not. We only need to look at the left-most value since if there was a larger value it would be replaced the left-most one and there would be a more recent index, or the left-most one is the max value that's moving out of the window. This is why for this problem it's easier to store the index in the deque compared to the value.
4. Check to see if the window is the right size, i.e. `r + 1 >= k` to ensure that you are ready to start adding values to the answer set. Also make sure to increment `r` at all iterations and `l` when the window is the right size.
This whole approach hinges on using the deque to keep track of the max value as we iterate through the array, while also keeping values that aren't the largest but could be. The main thing to keep in mind when understanding this problem is that we only care about the max values, so if we see a new max value, all the previous values in the deque can go away since they will never be the value added to the answer array, as the new max value will be in the window longer than all the other smaller ones. This idea of keeping the max in the leftmost spot in the deque allows us to move through the array and keep the max at each step while still having room for values that could be the max in the future as we slide the window to the right, without needing to look at values we have passed already and know could never be the max.

#### Solution
```python 
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        length = len(nums)
        ans = []
        l = r = 0
        q = collections.deque()

        while r < length:
            # If there is a smaller val in the q, remove it
            while q and nums[q[-1]] < nums[r]:
                q.pop()
            # Now add our val index to the q
            q.append(r)

            # If the leftmost val is outside the window, remove it
            if l > q[0]:
                q.popleft()
            
            # If the window is the right size, add the max to the ans set
            if r + 1 >= k:
                ans.append(nums[q[0]])
                l += 1
            r += 1
        return ans
```

#### Time Complexity
The time complexity of this approach is `O(n)`, since each iteration, which we do on each element of `nums`, only does constant operations. The only potentials non-constant operation would be the while loop where we pop all elements in the deque smaller than `nums[r]`, but the deque can only have each element added at most once, so the internal while loop can only iterate over `n` items total in the entire algorithm.