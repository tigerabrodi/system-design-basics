# Key Characteristics of Distributed Systems

## Scalability

- Scalability is the concept of a system being able to manage growing amount of work by adding resources to the system. Ideally we want our systems to grow without the performance getting worse.

- Horizontal scaling means you add more servers into your pool of resources.

- Vertical scaling means adding more power (CPU, RAM, Storage, etc.) to an existing server.

- Horizontal scaling is almost always what we want, because we won't suffer from downtime compared to vertical scaling. If any servers are offline or crash, we can still be online due to having other servers, compared to vertical scaling where you only have one server.

- Horizontal scaling can also be called scaling out, and vertical scaling up.

- Examples of horziontal scaling are Cassandra and MongoDB. A vertical one would be MySQL, you can switch from smaller to bigger machines, but this process often involves downtime.

## Reliability

- A distributed system is realiable if it keeps delivering its services even when one or several of its software or hardware components fail.

- The probability that a system performs correctly during a specific time perod.

- If a machine fails, it should be able to be replaced and the requested task should still be able to get completed.

- This comes with a cost, since we need to eliminated every single point of failure, hence need extra machines.

## Availability

- Availability is the time a system remains operational to perform its required function in a specific perod.

- Take a plane for an example, it may fly a lot in a month with very little downtime, but during times it is in maintenance, it is not available. Now, a plane that has not been properly tested, is not reliable, and may not be able to fly in any type of weather, compared to a plane that has been tested and does not have any vulnerabilities.

- A reliable system is available, but an available system may not be reliable.

- If a system is reliable, it is available long-term. The other way around, a system may be available, but that doesn't mean it will stay like that for long.

- We can also take a software project into account. The difference between one that has been properly tested, and one that hasn't. The one that is properly tested, we can ensure every part of the software works as expected, hence if a bug was to occur, it'd likely be far from now considering the cases we have covered.

## Efficieny

- A system is efficient if it uses inputs in a proper way. Looking at the input-output ratio, if it is not good, then the system is inefficient.

## Serviceability or Manageability

- How easy it is to operate and maintain a distributed system.

- Simplicity and speed with which a system can be repaired or maintained.

