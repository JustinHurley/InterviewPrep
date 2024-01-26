---
tags:
  - system_design
---
# Maintainability 
The probability that the service will restore it's functions within a specified time of fault occurrence. Basically, how quickly the service regains its normal operating conditions. We measure Maintainability with Mean Time To Repair (MTTR), which is also used in part of the definition for Reliability.

It's important not to confuse maintainability and reliability, and the big difference is that reliability also factors in the average time to failure as well as the time to repair.

Once we have built our service, how easy is it to add new features, fix bugs, update the service, and add other post-completion changes?
These concepts are already likely familiar to most Software Engineers, as these practices are used in day-to-day coding as well.

### Operability 
How easy is it to ensure "smooth" operation for a system when it is running under normal circumstances 

### Lucidity 
How simple is the code? Lucid code is easier to read, understand, and maintain

### Modifiability 
How easy is it to add new features to the system?
