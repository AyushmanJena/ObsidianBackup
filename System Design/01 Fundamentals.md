Note : Skipped Some topics from beginning


# Data and Data Flow

```
Layer                           Data Format
------------------              -----------------------------
Business Layer                  Text, Images, Videos
Application Layer               JSON, XML
Data Store (DB)                 Tables, Trees, Lists, etc
Network Layer                   Data Packets (transferred between other layers)
Hardware Layer                  O/I
```

To properly understand a system and its system design 
always understand the data flow 
- Data flow 
- Data format/type

Data Stores :
- Databases
- Queues
- Caches
- Indexes

Data Flow Methods : 
- APIs
- Messages
- Events

Most important thing is to understand which data store type and dataflow method to be used based on the data and usage requirements.
```
Example :
Database -> Username, phone number, city, address
Cache -> Request: Response pairs
Queues -> SMS request, email request
Indexes -> Most searched items 
``` 

#### Data Generation 
Based on how the data is being generated on the platform 
1. Users -> Youtube, reddit, etc
2. Internal Data -> Elytra Notes
3. Insights -> Youtube history, payment details, etc

#### Factors 
1. Type of Data
2. Volume
3. Consumption /  Retrival
4.  Security


### Types of Systems
1. Authorization System : User login, Identity management
	- Low Volume
	- High Security
2. Streaming System : Netflix, Hotstar
	- High Volume
	- High Retrieval Requests
3. Transactional Systems : eCommerce, Grocery
	- Transaction Management
	- Hight Security
4. Heavy Compute Systems : Image Recognition, ML Models
	- High Data Uploads/ Volume
	- High Computation
	- Low Retrieval



# Data Bases
Place where data is stored
Different databases depending on the property of data and volume of data, Querying requirements provide different features to store data.

Types 
1. Relational
2. Non Relational
3. File Type
4. Network  and more

Relational Databases : SQL based databases
employees  : id, name, age, etc.
dept : id, name, staff
account : id, ..

Non Relational Database :
- Key Value Stores
- Column based
- Document based
- Search Databases


ACID : Atomicity, Consistency, Isolation, Durability

Cons of Relational Databases : 
1. Updating Schema as per future requirements as data evolves is difficult (will need application separately)
2. Scaling Horizontally (Dividing the tables and putting it into two different machines is difficult)


### Non Relational Databases

1. Key Value Pair Stores
Ex : discounts, coupons, etc
- Fast implementation
- Quick Access

2. Document Based 
- When we have no fixed Schema
- Can support heavy reads and writes
Collections and documents 
Ex : MongoDb
```
[
	{
		name : "Ayush", 
		age : 12
	},
	{
		name: "Shaan",
		class : 12
	}
]
```

Each object represents a document / row

Cons : 
- You might have null or empty values
- need to handle them in the application code
- Do not provide transaction properties by default

Pros : 
- Highly Scalable
- Flexibility in Querying
- Sharding (Split large data sets into smaller more managable chunks)
- Dynamic Data Flexibility


3. Column Based Databases
- Midway between Relational and document based database
- Fixed column schema
- But do not support ACID transactions

- Used for Even data
	- data on which heavy analytics would be performed (like health data, etc.)

- Heavy writes, special reads
- Distributed

Column examples : 
users, songs, users_by_liked_songs, songs_by_liked_users

Ex : 
Cassandra, HBase, Sylla


4. Search Databases :
Tree like structure to store data and indexing like Hashing to quickly access data

- Data stored in primary databases is not the primary data stored
- The actual data will be stored in a primary database (relational or other non relational database) and the results of search queries on which frequent queries are executed will be stored in search DB

5. Other Use Cases
- Images / Videos - Cloud (S3, Buckets)
- Large Datasets
- Time Series


# Anatomy of Application and Service

```
------------       request         ------------
|          | ------------------>   |          |
|  client  |                       | server   |
|          | ------------------>   |          |
------------      response         ------------
```

For example : 
If a building is a system, the pipelines, electric cables, etc. are separate applications used to build a system

Application at client side will have different responsibility, applications at server side will have different responsibilities.
They might interact with each other

Ex : 
For a mobile app
Frontend : Java/kotlin/swift/js
Backend : Python/java/Go/PHP

Languages and Frameworks

- Tech Stack
- Responsibility

NOTE :
- Applications do not need to have the same tech stack
- It does not matter what language your frontend is written in or what language your backend applications are written in. This might change based on requirements
- The methods of communications between applications i.e. API are language independent

#### Responsibilities : 
Frontend : 
- Render UI elements
- Handle Interactions
- Collect Data
- Communicate with Backend to fetch / Store data
- Render static data or info

