---
tags:
  - system_design
  - component
aliases:
  - UNIX
---
# UNIX Timestamps
UNIX timestamps offer a millisecond-level of granularity and can be used as a timestamp.
- Important to note that 2 events could hypothetically have the same timestamp 
## Properties
- Scalable
- Easy to implement
- Allows for multiple servers to handle concurrent requests
- If the same ID is given to 2 events that occurred at the same time, we could end up with an ID collision, although this is a rare case (unless you have a bad ID hashing function)