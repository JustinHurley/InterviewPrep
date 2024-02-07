---
tags:
  - system_design
  - component
---

# Key Value Stores
**Key-value stores** are distributed hash tables (DHTs). A key is generated by the hash function and should be unique. In a key-value store, a key binds to a specific value and doesn’t assume anything about the structure of the value. A value can be a blob, image, server name, or anything the user wants to store against a unique key.
### Use Cases
- Smaller sized data that needs to be stored (KB to MB)
- Good at storing data during user sessions in a web app
- Good when low-latency is required
- Easy to scale
- Good when you want to cache data
## Design
### Functional Requirements
- `get`
- `put`
- `delete`
- **Configurable service**: Some applications might have a tendency to trade strong consistency for higher availability. We need to provide a configurable service so that different applications could use a range of consistency models. We need tight control over the trade-offs between availability, consistency, cost-effectiveness, and performance.
- **Ability to always write (when we picked “A” over “C” in the context of CAP)**: The applications should always have the ability to write into the key-value storage. If the user wants strong consistency, this requirement might not always be fulfilled due to the implications of the CAP theorem.
- **Hardware heterogeneity**: We want to add new servers with different and higher capacities, seamlessly, to our cluster without changing or upgrading existing servers. Our system should be able to accommodate and leverage different capacity servers, ensuring correct core functionality (`get` and `put` data) while balancing the workload distribution according to each server’s capacity. This calls for a peer-to-peer design with no distinguished nodes. 
	- I.e. it should work well with all the servers
### Non-Functional Requirements
- **Scalability**: Key-value stores should run on tens of thousands of servers distributed across the globe. Incremental scalability is highly desirable. We should add or remove the servers as needed with minimal to no disruption to the service availability. Moreover, our system should be able to handle an enormous number of users of the key-value store.
- **Fault tolerance**: The key-value store should operate uninterrupted despite failures in servers or their components.
### Assumptions
We’ll assume the following to keep our design simple:
- The data centers hosting the service are trusted (non-hostile).
- All the required authentication and authorization are already completed.
- User requests and responses are relayed over HTTPS.
### API Design
- `get(key)` returns the specified asset
- `put(key, data)` returns status of operation