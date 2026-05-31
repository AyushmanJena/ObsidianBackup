# Pub - Sub Messaging

Publisher Subscriber pattern in message Queues

```
                                  ------                  |-----Subscriber 1
--------------     -----------    |    |     ----------   |-----Subscriber 2
| Publisher  |---> |  Input  | -> |    | --> | Output |---|-----Subscriber 3
--------------     -----------    |    |     ----------   |-----Subscriber 4
                      channel     |    |
                                  ------
                                   message
                                   broker
```
Message Broker -> Can divide the messages based on topics and specify whom to send

Real World Example :
```
                                  ------                  |-----Subscriber 1
--------------     -----------    |    |     ----------   |-----Subscriber 2
| Publisher  |---> |  Input  | -> |    | --> | Output |---|-----Subscriber 3
--------------     -----------    |    |     ----------   |-----Subscriber 4
                      channel     |    |
                                  ------
                                   message
                                   broker
```
Input 
 `{order_placed, order_id, basic details}`
Broker adds more details like : time, place, etc.
Subscriber 1 -> SMS
Subscriber 2 -> Email
Subscriber 3 -> Logging
Subscriber 4 ->Order Processing


Note : In all cases or actions not all events or subs are used

Ex : 
```
Cases              Events 
--------           -------------
Order Placed  --> All
Add to Cart   --> Logging only
Subscribe to news letter --> Email only
etc.
```

The whole concept helps in scaling or decoupling of the entire system

### Factors of Pub-Sub Messaging 
1. Message Ordering -> Use Priority queue or some other algorithms inside your application
2. Message Consumption -> How msg is consumed
3. Malicious or wrong message handling
4. Handling Duplicated messages (especially payments)

### Pub-Sub Use Cases
#### 1. Asynchronous WorkFlow 
```
Order Placed -------------------> Send Order confirmation
                       |--------> Process Item Packing
                       |--------> Generate Invoice
                       |--------> Initiating Seller Workflow
```
Async workflows
All actions can be treated as different subscribers

#### 2. Decoupling 
- All user events can be queued and subscribers can pick up messages from queues and process for analytics.

#### 3. Load Balancing
- Increased load can be balanced either by increasing the number of subscribers
- And even if subscribers are limited, then too messages would be retained in the queue

#### 4. Deferred Processing
- Scheduling the message for off-peak hours if the message has less priority or more resource intensive.

#### 5. Data Streaming
- Ex : CCTVs, etc.


### Cases when not to use Pub-Sub Problem
1. If your application has smaller userbase no need to implement such complex system architecture
2. If your application requires real time systems, or instantaneous responses.



# Performance Metrics of the System
- Throughput
- Bandwidth
- Latency
- Response Time

#### Throughput 
 = (work done) / (time taken)
