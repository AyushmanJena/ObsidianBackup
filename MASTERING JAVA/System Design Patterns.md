System Design Patterns you should know 
by Piyush Garg  : https://youtu.be/OdNpY3WQniQ?si=hmYSnJ0JnDb0RDPX

1. Microservice vs Monolith
Microservice : Each feature has its own service or application
Ex : Auth Service, Notification Service, Comment Service
All services work independently and no not affect each other
They are isolated
Can be scaled and modified individually
Issue : Need to communicate different services now and manage multiple different applications.

Monolith : Single server and the entire code is in that application
This can be scaled horizontally as a whole, but cannot scale only one service from it.
Issue in one service will affect the entire server/application

2. Database Per Service Architecture
For Example in Microservice Architecture : 
We can have a single database being accessed by all the microservices or we can have individual databases for each service.
If we have a single database, issues in one service might affect entire service.
Might have database bottleneck. This is not a true isolation.
So Database Per Service Architecture says each service should have its own database. 
Joins are application level now, so there are extra requests within applications.
Another advantage is we can have different types of databases as well :e.g. nosql for auth and postgress for comments, etc.
You can also optimize and scale each database based on requirements.


3. Circuit Breaker : Building Fault Tolerant Systems
Lets say service A makes a request from service B, and service B to respond needs to make a request from service C.
Now if service C is down, service B also cannot respond hence A also does not work.
This is called Cascade Failure. 
Despite being microservices, due to dependency multiple services are down.
To fix this we use Circuit Breaker Pattern  
Circuit Breaker Pattern 
Instead of directly connecting 2 services, we implement a proxy layer in between, A makes request to proxy AB and it makes request to service B. If proxy detects any issues with B, then it virtually disconnects the two services and does not let data flow through to avoid failure. 
The circuit breaker acts as a switch.
Optional : After some interval, it might test if the service is up or not. If the service is up then it reconnects the services.


4. Event Sourcing :
In microservices and High Throughput systems
e.g. banking, ecommerce, etc.

In database you can have a state 
e.g. Orders table : state = order placed -> order shipped -> out for deliver -> delivered
There is a state which is being changed multiple times. 
It might work on smaller systems but on larger systems like Amazon it might become a bottleneck.

Event Sourcing gives a Event Log / Event Stream which is immutable 
It creates multiple logs : 
12:00 Order Placed
12:10 Shipped
12:30 Out for Delivery

Now if user asks you can fetch the latest event
You are using events as source of truth


Also in banking they dont change the value in db
they maintain a log of balance changes and fetch the latest log when requested.



5. CQRS : Command Query Responsibility Segregation 
A different side for commands / mutations and different side for Reads
When user mutates something it goes to command side and creates an event and data is stored in normal database format as a copy in the reads side.
When user wants to read it, it fetches data from the reads side as it would from a normal db.
It works on Eventual Consistency.