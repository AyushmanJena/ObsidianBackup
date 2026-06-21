# Hashing Introduction

```
data -----> | function | -----> hash
```
hash is a value, generally an integer

hash function  :


For the same input the hash function will always generate the same hash value /  output.

```
Generating hash values : 
Ankit------fn--------> 12
Neha-------fn--------> 42
Ankur------fn--------> 34
Ruchi------fn--------> 67

Storing in indices : 
hash[12] = Ankit
hash[42] = Neha
hash[34] = Ankur
hash[67] = Ruchi
```

The data is stored in an array, hence the data can be accessed in constant times.

Range of hash values can be from 0  to 2<sup>32</sup>


This would be too large. 
hence after calculating the hash we take the modulus the `2*size of array`
(twice or more based on size requirements)

```
Ankit--------- fn ------ mod 8 ---------> 4
```

This multiple inputs will have the same resultant output and in this case a list is stored in that index

```
hash[2] = [Neha, Ankur]
hash[3] = [Ruchi]
hash[4] = [Ankit, Ankit]
```

Collision : The phenomenon where you have two values in the same index is called collision


### Hashing Use cases : 
##### 1. Saving key value pairs : 
Reducing search times for caching
Ex : Redis, mem cache

The modulus vales are in such a way that the value denotes in which server the data would be stored. 
(in the above example the index denotes the server number)

Issue: 
When the load / data increases we will have to increase the number of servers.
(or no. of servers will decrease)
since data is already stored based on a particular hashfunction.
We need to redistribute the data 
the modulus value will change. increase or decrease.
All the keys would get reassigned to different databases.

To solve this : 
We use **Consistent Hashing**


# Consistent Hashing
Our goal is to reduce the number of keys that would need to be remapped when number of servers changes.

We try to map values in a circle
ex : 
```
Hash output range -> 0-100
Angles on Circle -> 0-360

Ankit,Neha, Ankur, Ruchi -------hash fn----------> 12, 42, 34, 67


```

these values would be mapped to different sectors of the circle 

1st quarter -> 12
2nd quarter -> 67
3rd quarter -> 34
4th quarter -> 42

![[assets/Pasted image 20260601220955.png]]
(How the mapping is done is not important (abstraction))

We will map both values and servers on the same circle i.e. share the same output range

For every input there will be a hash generated
For every server there will be a hash generated using server ip/ server name

![[assets/Pasted image 20260601221239.png]]

mapping of values to servers : 
1. Clockwise
2. Anticlockwise

For every value to find on which server it will be set it would follow one of the above conventions.

Clockwise -> the current value will be stored on the next appearing server
(respectively for anticlockwise)

```
42 -> S1
12 -> S1
67 -> S2
34 -> S3
```

##### Removing Servers
Let S3 is removed
```
42 -> S1
12 -> S1
67 -> S2
34 -> S1
```
Only servers between S2 and S3 will need to be remapped.

NEW PROBLEM : 
Now S1 has 3 keys, the keys are not evenly distributed
So hotspots can be created and S1 will  have a lot of load

Solution : 
This problem of uneven distribution can be solved with Replication

#### Replication with Consistent Hashing
All the servers will have multiple copies and will be hashed such that they are stored in different sectors of the circle.
![[assets/Pasted image 20260601224513.png]]

While we are mapping the keys we will do as before but to the replication servers 
After removing S3 server, we will remove all the replicas of S3 as well.
Now the number of keys remapped will be lesser and there will be no hotspots as before.
