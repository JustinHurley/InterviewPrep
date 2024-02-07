---
tags:
  - system_design
  - component
aliases:
  - logical clock
---
# Logical Clocks
Storing data in timestamps can sometimes be not good enough (if multiple events are happening in the same millisecond) or a waste of space (if we aren't having many events each day and don't need `ms` granularity).
Below are two types of logical clocks
## Lamport Clock
- All nodes have a counter that starts at 0
- When a node sends an event, it increments its counter by 1 and then sends the event and the counter to the next node.
- The receiving node then looks at the counter and updates it's counter by taking the max of the counter values send
- The event is then processed by the server
- Cannot be used to infer causality at a global level
## Vector Clocks
Vector clocks work similarly to Lamport Clocks, the main difference is that instead of there being one universal counter value that all the nodes pass around, there is an array of `n` counts for each node, and when we send a request from one node to another, the vector is updated for the counts of every node, even those that might not be involved in the direct transaction occurring.
