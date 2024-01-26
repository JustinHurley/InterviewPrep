---
tags:
  - system_design
---
# Scalability
A measure of how a system responds to an increasing amount of workload without altering different parts of the system

#### Request Workload
The number of requests served by the system

#### Data/storage Workload
The amount of data stored by the system

## Dimensions
- **Size Scalability**: A system is scalable in size if we can add more users and resources to it
- **Administrative Scalability**: A system's ability to share a single distributed system with ease
- **Geographic Scalability**: How a program caters to other regions that it may not exist in, basically how good is your regionalization

### Vertical Scaling
- AKA "Scaling Up"
- It's when you add more resources to existing hardware
- For example, adding more RAM to a server or more CPUs would be vertical scaling 

### Horizontal Scaling
- AKA "Scaling Out"
- It's when you add more machines
- For example, buying more server racks at a datacenter to scale your service