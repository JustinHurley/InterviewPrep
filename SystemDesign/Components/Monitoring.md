---
tags:
  - system_design
  - component
---
# Monitoring 
As a service becomes more and more distributed, it's important that we have ways to monitor the health of the system and it's important to be able to update and tweak aspects of the systems. Outages must be resolved as quickly as possible, as downtime can be very costly for businesses. 
# Metrics and Alerting 
If you are tracking the wrong information, you may miss outages or waste time collecting and analyzing useless data. Once an issue is detected, it's important that someone is alerted to resolve the issue.
## Metrics
Tracking metrics in-time saves us computing power and time later to collate all the datapoints and present them
#### Example Metrics
- TPS
- CPU utilization
- Average latency per transaction
- Requests per second
- Error rates
- HTTP responses
#### Metric Population
We can either push or pull the metrics (to be clear the pushing and pulling are from the perspective of the monitoring system
#### Persist the Data
Typically metrics are stored in a centralized datastore.
- In the event that the metric data being stored is too large, a [[SystemDesign/Components/Time Series Database|time series database]] might come in handy
## Alerting
Once a determined metric threshold has been reached, it will trigger an alarm which will send an alert to the necessary party.
# Monitoring System Design 
## Example Requirements
- Monitor critical local processes on a server for crashes.
- Monitor any anomalies in the use of CPU/memory/disk/network bandwidth by a process on a server.
- Monitor overall server health, such as CPU, memory, disk, network bandwidth, average load, and so on.
- Monitor hardware component faults on a server, such as memory failures, failing or slowing disk, and so on.
- Monitor the server’s ability to reach out-of-server critical services, such as network file systems and so on.
- Monitor all network switches, load balancers, and any other specialized hardware inside a data center.
- Monitor power consumption at the server, rack, and data center levels.
- Monitor any power events on the servers, racks, and data center.
- Monitor routing information and DNS for external clients.
- Monitor network links and paths’ latency inside and across the data centers.
- Monitor network status at the peering points.
- Monitor overall service health that might span multiple data centers—for example, a CDN and its performance.