---
tags:
  - system_design
  - component
aliases:
  - database
  - Database
  - databases
---
# Databases
A **database** is an organized collection of data that can be managed and accessed easily. Databases are created to make it easier to store, retrieve, modify, and delete data in connection with different data-processing procedures.
Databases come in two types:
- SQL (relational database)
- NoSQL (non-relational database)
#### Benefits of using a database
- **Managing large data**: A large amount of data can be easily handled with a database, which wouldn’t be possible using other tools.
- **Retrieving accurate data (data consistency)**: Due to different constraints in databases, we can retrieve accurate data whenever we want.
- **Easy updatability**: It is quite easy to update data in databases using data manipulation language (DML).
- **Security**: Databases ensure the security of the data. A database only allows authorized users to access data.
- **Data integrity**: Databases ensure data integrity by using different constraints for data.
- **Availability**: Databases can be replicated (using [data replication](https://www.educative.io/courses/grokking-modern-system-design-interview-for-engineers-managers/data-replication)) on different servers, which can be concurrently updated. These replicas ensure availability.
- **Scalability**: Databases are divided (using [data partitioning](https://www.educative.io/courses/grokking-modern-system-design-interview-for-engineers-managers/data-partitioning)) to manage the load on a single node. This increases scalability.

### Differences Between Relational and Non-Relational
- SQL uses a set schema while NoSQL uses a dynamic schema (so no set schema)
- Relational is more consistent and structured, while non-relational is fast and loose
### When to Use?
| **Relational Database** | **Non-relational Database** |
| ---- | ---- |
| If the data to be stored is structured | If the data to be stored is unstructured |
| If ACID properties are required | If there’s a need to serialize and deserialize data |
| If the size of the data is relatively small and can fit on a node | If the size of the data to be stored is large |
## Relational Databases
Relational databases must adhere to a schema before data can be added to them, i.e. the data must have the right shape to be added to the database.
A **table** or **relation** uses **unique keys** to keep track of data in a table.
Relational databases use ACID properties to maintain data consistency.
#### Popular Database Management Systems for Relational Databases
- MySQL
- Oracle Database
- Microsoft SQL Server
- IBM DB2
- Postgres
- SQLite
### Why Use a Relational Database?
- ACID compliant 
- Can change tables, columns, table names, etc.
- Reduced redundancy - data is only stored in one place and linked with foreign keys when needed elsewhere
- Concurrency - since transactions are atomic and isolated, we can handle concurrent transactions at the same time for a database
- Easy for multiple services to integrate and use 1 DB
- Consistency is guaranteed, so data is easier to import and export in the event of a failure or to backup
### Why Not Use a Relational Database?
- Impedance Mismatch - the idea that a table-based relational DB isn't the best way to store OOP objects, since it takes effort to convert a JSON-esque object and extract all the fields so it can be stored in a table.

## Non-Relational (NoSQL) Databases 
These DBs are used when an application requires a lot of semi-structured and unstructured data, low-latency, and flexible data models.
### Why use a Non-Relational Database?
- Simple - you don't have to deal with the impedance mismatch when adding data to the store
- Great at **horizontal scaling** since databases are run on a large cluster, and copy data across multiple nodes to make access easier, they are basically already a distributed service OOB
- Availability - node replacement can be done without downtime 
- Don't need to know structure of data at the time of write or creation of the DB
- More open source options 
### Types of Non-Relational Databases
#### Key-Value Store Database
E.g. DynamoDB, Redis
As the name implies, each item is stored in a KVP, where the key is the primary key and the value is everything else.
Good for managing user-sessions, where each session can be associated with a UUID or some other signifier. 
This type of database excels when you want an easy and fast DB. If you need to do more complex querying, handle more complex data relationships, or have complicated transactions, consider using another database.
![[KeyValueDB.png|500]]
#### Document Database
E.g. MongoDB and Firestore
A **document database** is designed to store and retrieve documents in formats like XML, JSON, BSON, and so on. These documents are composed of a hierarchical tree data structure that can include maps, collections, and scalar values. Documents in this type of database may have varying structures and data. 
**Use case**: Document databases are suitable for unstructured catalog data, like JSON files or other complex structured hierarchical data. For example, in e-commerce applications, a product has thousands of attributes, which is unfeasible to store in a relational database due to its impact on the reading performance. Here comes the role of a document database, which can efficiently store each attribute in a single file for easy management and faster reading speed. Moreover, it’s also a good option for content management applications, such as blogs and video platforms. An entity required for the application is stored as a single document in such applications.
#### Graph Database
E.g. Neo4J 
**Graph databases** use the graph data structure to store data, where nodes represent entities, and edges show relationships between entities. The organization of nodes based on relationships leads to interesting patterns between the nodes. This database allows us to store the data once and then interpret it differently based on relationships. Popular graph databases include Neo4J, OrientDB, and InfiniteGraph. Graph data is kept in store files for persistent storage. Each of the files contains data for a specific part of the graph, such as nodes, links, properties, and so on.
**Use case**: Graph databases can be used in social applications and provide interesting facts and figures among different kinds of users and their activities. The focus of graph databases is to store data and pave the way to drive analyses and decisions based on relationships between entities. The nature of graph databases makes them suitable for various applications, such as data regulation and privacy, machine learning research, financial services-based applications, and many more.
#### Columnar Database
E.g. Redshift, Cassandra 
**Columnar databases** store data in columns instead of rows. They enable access to all entries in the database column quickly and efficiently.
**Use case**: Columnar databases are efficient for a large number of aggregation and data analytics queries. It drastically reduces the disk I/O requirements and the amount of data required to load from the disk. For example, in applications related to financial institutions, there’s a need to sum the financial transaction over a period of time. Columnar databases make this operation quicker by just reading the column for the amount of money, ignoring other attributes of customers. I.e. it works quickly because you can ignore all other values in that given entry.
### Why Not Use a Relational Database?
- NoSQL doesn’t follow any specific standard, like how relational databases follow relational algebra. Porting applications from one type of NoSQL database to another might be a challenge. Basically it's messier -> more problems. 
- Data might not be strongly consistent, and data integrity might be weaker
# Replication
**Replication** refers to keeping multiple copies of the data at various nodes (preferably geographically distributed) to achieve availability, scalability, and performance. In this lesson, we assume that a single node is enough to hold our entire data. We won’t use this assumption while discussing the partitioning of data in multiple nodes. Often, the concepts of replication and partitioning go together.
## Synchronous Versus Asynchronous Replication
![[ReplicationTypes.png|500]]
### Synchronous Replication
The primary node waits for secondary nodes to update their data. Once all secondary nodes report success, the primary node can report success.
- Ensures all secondary nodes are up to date with the primary node
- Having to wait for all secondary nodes causes **high latency**
### Asynchronous Replication
The primary node doesn't wait for the secondary nodes to update, it just updates itself and then returns a success to the user. 
- Doesn't have to wait for other nodes to update so **low latency**
- If the primary node fails, any data not sent to secondary nodes will be lost
## Data Replication Models
### Single Leader or Primary-Secondary Replication
In **primary-secondary replication,** data is replicated across multiple nodes. One node is designated as the primary. It’s responsible for processing any writes to data stored on the cluster. It also sends all the writes to the secondary nodes and keeps them in sync.
- Good for read-heavy DBs
- Leads to inconsistent data if done asynchronously since the primary node could fail before the secondary nodes get the data
### Multi-Leader Replication
**Multi-leader replication** is an alternative to single leader replication. There are multiple primary nodes that process the writes and send them to all other primary and secondary nodes to replicate.
- Handles issue of the primary node being lost during asynchronous replication
- Useful in services where users could input data while online and offline,
- Can result in conflicting data if writes happen to 2 primary nodes at the same time, handled with timestamps, custom logic, or just trying to not have a conflict
### Peer-to-peer or Leaderless Replication
The **peer-to-peer replication** model resolves these problems by not having a single primary node. All the nodes have equal weightage and can accept read and write requests. This replication scheme can be found in the Cassandra database.
- No primary nodes so don't need to worry about losing one
- Writing everywhere leads to inconsistency
- Conflict is dealt with using **Quorums** where the result resolves to the most most popular answer (over some preset quorum amount)
# Partitions 
At a certain point, there is too much data to keep in one place, so it needs to be **partitioned** into more manageable pieces. 
## Sharding
To divide load among multiple nodes, we need to partition the data by a phenomenon known as **partitioning** or **sharding**. In this approach, we split a large dataset into smaller chunks of data stored at different nodes on our network.
### Vertical Sharding
Vertical sharding is when you split the data by attribute. E.g. a database of `carId, color, and model` is split into 2 DBs `carId and color` and `carId and model` note that both need to keep the primary key.
Often, **vertical sharding** is used to increase the speed of data retrieval from a table consisting of columns with very wide text or a binary large object (blob). In this case, the column with large text or a blob is split into a different table.
- Use when one of the columns holds very large elements
### Horizontal Sharding
**Horizontal sharding** or partitioning is used to divide a table into multiple tables by splitting data row-wise. Each partition of the original table distributed over database servers is called a **shard**.
#### Key-Range-Based Sharding
In **key-range** based horizontal sharding, shards are made via ranges set for the ID or value of a column of data. This is what a partition key can be used for in DynamoDB. 
E.g. splitting data by customer ID 
- Can only query ranges with the partition key
- Can lead to uneven distribution if partition key is poorly chosen
#### Hash-Based Sharding
**Hash-based sharding** uses a hash-like function on an attribute, and it produces different values based on which attribute the partitioning is performed. The main concept is to use a hash function on the key to get a hash value and then mod by the number of partitions. Once we’ve found an appropriate hash function for keys, we may give each partition a range of hashes (rather than a range of keys). Any key whose hash occurs inside that range will be kept in that partition.
- Can't do range queries 
## Rebalancing Partitions 
Query load can be imbalanced across the nodes due to many reasons, including the following:
- The distribution of the data isn’t equal.
- There’s too much load on a single partition.
- There’s an increase in the query traffic, and we need to add more nodes to keep up.
We can apply the following strategies to rebalance partitions.
##### Fixed Partitions
Use a fixed number of partitions, which is good at evenly balancing the data as new nodes are added but if there are a lot of small partitions, the overhead associated with rebalancing the nodes in case of a failure could be significant. 
- E.g. Redshift
##### Dynamic Partitioning 
When a node gets too large, it splits in two.
- Can cause issues when doing read/writes while splitting 
- E.g. MongoDB
##### Partition Proportionally to Nodes
Every node has fixed partitions
- As number of nodes increase, partitions shrink in size
- When number of nodes is constant, partitions increase in size as more data is added
- E.g. Cassandra
# Database Trade-Offs
### Pros and Cons of a Centralized Database
Pros
- Data maintenance, such as updating and taking backups of a centralized database, is easy.
- Centralized databases provide stronger consistency and ACID transactions than distributed databases.
- Centralized databases provide a much simpler programming model for the end programmers as compared to distributed databases.
- It’s more efficient for businesses that have a small amount of data to store that can reside on a single node.
Cons
- A centralized database can slow down, causing high latency for end users, when the number of queries per second accessing the centralized database is approaching single-node limits. Does not scale.
- A centralized database has a single point of failure. Because of this, its probability of not being accessible is much higher.
### Pros and Cons of a Distributed Database
Pros
- It’s fast and easy to access data in a distributed database because data is retrieved from the nearest database shard or the one frequently used.
- Data with different levels of distribution transparency can be stored in separate places.
- Intensive transactions consisting of queries can be divided into multiple optimized subqueries, which can be processed in a parallel fashion.
Cons
- Sometimes, data is required from multiple sites, which takes more time than expected.
- Relations are partitioned vertically or horizontally among different nodes. Therefore, operations such as joins need to reconstruct complete relations by carefully fetching data. These operations can become much more expensive and complex.
- It’s difficult to maintain consistency of data across sites in the distributed database, and it requires extra measures.
- Updates and backups in distributed databases take time to synchronize data.
