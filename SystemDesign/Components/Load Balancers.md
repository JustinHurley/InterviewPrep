---
tags:
  - system_design
  - component
aliases:
  - LB
  - load balancer
  - Load Balancer
---
# Load Balancer
A lot of requests can come in at the same time and to ensure that a system can scale horizontally and vertically, we need load balancers to manage incoming traffic and evenly or appropriately distribute it. 
Load balancers come in 2 flavors: **global** or **local** and 2 types: **layer 4** and **layer 7**

**Improves**
- Scalability - can divert traffic to new servers, and is especially good at handling horizontal scaling
- Availability - can divert traffic away from crashed servers
- Performance - can divert traffic to lower loaded servers for better performance 

### Global Server Load Balancing 
GLSB involves the distribution of traffic load across geographic regions. For example a GSLB could be used to forward an incoming request to a datacenter that is closer to the original request, or could divert traffic if there is a region-specific failure. 

### Local Load Balancers
These are the load balancers that exist at a given datacenter, and are used to divvy up incoming requests to the right server for the job

### Services Offered 
**Health Checking**: LBs use "heartbeat protocol" to monitor the health and reliability of servers

**TLS Termination**: LBs reduce the burden on end-servers by handling TLS termination with the client.

**Predictive Analytics**: LBs can predict traffic patterns through analytics performed over traffic passing through them or using statistics of traffic obtained over time.

**Reduced Human Intervention**: Because of LB automation, reduced system administration efforts are required in handling failures.

**Service Discovery**: An advantage of LBs is that the clients’ requests are forwarded to appropriate hosting servers by inquiring about the service registry.

**Security**: LBs may also improve security by mitigating attacks like denial-of-service (DoS) at different layers of the OSI model (layers 3, 4, and 7).

### Load Balancer Algorithms 
- **Round-robin scheduling**: In this algorithm, each request is forwarded to a server in the pool in a repeating sequential manner.
- **Weighted round-robin**: If some servers have a higher capability of serving clients’ requests, then it’s preferred to use a weighted round-robin algorithm. In a weighted round-robin algorithm, each node is assigned a weight. LBs forward clients’ requests according to the weight of the node. The higher the weight, the higher the number of assignments.
- **Least connections**: In certain cases, even if all the servers have the same capacity to serve clients, uneven load on certain servers is still a possibility. For example, some clients may have a request that requires longer to serve. Or some clients may have subsequent requests on the same connection. In that case, we can use algorithms like least connections where newer arriving requests are assigned to servers with fewer existing connections. LBs keep a state of the number and mapping of existing connections in such a scenario. We’ll discuss more about state maintenance later in the lesson.
- **Least response time**: In performance-sensitive services, algorithms such as least response time are required. This algorithm ensures that the server with the least response time is requested to serve the clients.
- **IP hash**: Some applications provide a different level of service to users based on their IP addresses. In that case, hashing the IP address is performed to assign users’ requests to servers.
- **URL hash**: It may be possible that some services within the application are provided by specific servers only. In that case, a client requesting service from a URL is assigned to a certain cluster or set of servers. The URL hashing algorithm is used in those scenarios.

#### Static vs. Dynamic Load Balancer 
- Static algorithms don't take server state into consideration when determining how to balance the load, as opposed to dynamic algorithms that use server health, active requests, and other options to make the best choice
- In practice, choose a dynamic load balancer 

### Stateful vs. Stateless LBs
As the name implies **stateful** LBs use a data structure to map clients to servers. Because of this it does make it harder to scale the LBs since now LBs will scale linearly with size as clients increase. 
**Stateless** is faster and as implied, does not keep that state mapping of clients to servers, and it instead uses a hash function to determine where the client's traffic should be routed to. This makes it faster and uses less space, however it is less scalable to new servers, as the 

### Load Balancer Types
Layer 4 is faster but layer 7 allows for better load balancing as there is more information available 
#### Layer 4 LB
Layer 4 refers to the load balancing performed on the basis of transport protocols like TCP and UDP. These types of LBs maintain connection/session with the clients and ensure that the same (TCP/UDP) communication ends up being forwarded to the same back-end server. Even though TLS termination is performed at layer 7 LBs, some layer 4 LBs also support it.

#### Layer 7 LB
Layer 7 load balancers are based on the data of application layer protocols. It’s possible to make application-aware forwarding decisions based on HTTP headers, URLs, cookies, and other application-specific data—for example, user ID. Apart from performing TLS termination, these LBs can take responsibilities like rate limiting users, HTTP routing, and header rewriting.

### LB Deployment
Typically LBs are deployed at multiple OSI layers of a service, and will have many redundancies. 
#### Tier-0 and Tier-1 LBs
If DNS can be considered as the tier-0 load balancer, equal cost multipath (ECMP) routers are the tier-1 LBs. From the name of ECMP, it’s evident that this layer divides incoming traffic on the basis of IP or some other algorithm like round-robin or weighted round-robin. Tier-1 LBs will balance the load across different paths to higher tiers of load balancers.
ECMP routers play a vital role in the horizontal scalability of the higher-tier LBs.
#### Tier-2 LBs
Include Layer 4 LBs, typically used to ensure all packets from the same connection are sent to the same Tier-3 LB. Will often use stateless hashing to do so.
#### Tier-3 LBs
Layer 7 LBs provide services at tier 3. Since these LBs are in direct contact with the back-end servers, they perform health monitoring of servers at HTTP level. This tier enables scalability by evenly distributing requests among the set of healthy back-end servers and provides high availability by monitoring the health of servers directly. This tier also reduces the burden on end-servers by handling low-level details like TCP-congestion control protocols, the discovery of Path MTU (maximum transmission unit), the difference in application protocol between client and back-end servers, and so on. The idea is to leave the computation and data serving to the application servers and effectively utilize load balancing commodity machines for trivial tasks. In some cases, layer 7 LBs are at the same level as the service hosts.