- In this case testing helps a lot (the right ones, we don't want tests which break every time we change code), but also writing clean code and making sure that the architecture is good, i.e. keeping things isolate, modular and decoupled.

- It is also important to keep the test code on the same standard as the production code, making sure we delete tests we no longer need and also improve existing ones if possible.

# Load Balancing

- Very important part of any distributed system.

- Helps spread traffic across a cluster of servers to improve responsiveness and availability of the system.

- Makes sure to stop sending traffic to servers that aren't available, i.e. if a server is not able to take more requests.

- Usually sits between the server and client, and distributes the traffic across the servers using different algorithms.

- By balacing out the requests, we reduce the individual server load of each server, hence reduce the chance of each server becoming a single point of failure, therefore improving overall responsiveness and availability.

Benefits:

- Users experience a faster and responsive service. The requested task don't have to be delayed due to a server struggling, rather the load balancer passes the request to a server that is available.

- Even during a full server downtime, the load balancer will simply route around it to a healthy server. You can also manage the downtime of a server without having the whole system crashing or being unresponsive.

- Keeps the system efficient, even as it scales.

## Load Balancing Algorithms

- First enures the server they choose is responding properly to requests, and then uses pre-configured algorithm to select one from the set of healthy servers.

- Consistently checks if the backend servers are healthy, so called "health checks".

- There are various load balancing methods: Least Connection Method, Least Response Time Method, Least Bandwidth Method, Round Robin Method, Weighted Round Robin Method, IP Hash.

- The load balancer itself may become a single point of failure. We can have a cluster of load balancers, where we have an active and then passive ones. A passive one would step in whenever the active one for some reason doesn't work.

# Caching

- A cache a short term memory that has a limited amount of space.

- Helps you retrieve data you need more often. Faster than the original data source.

- Often implemented nearest to front end where they can return data quickly, instead of the data being a few levels downwards.

## Content Distribution Network (CDN)

- Useful for sites serving large amounts of static media.

- A request from client to server would first ask the CDN for the desired static media; the CDN will server that content if it has it locally available, otherwise it will query the back-end servers for the file, cache lit locally and serve it to the requesting user.

## Cache Invalidation

- It is important that we invalidate cached data that has been changed in the database.

- **Write-through cache:** Data is written into the cache and database at the same time. Querying the cached data is quick, and the data in the cache is up-to-date with the one in the database since they get written at the same time. The **downside** with this approach is that we always have to write twice, which can lead to higher latency for write operations-

- **Write-around cache:** This is similar to write through cache, but data is written directly to the database, and not the cache. The **downside** here is that recently written data when read will create a "cache miss", thus the data must be read from the database, and the latency is higher.

- **Write-back cache:** The data is written to the cache alone. The write to the database is done after specified intervals or under certain conditions. The **downside** here however is that this speed comes with the risk of data loss, in case something goes wrong, because we are only directly writing to the cache.

## Cache eviction policies

- Algorithm to choose which items to remove from the cache when it gets full to make room for new data.

# Data Partitioning

- Break up a big database into many smaller parts. Split up the database across multiple machines to improve the manageability, performance, availability, and load balancing of an application.

- Why? Because after a certain point of scaling, it is cheaper and more feasible to scale horizontally by adding more machines than to grow it vertically by adding more expensive servers.

## Partitioning Methods

Three popular schemes used by large applications.

- **Horizontal partitioning:**: Also called range based partitioning or Data Sharding. We put different rows into different tables, storing difference ranges of data in separate tables. The **downside** here is that if the value whose range is used for partitioning isn't chosen carefully, then this will lead to unbalanced servers.

- **Vertical Partitioning:** Divide our data to store tables related to a specific feature in their own server. This approach is straightforward to implement and has a low impact on the application, the only issue is it may require further partitioning if our app grows.

- **Directory Based Partitioning:** Create a lookup service which knows our current partitioning scheme and abstracts it away from the database access code. Since this approach is loosely coupled, we can perform tasks or changing our partitioning scheme without having an impact on the app. The **downside** here is that it can lead to a single point of failure.

## Partitioning Criteria

- **Hash or Key-based partitioning:**: We apply a hash function to some key attributes of the entity we are storing; that yields the partition number. No performance benefits, since data gets shuffled acroos the table space randomly. Efficiently matches queries.

- **List partitioning:** Each partition is assigned a list of values, so whenever we insert a new record, we see which partition contains our key and then store it there.

- **Round-robin partitioning:** Ensures uniform data distribution. With "n" partitions, the "i" tuple is assigned to partition.

- **Composite partitioning:** Combination of any of the above partitioning schemes to devise a new scheme.

## Common problems of data partitioning

- **Joins and Denormalization:** Often it is not feasible to perform joins that span database partitions.

- **Referential integrity:** Trying to enforce data integrity constraints such as foreign keys in a partitioned database can be extremely difficult.

- **Rebalancing:** There could be cases where we have to change our partitioning scheme. Doing this without incurring downtime is extremely difficult.

# Indexes

- When the database's performance isn't doing well, one of the first things you should turn to is database indexing.

- An index is a data structure that can be perceived as a table of contents that points us to the location where actual data lives.

- Can dramatically speed up querying for data.

- Writing to the data also means updating the index. If there is data we often write to but rarely read, then it is better not to have an index.

# Proxies

- A proxy server is a server between client and backend server.

- Usually used for filtering requests, logging requests, or sometimes transforming requests.

# Redundancy and Replication

## Redundancy

- Duplication of critical pieces of a system in order to make it more reliable, such as backup, fail-safe or making it more reliable.

- Plays a key role in removing single points of failure in the system.

## Replication

- Sharing information to ensure consistency between redundant resources.

- Data is store on multiple storage devices or the same computing task is executed many times.

# SQL vs. NoSQL

- Relational and non-relational databases.

- Relational databases are structured and have predefined schemas, while non-relational ones are unstructured and dynamic.

## SQL

- Stores data in rows and columns.

## NoSQL

Common Types:

- **Key-Value Stores:** Array of key-value pairs, where key is an attribute name which points to a value.

- **Document Databases:** Data is stored in documents which are grouped by collections. Each document can have an entirely different structure.

- **Wide-Column Databases:** We have column familiar, which are containers for rows. We don't need to know all the columns up front and each row doesn't have to be the same number of columns.

- **Graph Database:** Store data whose relations are best represented in a graph. Nodes, properties and lines. An example would be Neo4J. A use case could be when a user can have friends.

## High level differences between SQL and NoSQL

- SQL stores data in tables, with rows and columns, where each row represents an entity and each column represents a data point. We may have columns "Color", "Age" and "Name", each column will contain a value, each row will be a complete dataset containing these values.

- NoSQL databases have a different model when it comes to storing data.

- In SQL, each record is bound to a specific schema. The schema can be changed later, but that means modifying the whole codebase and offline.

- NoSQL databases' schemas are dynamic. "Columns" can be added right away, since each document can be entirely different, they don't adhere to a strict schema/model defined.

- SQL databases are vertically scalable. It is possible to scale them horizontally, but is a challenging and time-consuming process.

- NoSQL databases are horizontally scalable. We can add more servers easily to out database infrastructure, hence making it more cost-effective than vertical scaling.

- Most relational databases are ACID compliant, meaning they are more reliable. Non-relational databases on the other hand tend to sacrifice ACID compliance for performance and scalability.

## Which one to use?

- **SQL:** We need to ensure ACID compliance and the data is structure plus highly likely to change.

- **NoSQL:** Storing large sets of data that often have little structure. Making the most of cloud-based storages, since they are a cost-effective solution, but do require data to be easily spread across multiple servers to scale up. Great for rapid development since you don't have to pre-define schemas.

# CAP Theorem

- States that it is impossible for a distributed software system to simultaneously provide more than two out of three of the following guarantees: Consistency, Availability and Partition tolerance.

- **Consistency:** Most recent write is received when querying/reading, all users see the same data at the same time. Achieved by updating several nodes before allowing further reads.

- **Availability:** System continues to function even with node failures. Achieved by replicating the data across different servers.

- **Partition tolerance:** System continues to function even if the communication failes between nodes.

# Consistent Hashing

- Directly doesn't depend on the number of server since it uses a distribution scheme.

- Operates independently of the number of servers in a distributed hash table by assigning them a position on a hash ring, thus servers can scale without affecting overall system.

- When a server is removed, only keys from that server are relocated.

- When a hash table is resized, only "n/m" keys need to be remappd on average, where "n" is the number of keys and "m" is the number of servers.

- Easy to scale up and down. Allows us to distribute data across a cluster in such a way that will minimize reorganization when nodes are added or removed.

- Traditionally in hash tables, a change causes nearly all keys to be remapped, this is where consistent hashing comes into place.

- Maps a key to an integer.

1. Given a list of cache servers, hash them to integers in the range.

2. To map a key to a server:

- Hash it to a single integer.

- Move clockwise on the ring until finding the first cache it encounters.

- That cache is the one that contains the key.

# Long-Polling vs WebSockets vs Server-Sent Events

## HTTP Long-Polling

- Same as the traditional polling approach, but with expectation that the server may not respond immediately.

- If the server has no data available for the client, instead of sending an empty response, the server holds the request and waits until some data becomes available.

- Once data is available and the server has sent its response, the client immediately makes another request, this way the server always has a waiting request to be responded to.

1. Client makes an initial request and waits for response.

2. Server when data is available responds with the data to the client, or when a timeout has occurred.

3. Once the client receives the data, it then makes another request to client, this request could be make after a specific pause to allow an acceptable latency period.

4. Each Long-Poll request has a timeout. The client has to reconnect after the connection has been closed.

## Web Sockets

- A persistent connection between a client and a server that both can use to start sending data at any time.

- Client starts this via a process known as the Web Socket handshake.

- Enables real-time data transfer and communication.

## Server-Sent Events

- Client establish a persistent and long-term connection with the server, but data is sent from the server.

- The client can't send data to the server.

Useful when we need real-time traffic from the server.
