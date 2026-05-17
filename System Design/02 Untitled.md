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
