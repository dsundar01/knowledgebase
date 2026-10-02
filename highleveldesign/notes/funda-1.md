## Funda

1. What is a Server?
   1. How to deploy an application?
      1. attach server to public ip address
      2. cloud providers
2. Latency and Throughput
   1. In the ideal case, we want to make a system whose throughput is high, and latency is low.
3. Scaling and its types
   1. example of a mobile phone, when you buy a cheap mobile with less RAM and storage then your mobile hangs by using heavy games or a lot of applications simultaneously
   2. Types of Scaling
      1. Vertical Scaling - increase the specs (RAM, Storage, CPU) of the same machine to handle more load
      2. Horizontal Scaling - add more machines and distribute the incoming load
         1. load balancer - Clients are not smart; we can’t give them 2 different IP addresses of servers and let them decide what machine to hit because they don’t know about our system. For this, we put a load balancer in between. All the clients hit the load balancer, and this load balancer is responsible for routing the traffic to the least busy server.
   3. Auto Scaling - if CPU usage of an EC2 instance goes up to a certain threshold (say 90%) then launch another instance and distribute the traffic without us manually doing this.
4. Back-of-the-envelope Estimation
   1. in the horizontal scaling, we saw that we need more servers to handle the load. In back-of-the-envelope estimation, **we estimate the number of servers,** storage, etc., needed.
5. CAP Theorem
   1. Consistency: Every read request returns the same result irrespective of whichever node we are reading from
   2. Availability is continued serving when node failure happens.
   3. Partition Tolerance is continued serving when “network failures” happen.
   - CAP theorem states that in a distributed system, you can only guarantee two out of these three properties simultaneously. It’s impossible to achieve all three.
     - CA — Possible
     - AP — Possible
     - CP — Possible
     - CAP — Impossible with network partition.
   - secure apps like banking, payments, stocks, etc, go with consistency.
   - social media etc, go with availability
6. Scaling of Database
   1. Indexing
   2. Partitioning - breaking the big table into multiple small tables in same database server. (Posgres handles queries automatically)
   3. Master Slave Architecture - after all options cannot help
      - write in the master node, and then it asynchronously (or synchronously depending upon configuration) gets replicated to all the slave nodes.
   4. Multi-master Setup - When write queries become slow or one master node cannot handle all the write requests. put two master nodes, one for North India and another for South India and periodically, they sync (or replicate) their data.
      1. the most challenging part is how you would handle conflicts, then you have to write the logic in code and it is totally depends upon the business use case.
   5. Database Sharding - different sub table into a different server.(shards.)
      1. Note: The sharding key should distribute data evenly across shards to avoid overloading a single shard.
      2. Why sharding is difficult?
         1. read : write app code to navigate
         2. write : which shard to go, write code in app
      3. Sharding Strategies
         1. Range-Based Sharding: - Data is divided into shards based on ranges of values in the sharding key.
         2. Hash-Based Sharding: A hash function is applied to the sharding key, and the result determines the shard.
         3. Geographic/Entity-Based Sharding : Data is divided based on a logical grouping, like region or department.
         4. Directory-Based Sharding: A mapping directory keeps track of which shard contains specific data.
      4. Disadvantage of sharding
         1. Difficult to implement because you have to write the logic yourself for read and write
         2. cross shard join is expensive
         3. cross shard consistency is hard
      5. Sum up of Database Scaling
         1. always and always prefer vertical scaling. It's easy
         2. read heavy traffic, do master-slave architecture.
         3. write-heavy traffic, do sharding because the entire data can’t fit in one machine. Just try to avoid cross-shard queries here.
         4. ead heavy traffic but master-slave architecture becomes slow/hard to handle then do sharding
7. SQL vs NoSQL Databases and when to use which Database
   1. SQL - predefined schema, & ACID properties, ensuring data integrity and reliability. 1. Ex: Financial transactions, account balances of banking app
      Orders, payments of e-commerce app.
   2. NoSQL - flexible schema & No Acid prioritizes scalability and performance.
      1. Ex: Posts, likes, comments, messages of social media app.
   3. Scaling in SQL vs NoSQL
      1. SQL - naturally vertical scale thing to avoid complex expensive cross joins
      2. NOSQL - designed primarily to scale horizontally/sharded to accomadate more data.
8. Microservices
   1. Break down large applications into smaller, manageable, and **independently deployable services.**
   2. Why do we break our app into microservices?
      1. scale only that service independently to handle high load for one server (feed server not payment server)
      2. Flexibility to choose tech stack
      3. Failure of one service doesn’t necessarily impact others.
   3. When we want to avoid single-point failure, then also we choose microservice.
   4. How do clients request in a microservice architecture?
      1. All are deployed on different machines. It’s very hectic to use different IP addresses (or domain names) for each microservice. So, we use API Gateway for that.
      2. API Gate Way - It will take the incoming request and map it to the correct microservice.(Rate Limiting/Caching/Security (Authentication and authorization))
9. Load Balancer Deep Dive
   1. we can’t give all the IP Addresses of the machines to the client and let the client decide which server to do the request.
   2. Load Balancer Algorithms
      1. Round Robin Algorithm - Requests are distributed sequentially to servers in a circular order.
      2. Weighted Round Robin Algorithm - Similar to Round Robin, but servers are assigned weights based on their capacity.
      3. Least Connections Algorithm - Directs traffic to the server with the fewest active connections.
      4. Hash-Based Algorithm - load balancer takes anything, such as the client’s IP, user_id, etc, as input and hash that to find the server.
10. Caching
