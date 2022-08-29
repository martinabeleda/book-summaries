## Chapter 1 - Scale from Zero to Millions of Users

### Databases

Relational databases include MySQL, Oracle database and PostgreSQL. They allow you to store data in tables and rows and join operations can combine data across different tables.

Non-relational databases are also called NoSQL and are grouped into four categories: key-value stores, graph databses, column stores and document stores. Examples
include Cassandra, DynamoDB, Neo4j, CouchDB and HBase. Non-relational databases could be useful if:

- You require super low latency
- You have unstructured data or don't have relational data
- You only need to seralize and deserialize data
- You need to store a massive amount of data

### Scaling

Vertical scaling is the process of adding more CPU and RAM. This is great when traffic is low with simplicity being the main advantage. The limitations are:

- Has a hard limit, it is impossible to add unlimited CPU and memory
- No failover or redundancy

Horizontal scaling is more desirable for large scale applications.

### Load Balancer

Evenly distributes incoming traffic among web servers that are defined in a load balancing set. Servers are usually protected with private IPs and the DNS resolves to the public IP
of the load balancer.

- If one server goes offline, all traffic is routed to the second server while a replacement is being brought up.
- If traffic grows rapidly, the load balancer can handle routing traffic to new servers that are added to the pool.

### Database replication

Advantages:

- Better performance: all write options happen to the master node, read operations can be distributed across the read replicas.
- Realiability: If any server is lost, you do not need to worry about data loss. (Note: if the write node is lost, there may be some missing data).
- High availability: Website remains operational if one database is offline.

### Cache

A Cache is temorary storage that stores the results of expensive operations so that requests can be served more quickly. When a request is received, the application checks to see if the
cache has an available response. If it has, it sends the data back to the client. If not, it queries the database, stores the response in the cache and sends it back to the client.
This is called a read-through cache. Here are some considerations:

- Useful when data is read frequently but modified infrequently.
- Expiration policy can help to reduce stale data in cache.
- Inconsistencies can happen if data and cache become out of sync.
- A single cache can represent a single point of failure.
- Eviction policies include least recently used, least frequently used, first in first out.

### Content Delivery Network

A CDN is a network of geogrphically dispersed servers used to deliver static content. CDNs serve content like images, videos, CSS, etc. Examples include CloudFront and Akami. CDNs serve
traffic before it reaches your application and are usually managed by third parties. The considerations include:

- Cost: charging usually happens for data transfers in and out. There are little benefits for caching infrequently used assets so you should consider moving them out.
- Set an appropriate cache expiry.
- Fallback: consider how your application copes with a CDN failure.
- Invalidation: consider how you can remove files from the cache and use versioning to serve multiple versions of an object.

### Stateless web tier

Stateful applications are where the server remebers client information from one request to the next. It requires that requests are routed to the exact same server for the same
client. It is more challenging to add or remove servers and very challenging to handle server failures.

With a stateless architecture, each request can be sent to any server. State is stored and retrieved from a shared data store (DB, cache or NoSQL). After state is moved from the server
tier, it is easy to auto-scale the servers based on load.

### Data Centers

GeoDNS allows traffic to be routed based on the location of the client. In the event of a data center outage, we direct all traffic to the healthy data centers. Complexity
is increased since we need the correct tools to handle routing and data synchronization. Testing needs to occur on each of the different locations.

### Message Queues

A durable component, stored in memory that supports asynchronous communication. It serves as a buffer and distributes asynchronous requests. The basic architecture consists of
producers/publishers that create messages and publish them to the queue for a consumer/subscriber to process. This decouples the creation of messages to their consumption which allows
each to be scaled independently.

### Database Scaling

Vertical scaling or scaling up is where more CPU/RAM/DISK is added to your existing databases. This increases the risk of single point of failures and costs more since large machines
are more expensive per unit of capacity.

Horizontal scaling adds more servers but increases complexity. One approach is to shard the databases based on some sharding key. You must ensure that the sharding key evenly distributes
data across databases. This introduces complexities around:

- Resharding data: requires movind data around if we need to add another server
- Celebrity problem: hotspots caused if popular entities are all allocated to the same shard. One solution is to allocate popular nodes to their own shard.
- Join and denormalization: join operations become difficult between shards. De-normalization allows queries to be performed across a single table.
