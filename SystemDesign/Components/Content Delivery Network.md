---
tags:
  - system_design
  - component
---
# Content Delivery Network
A CDN is a group of geographically distributed proxy servers. A **proxy server** is an intermediate server between a client and the origin server. The proxy servers are placed on the network edge. As the network edge is close to the end users, the placement of proxy servers helps quickly deliver the content to the end users by reducing latency and saving bandwidth. A CDN has added intelligence on top of being a simple proxy server.
## Main Idea
- Solves latency issues caused by geographic distance
- Solution is to have servers located closer to users across the globe
- Serves two types of data **static** and **dynamic**
### When to Use
- When you need to vend data to users located far away
- When you need low latency
- When you have data-intensive applications
## Functional Requirements
- **Retrieve**: Depending upon the type of CDN models, a CDN should be able to retrieve content from the origin servers.
- **Request**: Content delivery from the proxy server is made upon the user’s request. CDN proxy servers should be able to respond to each user’s request in this regard.
- **Deliver**: In the case of the push model, the origin servers should be able to send the content to the CDN proxy servers.
- **Search**: The CDN should be able to execute a search against a user query for cached or otherwise stored content within the CDN infrastructure.
- **Update**: In most cases, content comes from the origin server, but if we run a script in a CDN, the CDN should be able to update the content within peer CDN proxy servers in a PoP.
- **Delete**: Depending upon the type of content (static or dynamic), it should be possible to delete cached entries from the CDN servers after a certain period.
## Non-Functional Requirements
- **Performance**: Minimizing latency is one of the core missions of a CDN. The proposed design should have the minimum possible latency.
- **Availability**: CDNs are expected to be available at all times because of their effectiveness. Availability includes protection against attacks like DDoS.
- **Scalability**: An increasing number of users will request content from CDNs. Our proposed CDN design should be able to scale horizontally as the requirements increase.
- **Reliability and security**: Our CDN design should ensure no single point of failure. Apart from failures, the designed CDN must reliably handle massive traffic loads. Furthermore, CDNs should provide protection to hosted content from various attacks.
## CDN Components
- **Clients**: End users use various clients, like browsers, smartphones, and other devices, to request content from the CDN.
- **Routing system**: The routing system directs clients to the nearest CDN facility. To do that effectively, this component receives input from various systems to understand where content is placed, how many requests are made for particular content, the load a particular set of servers is handling, and the URI (Uniform Resource Identifier) namespace of various contents.
- **Scrubber servers**: Scrubber servers are used to separate the good traffic from malicious traffic and protect against well-known attacks, like DDoS. Scrubber servers are generally used only when an attack is detected. In that case, the traffic is scrubbed or cleaned and then routed to the target destination.
- **Proxy servers**: The proxy or edge proxy servers serve the content from RAM to the users. Proxy servers store hot data in RAM, though they can store cold data in SSD or hard drive as well. These servers also provide accounting information and receive content from the distribution system.
- **Distribution system**: The distribution system is responsible for distributing content to all the edge proxy servers to different CDN facilities. This system uses the Internet and intelligent broadcast-like approaches to distribute content across the active edge proxy servers.
- **Origin servers**: The CDN infrastructure facilitates users with data received from the origin servers. The origin servers serve any unavailable data at the CDN to clients. Origin servers will use appropriate stores to keep content and other mapping metadata.
- **Management system**: The management systems are important in CDNs from a business and managerial aspect where resource usage and statistics are constantly observed. This component measures important metrics, like latency, downtime, packet loss, server load, and so on. For third-party CDNs, accounting information can also be used for billing purposes.
## Workflow
![[CDNWorkflow.png]]

## API Design 
#### Retrieve (proxy server to origin server)
This is done to refresh content on a proxy server from an origin server to keep the proxy server data updated
```
retrieveContent(proxyserver_id, content_type, content_version, description)
```
#### Deliver (origin server to proxy servers)
This is when the origin server responds to the proxy server request with the content asked for
```
deliverContent(origin_id, server_list, content_type, content_version, description)
```
#### Request (clients to proxy servers)
A client makes a request to their closest proxy server, the rest is abstracted through the CDN from the client's perspective.
```
requestContent(user_id, content_type, description)
```
#### Search (proxy server to peer proxy servers)
This needs to be done if the requested content isn't available on the closest proxy server
```
searchContent(proxyserver_id, content_type, description)
```
#### Update (proxy server to peer proxy servers)
There are many reasons a proxy node may need to update another proxy node, it could be sharing updated content, balancing a workload, or informing the node that a change has been made and it should re-fetch data from the origin node
```
updateContent(proxyserver_id, content_type, description)
```
## Content Caching
Identifying content to cache is important in delivering up-to-date and popular web content. To ensure timely updates, two classifications of CDNs are used to get the content from the origin servers.
### Push CDN
Content gets sent automatically to the CDN proxy servers from the origin server in the push CDN model. The content delivery to the CDN proxy servers is the content provider’s responsibility. Push CDN is appropriate for static content delivery, where the origin server decides which content to deliver to users using the CDN. The content is pushed to proxy servers in various locations according to the content’s popularity. If the content is rapidly changing, the push model might struggle to keep up and will do redundant content pushes.
- Good for static content
- Origin sends updated info whenever there's an update
### Pull CDN
A CDN pulls the unavailable data from origin servers when requested by a user. The proxy servers keep the files for a specified amount of time and then remove them from the cache if they’re no longer requested to balance capacity and cost.
- It gets stuff when it needs to
- Cache has a TTL
- Good for dynamic content
## Multi-tier CDN Design
To improve CDN distribution efficiency, often CDNs can be arranged in a tree-like structure, where the CDNs get more granular and geographically closer to the requester. 
- When a new node is added, it simply makes a request which propagates all the way up the tree to the origin node, which then vends the necessary data
- Typically, content has a "long-tail" distribution, this means there are a few items of content that are accessed very often and by many clients, while there is a lot of content that is rarely accessed. This is typically how data arranged in a graph shows itself.
![[LongTail.png|400]]
## How is a Request Properly Routed to the Closest Proxy Server?
- Distance between a client and a proxy server is determined by **network distance** which is a function of:
	- Length of the network path
	- Bandwidth of the network path 
