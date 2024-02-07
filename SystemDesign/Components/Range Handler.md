---
tags:
  - system_design
  - component
---
# Range Handler
A range handler is used when multiple nodes or records need to coordinate having an ID system where IDs are unique
## How it Works
- A unique ID is needed
- A request is made to the range handler to get a new range
- The range handler assigns an unclaimed range to the database
- Whenever the node or database needs to generate IDs, it can refer to the range given by the **range handler**
## Properties
- Range handler can become a single point of failure, which is why we have a rollover range handler to take over if the main one fails
- If a server dies, a lot of range is lost. This can be partially mitigated by making the ranges smaller (but not too small)