Backend : 
- Expose API endpoints
- House Business logic
- Handle data modelling / transforming
- Interact with data store
- Interact with other services


Elements / factors considered while designing or developing an application : 
1. Feature requirements
2. Layers
3. Tech Stack
4. Code Structure /  Design Pattern
5. Data Store interactions
6. Performance  / costing
7. Deployment technique
8. Monitoring
9. Operational excellence / Reliability 


Monolith vs Microservice Architecture 
- When one application performs all the tasks such applications are called **Monolith Architecture**
- Good for small applications only.

- The other way where multiple applications perform different tasks are called **Microservice Architecture.**


# Application Programming Interface (API)

When an application has to interact with another application or a piece of code interacts with another piece of code, it does so through APIs

When Machine 1 calls API on Machine 2, Machine 1 know what and how to call the API but it would not know how Machine 2 is doing it.

Advantages of API : 
1. Communication 
2. Abstraction
3. Platform Agnostic (independent)

Examples of APIs
1. Private APIs -> hidden APIs (payment, etc.)
2. Public APIs -> google maps, weather APIs, etc.
3. Web APIs
4. SDK / Library APIs

### API Factors : 
1. API contracts : You decide with the developer of the API, methods, endpoints, format of data, etc. between the two parties
2. Documentation : Available for public on how to use the APIs. Ex : Stripe API
3. Data Format : Request, response, header formats, etc
4. Security : If someone gets access to your APIs the might send wrong info, bring your system down, etc. Therefore rate limiting, throttling, etc are also implemented

### API Standards : 
1. RPC (Remote Procedure Call) : Focuses on executing actions or functions on another system 
2. SOAP (Simple Object Access Protocol) : A highly structured protocol, typically relying on XML for strict, secure messaging
3. REST (Representational State Transfer)  : A flexible architecture style focused on managing data as resources (e.g. via JSON) 


# Caching
Storing a copy temporarily for future use.

- You can either cache the data in client side (ex : browser cache) or on server side (ex : reverse proxy)
- Helps to decrease the times the data is retrieved from the database

Ex : 
We make a caching layer on server side 
```
------------       request         ------------
|          | ------------------>   |          |
|  client  |                       | server   | <-------> DB
|          | ------------------>   |          |
------------      response         ------------
                                         |
                                         |
                                         |
                                 --------------------         [req1 -> resp1]
	                             |   Cache Memory   |        [req2 -> resp2]
	                             --------------------        [req3 -> resp3]

```

- The cache memory will store requests and responses
- When the next request comes, the server will check for it in the cache
- If the response is there in the cache it is called **cache hit.**
- If there is no such (request-response) in the cache then it is called cache miss, in this case server has to fetch from the DB again.

#### Invalidation and Eviction
- The data kept in the cache is not stored permanently
- It is volatile, it has to be invalidated

Cache expiry time -> TTL (Time To Leave)
Time in which the data will be expired

#### Other methods to invalidate : 
Update the value in cache in two ways : 
1. Delete from cache when again request is made data is fetched from DB and new pair is added
2. The response value is updated only being fetched from DB


- Every cache solution has a limit to how many keys it can store
- When the limit is reached and a new key has to be inserted, an old key is removed. This is called **Cache Eviction**
Types : 
	1. FIFO (First In First Out)
	2. LRU (Least Recently Used)
	3. LFU (Least Frequently Used)
- Based on the usecase and frequency of accepted data, one is picked.

#### Cache Patterns
1. Cache Aside Strategy / Pattern
- The cache talks to the application and not to the DB
- Problem : When the data is updated in DB, the cache has to be invalidated. This has to be done through the app.
```
------------       request         ------------
|          | ------------------>   |          |
|  client  |                       | server / | <-------> DB
|          | ------------------>   |   app    |
------------      response         ------------
                                         |
                                         |
                                 -------------------- 
	                             |   Cache Memory   | 
	                             --------------------  
```
Advantage : If the cache fails, the system as a whole would not completely fail

2. Read through strategy / patterns 
- The cache sits inbetween the application and the database
- The application talks to the cache and never to the DB
```
------------    request      ------------           -----------
|          | ------------>   |          |           |         |
|  client  |                 | server / | <-------> |  cache  |<------> DB
|          | ------------>   |   app    |           |         |
------------   response      ------------           -----------
```
Used for read heavy systems like news feeds, etc.
Disadvantage : When the first request are called it always results in cache miss
This can be solved by pre loading the cache

3. Write Around Strategy / Pattern
```
------------    request      ------------           -----------
|          | ------------>   |          |   READ     |         |
|  client  |                 | server / | <-------> |  cache  |<------> DB
|          | ------------>   |   app    |           |         |          |
------------   response      ------------           -----------          |
                                  |                 WRITE                |
                                  |------------>--------------->---------|
```
- The app/server directly interacts with the DB for write operations, but for read operations it reads from the cache.
- This strategy is useful when the system is Write Heavy