- So the shortest network path to a client would be the proxy server with a short network path and a high-bandwidth.
- Works the same way as moving water, it's easiest to move water when we have big pipes and the destination is close to the origin
- We care about bandwidth when a proxy server is overloaded with requests and cannot maintain it's SLA, in this case the proxy servers then may then route the requests to a close node that also has high bandwidth 
### DNS Redirection
In a typical _DNS resolution_, we use a DNS system to get an IP against a human-readable name. However, the DNS can also return another URI (instead of an IP) to the client. Such a mechanism is called **DNS redirect**.
- This is done when the node that is geographically the closest may not be equipped to handle the requests
### Anycast
**Anycast** is a routing methodology in which all the edge servers located in multiple locations share the same single IP address.
### Client Multiplexing 
**Client multiplexing** involves sending a client a list of candidate servers. The client then chooses one server from the list to send the request to. This approach is inefficient because the client lacks the overall information to choose the most suitable server for their request.
- Gives options to the client for routing but doesn't actually resolve which route is best
- Doesn't necessarily ensure load balancing as the client can choose any of the servers to route the request to
### HTTP Redirection
**HTTP redirection** is the simplest of all approaches. With this scheme, the client requests content from the origin server. The origin server responds with an HTTP protocol to redirect the user via a URL of the content.
## CDN Content Consistency 
Distributed nodes solve the problem of having servers geographically close to clients, but it introduces the problem of keeping all the nodes updated and synced with the origin node content.
### Periodic Pulling
With periodic pulling, proxy servers request data from the origin server periodically. We use the **Time-to-refresh** (TTR) parameter to determine how often the proxy servers should be polling from the origin server.
### Time-to-Live (TTL)
A TTL is used to determine how long content should be cached for before deleting. Once the TTL is reached for a given piece of content, it will remove that content and re-request it from the origin server, either updating the data or keeping it the same. 
### Leases
A proxy server creates a **lease** with the origin server, in which a time period is agreed upon when the origin server will send updates to the proxy server. Once the lease is up the proxy server can remake the lease with the origin server to continue receiving updates.
## Deployment
### Questions to consider when deploying a CDN
- What are the best locations to install proxy servers?
- What's the right number of proxy servers?
### Proxy Server Placement
**On-Premises**: represents a smaller data center that could be placed near major IXPs
**Off-Premises**: represents placing CDN proxy servers in ISP’s networks
### CDN as a Service (CDNaaS)
People aren't making their own CDNs, typically they will contract with companies who can provide CDNaaS
#### Providers
- Cloudflare
- Akamai
- Fastly
### Specialized CDNs
If you can't adopt a CDNaaS solution due to specific needs, you'll need to make your own.
- One reason might be because your company or service needs to vend a lot of content, in which case the up-front cost of building CDN infra can be justified
- Netflix made their own CDNs since they required special work to ensure that they could properly scale with more users and increased streaming demands
## CDN Evaluation
### Performance
CDNs entire purpose is to improve performance and reduce latency. This is done via several methods:
- Content is served from the RAM
- CDN proxy servers are placed physically close to clients
- Routing is done to ensure requests go to the best proxy server to handle a request (which may not be the closest server physically)
- Long-tail content is stored in nonvolatile storage (SSD or HD) ![[LongTail.png|400]]
- Multi-CDN networks allow for a graph of CDNs to more effectively route and cache content 
### Availability 
CDN availability is ensured through how nodes are able to cache origin data, and in the case of a failure nodes can be rerouted to the next closest CDN. 
### Scalability 
- CDNs solve the geographic scalability problem
- Add more CDNs to scale horizontally
- More layers can be added to deal with the limitations of an individual proxy server
### Reliability and Security
CDNs ensure the workload is distributed across nodes, so there is no single point of failure, even if the origin node fails.
Security is improved as DDoS attacks can be scrubbed and stopped at the proxy-server level instead of the main origin point of failure.




