---
tags:
  - system_design
---
# Reliability 
The probability that a service will perform its functions for a specified time interval. 
It is important to note that while Availability may seem similar, it is an independent metric for tracking.
A service can have 0% availability if it blocks all incoming calls, but be very reliable if it would correctly handle those calls if they were allowed to be made. 

### Mean Time Between Failures (MTBF)
$$MBTF =\frac{Total\ Elapsed\ Time\ - Sum\ of\ Downtime}{Total\ Number\ of\ Failures}$$

### Mean Time to Repair (MTTR)
$$MTTR = \frac{Total\ Maintenance\ Time}{Total\ Number\ of\ Repairs}$$

