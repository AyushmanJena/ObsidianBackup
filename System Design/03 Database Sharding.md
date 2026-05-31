# Database Sharding 
Logical and Physical Sharding, Dynamic vs Algorithmic Sharding

When the size of the data increases and increasing the storage capacity of a single database machine doesnot suffice, we need to store the data in multiple machines.
This is also called Data partitioning.

Instead of storing the entire data in one db, we use multiple dbs and store the split data.


How can we partition the data ? 
There are two ways to partition a table : 
1. Store different columns (could be multiple) in a different databases
2. Divide the rows in different databases.

Storing different columns in different DBs is called Vertical Partitioning
Storing different columns in different DBs is called Horizontal Partitioning

Horizontal Partitioning is same as SHARDING.

Sharding : Horizontal Scaling technique where you split a large dataset into smaller, manageable chunks called shards and distribute them across multiple servers.

### Logical and Physical Shards : 
The entire DB consists of 4 million rows.
They are divided into 4 parts 1 Million each.

The parts into which they are divided are called logical shards.

The logical Shards are stored in physical machines called physical shards.

Multiple logical shards can be stored in the same DB or physical shard.

```
                  |---------------|--------------- |DB 1|
				|LS 1|         |LS 2|
|  DB  |--------
                |LS 3|         |LS 4|
                  |---------------|--------------- |DB 1|
                  
DB is divided into 4 logical shards
Logical shard 1 and 2 are stored in DB 1 (Phycical Shard)
3 and 4 are stored in DB 2 
```


### Sharding strategies : 
There are two types of sharding : 
1. Algorithmic Sharding
2. Dynamic Sharding

The application, uses logic or algorithm (properties) using which it can find out which shard or which database the particular row/data has to be written to. This is called Algorithmic Sharding.

There is another service, which tells where the queries have to be routed to. This is called Dynamic Sharding module or Locator Service. The main application talks to this service. This is called Dynamic Sharding. 

Using dynamic sharding we can add or remove shards. 
Using Algorithmic Sharding number of shards remains constant.

Advantages of Sharding : 
- Not needing to store the entire dataset in one machine
- To perform a query searching through large dataset can be slower. If the sharding is done with correct approach then we can know which db has the data and makes querying faster.
- Can have different dbs in different geographical regions and have lower latency throughout.
- No single point of failure. 

Disadvantages of Sharding : 
- While choosing Sharding strategy, if done incorrectly the data would not be evenly distributed among the shards. Some shards might contain most of the data (called hotspots)
- Once you've partitioned your data and you need to comeback to non-sharded architecture. It is very complex to recover the data.
- If your queries require joins from different databases (multiple shards) then it would be taking a lot of time as well.
- All databases do not support sharding natively. You will have to implement it on your own.


# Key Based Sharding 

IMP : Shard Key != Primary Key

The Primary key can be a shard key

Shard Key -> Picking a column based on which we will be dividing the data 

```
UserId     Name      City
----------------------------
101        A         Berlin
102        B         Dubai
103        C         Delhi
104        D         Mumbai
```
Ex : here we choose UserId is the shard Key

#### Algorithmic Sharding
Using Hashing Function 
```
   userId    ----------------    Shard Value
-----------> |    Hash Fn   | ---------------->  
             ----------------
```
Based on the hash value/shard value we get which DB the data goes into.
Note : According to hashing rules, if you are using the same hash function, it will always generate the same value for the same input.

Advantages of using hash function:
- Data is evenly distributed

Disadvantage  : 
- When data increases and you have to add a new shard, your hashing function has to change. And now you will have to move the data between the shards which is nightmare. Note :Consistent Hashing solves this

How to choose a Shard Key :
- Choose a key which is static in nature.
- You can choose a combination of columns as shard key 
- They have to be unique.

# Range Based Sharding
ex : Time stamp based data :
```
uId      event              date
100       click_login       17-02-2026
101       click_logout       17-02-2026
102       click_buy         17-03-2026
104       click_login       17-03-2026
105       click_promo       17-05-2026
106       click_login       17-04-2026
```

Now we have 3 shards and the data is split based on range (here based on the date)
so 100, 101 will be stored in shard 1
102, 104 stored in shard 2
105, 106 stored in shard 3

Not necessarily date only, it could be anything like price, value, etc. and stored based on range of values

Advantages of Range Based Sharding : 
- same database structure for all shards
- since no hashing functions, so you can add more machines easily

Disadvantages of Range based sharding : 
- if one range contains more data then hotspots will be created in that particular shard. (uneven distribution of data)


# Directory Based Sharding
 Example of Dynamic Sharding

We have a lookup table where we store where the exact data is stored, other than the application and the database.

```
data table : 
uId        country/zones
200         A
201         B
202         C
203         D
```

```
lookup table 
zone        shard
A           1
B           2
C           3
D           4
```

When read is done, it goes for the lookup table and fetches where the actual data is stored. 
Picking the shard key is important so it is evenly distributed and represents a large number of rows with the same value.

Advantages : 
- If you have to add more rows, you can do it without touching the previous shards
- Removing other shards is easier, without affecting other shards

Disadvantages : 
- Due to the lookup table inclusion, the latency has increased
- If the lookup table application crashes, the whole system crashes. The lookup table becomes a single point of failure.