- In system Design -> number of api calls served per unit time (the more the better)
- Ex : Number of lanes 
#### Bandwidth
- Data getting transferred over networks
- Ex : Number of cars allowed per lane at a time
#### Response Time
- Often called Latency (but not same
- Time taken to serve API calls, 
- "Our System can serve 1000 requests per second"
- It means response time is 1 millisecond 
- or maybe 1 second (if it can serve all 1000 requests at a time)

As a system designer we have to find the sweet spot for resource usage, bandwidth, response time and throughput. 
In order to minimize the costs and maximize the performance of the systems.


### Latency vs Response Time
Latency 
- Time when your request is waiting to be handled.
- It includes physical distance the data needs to traverse over the networks and time it spent in the queue.

Response Time 
- Total time elapsed from sending the request to receiving the response
- It includes latency and also the server's processing time to generate the response.


# Performance of Components
#### 1. Application
Performance Metrics :
1. API Response Time
2. Throughput of APIs 
3. Error Occurrences
4. Bug/Defects in the code

#### 2. Databases
1. Time Taken by various database queries
2. Number of queries executed per unit time (or throughput)

#### 3. Cache 
1. Latency of writing to cache
2. Number of cache eviction and invalidation
3. Memory of cache instance

#### 4. Message Queues
1. Rate of production and Consumption
2. Fraction of stale or unprocessed messages
3. Bandwidth i.e. No. of consumers also affects bandwidth and throughput

#### 5. Workers
Workers are independent, background processes or dedicated servers to executing time consuming or resource-heavy tasks asynchronously.
1. Time taken for job completion
2. Resources used in processing 

#### 6. Server Instances
1. Memory/ RAM
2. CPU 

One might also need to revisit your code for optimizations after the initial production stage too. Similarly for databases and workers.

### Performance Metrics Tools
APM Tools
- Application Performance Management
- Some tools that manage all component performances
- It might be a SDK installed which takes all inputs and displays them in a dashboard
- You can also set alarms to notify when some resource usage exceeds limits or thresholds.
- Ex : Datadog, vividcortex, 
- Some companies also build their own tools too. 
- Some tools like Amazon AWS have their own performance dashboards


# Fault and Failure
"Fault is the cause, Failure is the effect"

- Our job is to anticipate the faults and failures and design the system in such a way that we can tolerate fault or have failsafe systems.

Ex : 
- Network faults 
- Not enough system memory
- Hardware issues
- Human errors or Software bugs

Hardware faults : To avoid hardware faults or memory faults you can have multiple instances of app server so that even if one system is down others can work

Software Bugs : To avoid software bugs we have various types of testing (yes some bugs go unnoticed) then we will have to handle them in client side

DB or Cache Faults : Database or Cache Faults can also occur, they  can be solved with multiple instances

#### Types of Faults
1. Transition Faults :
- Occurs for a very small duration
- Hard to locate
1. Permanent Faults : 
- Continues until fixed
- Easily identifiable


# Scaling
Analogy :
No. of tables = 2
No. of chairs per table = 4
Max people served = 8

Increasing no. of chairs per table -> Vertical Scaling
Increasing no. of tables -> Horizontal Scaling

Vertical Scaling -> Increasing capacity of existing resources
Horizontal Scaling -> Increasing number of resources


Proper Scaling is important so that the 
- the complexity of the system should not increase
- the performance of the system should not decrease
- with increasing users/ increased load

##### In app contexts : 
|   APP   |
one instance can server 1k requests per minute

when we increase the number of requests handled => 10k requests per minute (it is called vertical scaling)

|   APP   |          |   APP   |            |   APP   |
When we increase the number of app instances (or tier in aws) => (it is called horizontal scaling)
Net = 30k requests per minute 

Depending on cost requirements, different use cases and business needs we need to choose between horizontal scaling or vertical scaling.

The concept of scaling is not applicable only to app instances but also applicable to cache instances or database servers.

##### Building Scalable systems
In the beginning we need to take decisions so that we are able to scale them at a later stage
- Your system should be able to handle increasing load
- It should be able to grow along with requirements
- It should not increase the complexity of the system
- The performance as a whole should not take a hit.


# Database Replication

Replication -> To have a copy
In case of DBs, we will have 1 DB which will have all the data, then we will have a replica which will have copy of all the data in another DB.
The first DB is called Primary DB / Master DB
The copy/replica DB is called Secondary DB / Follower DB

In case some network/hardware issue happens and we are not able to access the primary db. In such cases having a replica helps.
- We will not lose the data 
- In certain cases the replica can take over and become master DB so as to save the system overall

Having replica helps in reducing latency
For the primary DB is in USA and replica DB is in Mumbai. 
When data is fetched in India from anywhere the Mumbai Db would respond.
Thus decreases network transfer times.


Common Practise to Follow : 
The primary DB is used for writes and updates queries.
The secondary DB is used for read queries.

This helps in increasing system scalability and application performance.


#### Replication Lag
an update comes to primary DB at t1
At same time read query comes to secondary Db at t3

Replication Lag (r)-> The time it takes to copy from Primary to Secondary DB.

if r < t3 - t1   ----> The user would see the right value
but if r > t3 - t1   ----> The user would see wrong data (inconsistent)

If the replication lag is huge, it becomes a problem. 
This problem becomes worse when there are multiple number of replicas.

#### Replicating Synchronously 
if r > t3 - t1 
Replication lag takes more time 

Our goal is to build the system in a way so that the data is consistent.
This is solved using Consistency Models / Consistency Algorithms
Ex : Read after Write consistency

##### Synchronous Replication : 
Master will write to its own, then send to all the replicas, 
then waits for all their Acknowledgements 
Then marks the write as complete.
Then only after that allows reads
In this case Replication lag is zero
This is also called Synchronous Replication

Advantages of Synchronous Replication : 
- The replication lag is 0
- The data is consistent across all databases.

Disadvantages : 
- The performance will take a hit, as every write will have to wait to get updated and acknowledgement. The latency of the whole write operation is higher.
- In case of failure, if any replica goes down and could not give ACK. 

##### Asynchronous Replication :
A new write is issued to master, it will send to all the replicas
It will not wait for ACK.
The replication is happening in the background

Advantages : 
- Fast

Disadvantages :
- If any replication fails, we might get inconsistent results
But if there are a number of replicas, we can fetch from multiple DBs to compare data


The use of Synchronous or Asynchronous depends on the business use- cases.
Synchronous : banking apps
Asynchronous : apps where some inconsistency is okay

##### Semi Synchronous Replication
As soon as a new write is issued, the primary DB will update to all values and will wait for only one replica for acknowledgement.
Then we are relying on other replicas to update in the background.
The primary DB might wait for 2/3/4 replicas as instructed. 
This number is called Quorum.

It has some positives of Synchronous and some positives of Asynchronous

You will have to choose among the trade-offs. It depends on the use-cases.


### DB Replica vs Snapshot
Snapshot -> Capturing the state of a database at a certain time
If something goes wrong, you can rollback to previous states.

DB Replica -> Helps in tolerate faults and increase application performance


### Overview of how Replication is done
In real world applications different systems implement replication in different manners

1. Periodic Replication
Primary DB has multiple DBs, 
Replication happens at scheduled times to another DB

2. Change Data Capture 
Any slight change in the master DB is reflected in other DBs.

3. Partial Replication
WE can replicate only a particular table and not the whole database


# CAP  
**Consistency - Availability - Partitioning**

Consistency : 
Having the same data across multiple services, or databases.

Availability : 
If one system fails, the other systems should be able to handle the overall workload.
With more systems, the overall availability of the system increases.
Highly Available Systems -> How much throughput these systems can sustain.

Partitioning : 
The communication channel between systems has failed.
This phenomenon is called partition. 
The overall system must be prepared for such situations and this is called Partition Tolerance.

### Consistency and Availability in Partition Tolerant Systems :
When there is a partition between the systems, but in order to stay consistent and available we have to make only one system accept the requests.
But it won't be as available as it was earlier.
If you want to have all systems and have it highly available you will have to sacrifice Consistency

# CAP Theorem

C, A and P in Distributed Systems :

Distributed System ->  A system consisting of a group of machines working in coordination so as to appear as a single coherent system to the end-user.

Consistency -> Any read that is happening after a latest write, all the nodes should return the latest value of that write.

Availability -> Every available node in the system should respond in a non error format to any read request without the guarantee of returning the latest write.

Partition Tolerance -> System will be responding to all read and write even if the communication channel (or middleware) between nodes is broken (or partitioned)

CAP Theorem is also known as Brewer's Theorem
Any Network shared system wants to have these 3 properties.
In any system having all 3 properties is nearly impossible. So we will have to sacrifice at least one of them. (based on the usecase)

Since network failure is something we cannot be controlled, therefore Partition Tolerance support becomes a mandatory property for our distributed systems.
Now we have to choose between Consistency and Availability

### Degree of Consistency and Availability
If you do not have Availability property, It does not necessarily mean that you cannot provide consistency at all. 
You can provide Consistency to some level
and same for Availability.

### Examples : 
Consistency > Availability : Booking seats on flights, banking systems, booking movies
Availability > Consistency : Social Media, Content apps

##### Tweaking Partition Tolerance : 
Having Backup Network Connections, to avoid Partitions
If one network fails over which nodes are connected, we switch to other network.
This will be costly and not particularly feasible.

