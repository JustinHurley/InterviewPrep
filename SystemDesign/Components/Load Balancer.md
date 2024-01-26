---
tags:
  - system_design
  - component
---
# Load Balancer
A lot of requests can come in at the same time and to ensure that a system can scale horizontally and vertically, we need load balancers to manage incoming traffic and evenly or appropriately distribute it.

**Improves**
- Scalability - can divert traffic to new servers
- Availability - can divert traffic away from crashed servers
- Performance - can divert traffic to lower loaded servers for better performance 

### Services Offered 
**Health Checking**: LBs use "heartbeat protocol" to monitor the health and reliability of servers

**TLS Termination**: LBs reduce the burden on end-servers by handling TLS termination with the client.

**Predictive Analytics**: LBs can predict traffic patterns through analytics performed over traffic passing through them or using statistics of traffic obtained over time.

**Reduced Human Intervention**: Because of LB automation, reduced system administration efforts are required in handling failures.

**Service Discovery**: An advantage of LBs is that the clients’ requests are forwarded to appropriate hosting servers by inquiring about the service registry.

**Security**: LBs may also improve security by mitigating attacks like denial-of-service (DoS) at different layers of the OSI model (layers 3, 4, and 7).