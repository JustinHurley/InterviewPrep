---
tags:
  - system_design
---
# Data Center Servers
There are 3 types of serves typically used at a data center, each tuned to support specific needs of that server

## Web Server
- High processor/CPU speeds
- Low RAM
- Low HD space
- Good for processing a lot of requests
- Typically next stop after a request is handled via a [[SystemDesign/ComponentsLoad Balancer|Load Balancer]] is to a web server

## Application Server
- Medium processor/CPU speeds
- High RAM
- Medium HD space
- Typically used to handle dynamic content as opposed to just serving static web content

## Storage Server
- Medium processor/CPU speeds
- Low RAM
- High HD space
- Stores data

## Requests Estimation
With respect to estimating how many requests a typical server can handle per second (TPS), which are split into 2 groups
- CPU bound requests - handled by the CPU
- Memory bound requests - handled by RAM

## Important Server Latencies
| Component | Time (nanoseconds) |
| ---- | ---- |
| L1 cache reference | 0.9 |
| L2 cache reference | 2.8 |
| L3 cache reference | 12.9 |
| Main memory reference | 100 |
| Compress 1KB with Snzip | 3,000 (3 microseconds) |
| Read 1 MB sequentially from memory | 9,000 (9 microseconds) |
| Read 1 MB sequentially from SSD | 200,000 (200 microseconds) |
| Round trip within same datacenter | 500,000 (500 microseconds) |
| Read 1 MB sequentially from SSD with speed ~1GB/sec SSD | 1,000,000 (1 milliseconds) |
| Disk seek | 4,000,000 (4 milliseconds) |
| Read 1 MB sequentially from disk | 2,000,000 (2 milliseconds) |
| Send packet SF->NYC |  |

