---
tags: [heap, design, set, array]
---
### 355. Design Twitter

Link: [here](https://leetcode.com/problems/design-twitter/description/)

#### Problem
Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and is able to see the `10` most recent tweets in the user's news feed.
Implement the `Twitter` class:
- `Twitter()` Initializes your twitter object.
- `void postTweet(int userId, int tweetId)` Composes a new tweet with ID `tweetId` by the user `userId`. Each call to this function will be made with a unique `tweetId`.
- `List<Integer> getNewsFeed(int userId)` Retrieves the `10` most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be **ordered from most recent to least recent**.
- `void follow(int followerId, int followeeId)` The user with ID `followerId` started following the user with ID `followeeId`.
- `void unfollow(int followerId, int followeeId)` The user with ID `followerId` started unfollowing the user with ID `followeeId`.

#### Main Idea
The main idea is knowing the best data structures to use: a dictionary of lists for tweets, and a dictionary of sets for following. The other main idea is implementing a heap to keep track of which ones should be shown.

#### Approach
So you create the `init` object parameters, and then implement the follow and unfollow commands which should be straightforward. Making a tweet is also simple, the only catch is that you also want to keep track of time so that we can know which tweets were the most recent. To do this we will use a time variable that we keep track of, which we make negative because Python only uses min-heaps. 
To get the news feed, we then take the most recent tweets from all users that we are following, and add to the heap. We then heapify and take a look. We know that in this heap we are guaranteed to have the most recent tweet because we took the most recent tweets from all the users being followed, and one of those must be the most recent of all.
Once that's done, we want to start looking through the heap and building the feed. We can pop the heap and then look at the most recent element. To ensure that we also consider the second-most recent tweet from the followee with the most recent tweet, we then add that tweet to the heap. We maintain that the most recent tweet that hasn't been added to the answer will always be in the min-heap.
Finally once we hit 10 tweets or run out of tweets to add to the feed, we can return the final feed.

#### Edge Cases
- User has no tweets
- User has no followers
- User unfollows someone they don't follow 
	- This scenario can be resolved with the Python `set()` method, `discard()` which removes the elt if present in the set.

#### Solution
```python 
class Twitter:

    def __init__(self):
        self.tweets = defaultdict(list)
        self.follows = defaultdict(set)
        self.t = 0
    

    def postTweet(self, userId: int, tweetId: int) -> None:
        # Get or create users tweets
        self.tweets[userId].append((self.t, tweetId))
        # Advance time (but backwards for minheap)
        self.t -= 1
        

    def getNewsFeed(self, userId: int) -> List[int]:
        # Get following list if exists
        following = self.follows[userId]
        minHeap = []
        # Now we need to go through follower tweets and add to feed
        self.follows[userId].add(userId)
        for folId in following:
            # If follower has tweets
            if folId in self.tweets:
                i = len(self.tweets[folId])-1
                # Get their most recent heap
                time, tweetId = self.tweets[folId][i]
                # Add it to the heap with next index for that user
                minHeap.append([time, tweetId, folId, i-1])
        heapq.heapify(minHeap)
        feed = []
        # While there are < 10 tweets and there are tweets to add
        while minHeap and len(feed) < 10:
            # Extract most recent tweet
            time, tweetId, followeeId, index = heapq.heappop(minHeap)
            # Add to feed
            feed.append(tweetId)
            # Look at the next tweet from that user if present
            if index >= 0:
                time, tweetId = self.tweets[followeeId][index]
                # Add their next tweet to the heap for consideration
                heapq.heappush(minHeap, [time, tweetId, followeeId, index-1]) 
        return feed

    def follow(self, followerId: int, followeeId: int) -> None:
        self.follows[followerId].add(followeeId) 
        

    def unfollow(self, followerId: int, followeeId: int) -> None:
        self.follows[followerId].discard(followeeId)
```

#### Time Complexity
`O(k)` where `k` is the number of people. This is for the `heapify` call, since building the feed only takes 10 operations.

#### Space Complexity
`O(k*n)` where `k` is the number of people and `n` is the number of tweets, since we track everyone's tweets for every user.

