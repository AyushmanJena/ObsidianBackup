SudoCode system design course 
https://www.youtube.com/playlist?list=PLTCrU9sGyburBw9wNOHebv9SjlE4Elv5a



System Design Interviews Prep : 
# Functional and Non Functional Requirements
Problem Statement : 
Design twitter, etc.

1. Point our or ask for functional and non functional requirements

**Functional Requirements :** 
- Post tweet
- Delete tweet
- Favourite Tweet
- Follow People

**Non Functional requirements:** 
- Latency of posting a tweet  < 1 sec
- System is available to handle certain number of DAU (Daily Active Users)


Also make list of functional requirements for the near future

2. System or Components of System

- APIs : basic overview
- Workers  : 
- Events
- Messaging

Break the functional requirements based on the components required. 
Distributing the functional requirements to those components to fulfil those requirements.


3. Outcomes 

Try to estimate number of users, number of transactions per day, total data storage requirements, etc.
CAPACITY ESTIMATION

- Choice of resources based on it 
- Number of Resources

Find how many requests one server can serve, how many users can it serve. So on how many servers would be required to fulfil the complete system requirements.

Which tech stack would be suitable to fulfil the requirements.
This tech instead of that, etc.

Availability or Consistency ?


# Capacity Estimation Thumb Rules 
How to solve capacity Estimation problems faster

Capacity Estimation is not necessarily mandatory but good to have planned and prepared.

Number of Transactions
Amount of Data to be stored
Availability

Find an estimation for resources, transactions, etc.

#### Rounding off
17000 connections/day
How many connections per second : 
round off 17000 to 20000 and 86400 to 100000

Sometimes rounding off to power of 2 helps as well
Ex : 68 -> 64, 250 -> 256 
helps in dividing easily

#### Metric System 
Calculate in Millions, Billions and Trillions and not thousands, lakhs, etc.

#### Memorizing Exercises : 
1 million/day = 12/sec
1 million/day = 700/min
1 million/day = 4200/hour
These are approx
