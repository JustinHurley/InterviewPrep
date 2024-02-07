---
tags:
  - system_design
  - component
---
# TrueTime API
To solve the causality and timestamping issue when it comes to tracking events, Google has developed their own Cloud Solution, the TrueTime API.
- Returns time as a range instead of a value, with a level of uncertainty 
- Data comes from atomic clocks at Google datacenters
- Main drawback is resolving when two events have overlapping time intervals and it is unclear which event should be going first
