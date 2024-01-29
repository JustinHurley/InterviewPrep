# CAP Theorem
Consistency, Availability, and Partition Tolerance (CAP) Theory, states that a distributed data store cannot provide more than 2 out of the 3 in CAP, and trade-offs must be made when designing a distributed data store.
In reality, networks must partition themselves to continue to scale with the system, so the real trade-off is between Consistency and Availability. The only networks that don't have to worry about partitioning tolerance are RDMBS like MySQL, since they exist as a monolithic data store. 
- A system that chooses consistency over availability will ensure that all nodes see the same data at the same time, but it might refuse to respond to a read or write operation if there's a network partition.
- A system that chooses availability over consistency will always process queries and updates but might provide an older version of the data (i.e., not the most recent one).
## Consistency
Every read receives the most recent write or an error. In other words, if a data item is updated, subsequent accesses will always see the updated value. Consistency here refers to linearizability or the idea that a system appears to perform all its operations in some sequential order.
## Availability 
Every request receives a (non-error) response – without guarantee that it contains the most recent write. Availability in the CAP context means that every request receives a response, regardless of the state of any individual node in the system.
## Partition Tolerance
The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes. In essence, the system can sustain any amount of network failure that doesn't result in a failure of the entire network.