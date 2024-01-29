---
tags:
  - system_design
---

# Consistency
Consistency can have many meanings in the context of distributed systems. That's why it's helpful to think of consistency ranging from **strongest consistency** to **weakest** consistency. 
In general, it refers to guarantees as to what data a replica of a node is able to provide in terms of correctness/recency.

### Eventual Consistency
- Weakest measure of consistency 
- Ensures that nodes in the system will converge on a final value at some point after write operations have stopped
- Pros
	- Very high availability 
- Cons
	- A client could call the data and get outdated information
- Real World Example: The domain name system allows users to look up a massive amount of information quickly, but may take some time to update entries
- Services: Cassandra (NoSQL)

### Causal Consistency 
- Categorizes operations into dependent and independent operations
	- Dependent operations or causally-related options have their order preserved when performing operations
- Pros
	- More consistency than eventual
- Cons
	- Still can allow for users to get stale data if non-causal and read/writes are close together 
- Example: We make a call (Call A) that sets `x` equal to 5. We then make a call (Call B) that sets `b` to `x + 5`, and a call that sets `y` equal to `b`. 
	- Call B is causally-related operation since we need to read `x` before evaluating `b` and then we can set `y` equal to `b`. 
	- Note that Call A and Call B exist fully separately, so Call B is not waiting for the write from Call A to start evaluating.
- Real World Example: A commenting system only cares about the order of comments and replies to that comment, however fully separate comments do not need to be synced

### Sequential Consistency
- Conserves the ordering specified by each client's program, however does not ensure instantaneous write operations, or for operations to be globally ordered 
- Useful when we care about consistency on a smaller level, but don't need any global order, or to immediately see write data
- Pros
	- More consistent that causal consistency
- Cons
	- Non-instant write operations
	- No global ordering on operations
- Real World Example: In a social media feed, we don't really care what orders posts appear in, however for a given user that we are following, we would expect to see their posts in chronological order

### Strict Consistency or Linearizability
- The strongest consistency model, it ensures that any read request from any node will get the most-recent value. It needs to wait for a write operation to be completed before informing clients that the data is available, updated, and readable
- Useful when it's critical that all users have access to the the same data, and writes must be visible by all users of the service
- Pros
	- Data is always up-to-date
	- No stale values 
- Cons
	- A lot of effort is required to ensure all nodes are synced 
	- Variable network delays and failures must be dealt with or the consistency model fails
	- Bad for availability
- Real World Example: Passwords, if a password is changed, that change will need to be reflected immediately to all users of the application, otherwise malicious actors could take advantage of the stale password 
