---
tags:
  - system_design
---

# Failure Models
Failure models are a good way of determining the "badness" of a given failure, and are arranged in a spectrum, ranging from how easy a given failure is to "deal with."

![[FailureModel.png]]

### Fail-stop
- A node in the distributed system halts permanently  
- Other nodes in the system are still able to communicate with the failed node
- Simplest fix to make

### Crash
- Silent failure
- Node fails and other nodes are not able to communicate with the failed node

### Omission Failure
- Node fails to send or receive messages
- If the node fails to respond to the incoming request, that is known as a **send omission failure**
- If the node fails to receive the request and can't acknowledge it, then it's a **receive omission failure**

### Temporal Failures
- The node generates the correct result, however fails to produce the result in a timely enough manner to be useful

### Byzantine Failures
- The node exhibits random behavior, transmits arbitrary messages, produces wrong results, stops tasks midway
- These are the most difficult issues to diagnose