4. Write Back Strategy / Pattern
```
------------    request      ------------           -----------
|          | ------------>   |          |           |         |
|  client  |                 | server / | <-------> |  cache  |<------> DB
|          | ------------>   |   app    |           |         |
------------   response      ------------           -----------
```

All the write data coming to the app are kept in cache and are responded with 'OK' and after some time all the writes in bulk are sent to the Database
- Useful for write heavy systems
- This pattern can handle database failures for sometime, but if cache goes down all the writes are gone.


# REST APIs

#### REST 
- Representational State Transfer
- Guidelines to be followed in a client-server architecture for data to be exchanged.

##### Guidelines of REST
1. Works in client-server architecture
2. Cacheable: Mentions if data sent from server is cacheable or not
3. Layered (Abstraction) : Layer 1 only knows about layer 2 and nothing about layer 3 
4. Stateless : 
	"One server will get request from multiple clients and one server will get same request from same client multiple times"
	- Server should not know about state of client at any point of time.
5. Uniform Interface
6. Code on Demand

### Example of REST APIs
For a Book Catalogue
- Get list of all books : GET `<server domain>/mystore/books`
- Add a new book : POST `<server domain>/mystore/books` with response body
- Remove a book : DELETE `<server domain>/mystore/books`
- Update a Book : PUT `<server domain>/mystore/books`

#### State Transfer and Stateless
State Transfer -> State of the data is being transferred 
Stateless -> Server is unknown to state of the client request hence stateless (state of rest API request)

### Path and Query Parameters
URI ->  `<server domain>/mystore/books/id` 
here id is the path parameter
ex : show individual book details

URI ->  `<server domain>/mystore/books?limit=20&offset=0` 
limit and offset are query parameters 
ex: show first 20 books on this page

for next page :  `<server domain>/mystore/books?limit=20&offset=21` 

`?tag=fiction` -> show all books with tag fiction

### HTTP Responses 
1xx -> Informational
2xx -> Success
3xx -> Redirection
4xx -> Client Error
5xx -> Server Error

#### Security Authorization and Error Handling
- Rate Limiting
- Throttling


# Message Queues

### Sync vs Async Communication : 
Synchronous Communication -> Both the parties are continuously exchanging information
Asynchronous Communication -> Continuous exchange of information is not necessary

### Asynchronous Communication
ex : While waiting in a queue for your turn

```

----------------
|              |
|    online    |-----------------> QUEUE [C1, C2, ...] 
|    store     |                           |
----------------                           |
       ^                                   |
       |                                   V
       |                             ---------------------
----------------                     |    invoice         |
|              |                     |   processing       |
|    request   |                     |     component      |
|              |                     ---------------------
----------------                             |sends output
                                             |
                                             V
                                    -----------------------
                                    |      email of       |
                                    |      customer       |
                                    -----------------------

```

A queue could be a process which has a data structure in memory to store the messages, it could be on the same machine as application or other machine.
Ex : SQS, Kafka, RabbitMQ
applications meant for highly scalable applications

parts : messages, order, consumption(one by one, etc.)

queue :
```
            --------------------------
producers   |    |    |    |    |    |         consumers
----------->|    |    |    |    |    | ------------>
            |    |    | M3 | M2 | M1 |
            --------------------------       
                                                   C1 -> M1 (used) M4 (pushed)
                                                   C2 -> M2
                                                   C3 -> M3

```

### Advantages :
- At a time system won't be able to handle so many requests if queue is not implemented
- Easily scalable. ex : The number of consumers can be increased for serving requests.

### Ordering and Consumptions
Consumption (one to one)
- The message sent by the producer will be consumed only once or will be processed only once.
- After processing the message is removed from the queue

Ordering 
Ex : In chat applications, the order of messages is very important
but in the previous invoice generation, order does not matter much

Ordered Queues : Ex : FIFO (First In First Out)

Disadvantage of Ordered Queues :
- If the consumer fails to process M3 message, the queue processing will stop completely until it is processed
- Whereas in Unordered queue if one message consumption fails it is pushed into **dead-letter queue** will continue as usual
- M3 will be tried after the queue is completed.


Ordered
---------------
M1, M2, M3, M4, M5

C1 -> ~~M1~~    M5
C2 -> M2
M3 -> M3 (fails) waits until processed
C4 -> M4 


Unordered
-------------------
M1 -> ~~M1~~   M5
C2 -> ~~M2~~   M3   (retired)
C3 -> ~~M3~~   (fails)
C4 -> M4

when C3 fails it is pushed to back of the queue for retry
