**One Shot by Kunal Kushwaha**

Network -> Computers connected together

World Wide Web -> Collection of Web pages and other resources

Protocols -> Rules followed while communicating on the network
Internet Society handles all the things (features of the internet)

### Terms
Client(e.g. your computer) Sends request to Server(e.g. google)
Server sends response to Client

#### Protocols : 
Rules defined by Internet society for transferring data over the networks
- TCP : Transmission Control Protocol
	- Making sure the accurate data reaches its destination and is not corrupted on the way.
- UDP : User Datagram Protocol
	- If you dont care 100% data reaches its destination (e.g. video calls)
- HTTP : Hyper Text Transfer Protocol
	- Used by WWW, defines the format of the data that is transferred between clients and servers.


How data is Transferred :
- While sharing data over the internet, you don't share the entire data in one go, it is shared in chunks 
- Like while loading a movie, watching a movie online, etc.
- Data is provided in different packets 

#### IP Address
? When you types google.com , how does it know which server to connect to ? 
- Servers are identified with their IP(Internet Protocol) address
google.com -> x.x.x.x (ip address format)
every x can have a value between 0-255

- some IP addresses are reserved, etc.

###### Layers : 
Internet
ISP(Internet service provider)
Modem/Router -> this will have a global IP address
d1 , d2, d3 -> all devices connected to the modem (will be assigned a local IP address each) 
	how ? DHCP -> Dynamic Host Configuration Protocol 

- Router will connect with the Internet and it will decide which device requested for the resource, it will decide that using NAT (Network Access Translator)

Now we know the device, how do we know the application requesting the resource ?
- Using PORT Numbers
- Ports are 16 bit numbers (0-65,536)

IP address and port number together work as a complete address for requesting and accepting response and overall connection.

- All HTTP Stuff happen on port 80
- Mongo DB runs on port 2707 (doubt)
- 0-1023 are reserved ports 
- 1024-49152 are registered for specific applications (mongodb, sql, etc)
- remaining ones can be used by us

Internet Speed : 
mbps -> mega bits per second (1000000 bits/second)
gbps
kbps


### How computers are connected ?
Physically: Optical fiber cables, coaxial cables...
Wirelessly : Wifi, Radio channels, Bluetooth, 3g, lte, etc.


### High school Networking refresh
LAN : Local Area Network (small area)
MAN : Metropolitan Area Network (across a city)
WAN : Wide Area Network (across countries)
	- SONET -> Synchronous Optical Networking
	- Frame Relay -> Connecting local area network to wider area 

Internet is a collection of all these 3 

**Modem** -> Convert digital signal to analog signal, so that it can be transferred over the network and vice versa.
**Router** ->  Router routes the data packets based on their IP addresses.

##### TOPOLOGY : 
The physical and logical arrangement of devices and the connections between them.
1. Bus Topology
2. Ring Topology
3. Star Topology
4. Tree (star connected like bus)
5. Mesh Topology 

![[network-topology-types-1024x536.jpeg]]


# Structure of Network

Application Layer : From which you are interacting sending and receiving at the user end
the rest part is part of networking


### OSI Model #imp
Stands for **Open Systems Interconnection** Model
A standard way how two or more computers communicate with each other.
- Transportation Model

7 Layers in OSI Model : 
1. Application Layer
2. Presentation Layer
3. Session Layer
4. Transport Layer
5. Network Layer
6. DataLink Layer
7. Physical Layer

Basic Overview of all the layers:
#### Application Layer 
- Implemented in Software
- Users interact with this layer
- Browsers, apps, etc.
- On the devices 
- Application Layer has some protocols like TCP/IP, etc.

#### Presentation Layer
- Convert the characters and symbols and everything into machine understandable binary format (Translation)
- Encoding
- Encryption
- Compression
- Provides Abstraction
- SSL Protocol is used (Secret Sockets Layer)

#### Session Layer
- Setting up and managing the connections (session)
- Helps in sending and receiving of data followed by termination of the connected sessions.
- Authentication and Authorization 

#### Transport Layer
- Transporting to the target based on protocols 
- **UDP and TCP** : protocols
- works in 3 parts : Segmentation (breaking the data into chunks), flow control (how much data to transfer), error control 

#### Network Layer
- Works for transferring data signals from one computer to another, when connection involves different networks.
- Router work involved here 
- Logical Addressing is handled here : IP 
- Transport the data 
- Load Balancing 

> [!note]
> The network layer focuses on host-to-host communication, handling routing and logical addressing (like IP addresses) to get data packets to the correct destination computer across potentially multiple networks. The transport layer, on the other hand, is responsible for process-to-process communication, ensuring reliable data delivery between applications on those computers, often using protocols like TCP and UDP

#### DataLink Layer
- Ensures reliable and efficient data transfer between two directly connected nodes on a network.
- Physical Addressing (MAC address) is assigned to the data packets at the datalink layer
- Mac address is a 12 digit alphanumeric number assigned to the network interface of your computer. Bluetooth, wifi, etc. may have different mac addess


#### Physical Layer
- Hardware layer : wires, etc.
- Transmits at the lowest level i.e. bits 0s and 1s


The overall flow of data : 

![[Pasted image 20250813230810.png]]



### Another Model
TCP/IP model
- Mostly similar to OSI model, except in layers
- Also called Internet Protocol Suite

5 Layers :
1. Application Layer
2. Transport Layer
3. Network Layer
4. Datalink Layer
5. Physical Layer

- This model is Used Practically, OSI model is only conceptually or theoritically


# Application Layer in Detail
### How Applications are connected 
1. Client Server Architecture
2. Peer to Peer Architecture (P2P)
#### Client Server Architecture
In networking how applications talk to each other

Client sends request to server
Server sends response to client

- Client is the user side or consumer side
- Server is the application backend or provider side

Servers have static IP addresses
In general there are multiple servers for large scale applications
Instances are increased or decreased based on number of requests or users
The collection of servers is called a Data Center 

**ping time** : the round trip time from the machine that sent the request and until the response is received (the entire time)


#### Peer to Peer Architecture
- Various devices get connected to each other
- There is no one dedicated server or one data center
- Each machine is a client as well as a server
- De-centralized network
- Example : Bit Torrent

Note : Add notes for the Networking Devices 1.:40:00 kunal kushwaha video



### Protocols 
(In application Layer)
Web Protocols : 
TCP/IP : 
	- HTTP : Hyper Text Transfer Protocol (enables connection between diff. applications)
	- DHCP : Dynamic Host Control Protocol (Allocates IP Addresses to your devices)
	- FTP : File Transfer Protocol (Rules for file transfers over network)
	- SMTP : Simple Mail Transfer Protocol (Used to send mails)
	- POP3 and IMAC : Post Office Protocol and Internet Message Access Protocol (To receive mails)
	- SSH : Secure Sockets Layer (Security Protocol that encrypts and authenticates communication)
	- VNC : Virtual Network Computing

Telnet

UDP : User Datagram Protocol
	Stateless connection


#### How Applications talk to each Other : 
For example in whatsapp if you are "sending a message", "recording a video", etc. are all processes.
One program can have multiple processes

**Sockets** -> Sockets acts as the software interface, that allows application layer protocols to communicate with the transport layer.
**Ports** -> Let specific applications or services send and receive data over the network.
	Ephemeral Ports (which port is assigned for an application/process)


### HTTP 
- HTTP is a client server protocol 
- It tells us how we request some data from the server, and it also tells us how the server will respond.
- i.e. HTTP request and HTTP response
- It is a application layer protocol, that also requires some transport layer protocol. HTTP uses TCP inside it (in the transport layer)
- HTTP is a stateless protocol. i.e. server will not store any information about the client by default.
- It has some methods like GET, POST, PATCH, etc.
- HTTP method : tells the server what to do
-  Each request and response have various parts like url, header, body, etc.
- Status Codes : Tell you about the status of the request (200 : success, 404 : not found, etc.)
```
1xx : informational
2xx : success
3xx : redirecting
4xx : client error
5xx : server error
```

Since HTTP is stateless, how does it store login info?
Ans : **Cookies**
A cookie is a small piece of data that a website stores on a user's computer or mobile device to remember information about the user's browsing session
Cookie is a unique string, stored on the client's side (browser) 
When you login / send a request for the first time it creates a cookie.
Then for the following requests the cookie is added to the request to identify the user and their details
They have an expiration date for timeout 

Third Party Cookies :
- Set for URLs you do not visit for ads 
- They're used to track users across multiple websites, enabling personalized advertising and potentially raising privacy concerns.

#### How Email Works?
SMTP for sending emails
POP3 or IMAP for receiving/downloading emails
at the application layer

at transmission layer it uses TCP

Each Email provider has their own servers e.g. yahoo, gmail, etc.

![[Pasted image 20250815234244.png]]

### DNS
Domain Name Server
Where the domain name is stored and corresponds to the ip address
- When you type "google.com", it will use DNS to find the IP address of Google's server

Huge databases to store the domain names and their ip addresses
These databases are divided into various classes : 

ex : mail.google.com : 
- mail -> sub domain
- google -> second level domain
- com -> top level domain

top -> root dns servers (first point of contact for domain query)
ex : .io, .org, .com, etc.

second level domain
student.io, google.com, change.org

these domains especially top level are registered and managed by ICANN : International Corporation for Assigned Names and Numbers

#### What happens when you type google.com
- First it will check in the local machine for the IP address
- Your machine also stores a list of previously visited domains and their respective IP addresses for quick access
- If not on local machine, then it looks for it in local DNS server like Internet service provider (ISP) 
- If still not found, it is searched in the root server 
- If still not found it is searched in TLD : Top Level Domain 
- Both root server and TLD respond to the ISP, which forwards the data to your machine 
![[Pasted image 20250816231454.png]]
- You cannot buy a domain name, you only rent it 

# Transport Layer in Detail

The primary role of the transport layer in the OSI model is to provide reliable, end-to-end communication between devices on a network

During data sharing, the transportation of data from one computer to another computer is done by Network Layer.
Transport Layer lies inside the devices, its role is to take the information from the application to network and network to the application. 
Also connecting multiple applications and sharing data among them
- It provides an abstraction.

Transport layer has some protocols : TCP and UDP
TCP handles entire data 
UDP data packets may be lost

### Multiplexing and DeMultiplexing
How computers determine the type of data sharing and how to determine which application asked for the data
Since data is shared in bundled packets.
Transport layer of sender has a multiplexer, 
Transport layer of receiver side has a demultiplexer

A multiplexer (MUX) combines data from multiple applications (like web browsers, email, chat) into a single stream and transmission over the network.
A Demultiplexer (DEMUX) separates that single incoming stream back into the correct application processes at the receiver

Using port numbers we refer to applications 
Using sockets we connect two applications

Transport layer will attach socket port numbers, therefore it knows which application it is coming from and to which application we need to send the data.

Transport layer also takes care of Congestion Control  (Traffic) 
It uses some Congestion Control Algorithms built in TCP

##### Checksums
Sometimes data might get corrupted while sharing.

Transport Layer protocol takes care of this using Checksums

Checksum is an error detection mechanism used to verify the integrity of data during transmission.

Checksum could be said is a number. 
The checksum is generated using the data and is associated with the data.
Then using the checksum we can check if the data is complete or not. If the value is different, the data is sent again.

##### Timers
Data packets might be lost during transmission.

How to know if the data has reached its destination. 

A timer is a clock based mechanism used to detect delays, retransmit lost packets and manage time related events in transport protocols like TCP.

case 1 : data being lost
- When sender sends the packet, sender starts the timer.
- When receiver receives the packet, he responds with an acknowledgement.
- If the acknowledgement is not received before the timer expires, the sender assumes the packet was lost or corrupted and retransmits it.

case 2 : ack being lost
- sender sends a packet, receiver received it and responds with an acknowledgement. 
- However the acknowledgement does not reach the sender. 
- Therefore the sender sends the same packet again assuming the packet is not received.
- To solve this packet we use Sequence Numbers
- Unique identification value number to each packet.

## Protocols
A protocol in networking is a set of rules and standards that define how data is transmitted, received and interpreted between devices in a network.
They ensure that computers, routers and servers can understand each other despite differences in hardware, software or architecture.

- TCP and UDP are Transport layer Protocols
- HTTP is Application Layer Protocol
- IP is Network Layer Protocol

#### UDP : 
User Datagram Protocol
- Provides fast, connectionless and lightweight way of sending data over a network.
- Used in streaming, online gaming, Voice Over IP
- Data may or may not be delivered
- Data may change on the way
- Data may not be in order or sequence
- UDP uses checksum to detect errors, but does nothing about it.

**UDP Packet  structure :**
Source Port Number : 2 bytes
Destination Por Number : 2 bytes
Length of Datagram : 2 bytes (contains length of UDP header and data)
Checksum : 2 bytes
Data : Variable size ~ (2<sup>16</sup> - 8) bytes

```
 ------------------------------------------------
| Source Port (16) | Destination Port (16)       |
 ------------------------------------------------
| Length (16)      | Checksum (16)               | // size in bits
 ------------------------------------------------
|                 Data (variable)                |
|                 ............                   |
 ------------------------------------------------

```


#### TCP
Transmission Control Protocol
shares data between network layer and transport layer
- HTTP uses TCP
- Application Layer sends lots of Raw Data
- TCP segments the data, divides the data into chunks and adds headers, etc.
- It may also collect data from network layer 
- Also provides Congestion Control
- Takes care of : when data does not arrive and also maintains the order of data using ack and sequence numbers.
- Also provides error control.
- Full Duplex (Bi directional data transfer)
- Used in emails, web browsing, etc.


How the connection Oriented(handshake) works :
3 way handshake :
- client sends a connection request to server, sends a synchronization flag SYN (a value inside the header) and a sequence number
- server responds a acknowledgement flag ACK, and a SYN for its own request to start a connection and a sequence number
- then the client again responds with another acknowledgement ACK and a sequence number
then the connection will be established. 


#### Difference between UDP and TCP : 

| TCP                                         | UDP                                             |
| ------------------------------------------- | ----------------------------------------------- |
| Connection Oriented(requires handshake)     | Connectionless(no handshake)                    |
| Reliable(acknowledgements, retransmissions) | Unreliable(no guarantee of delivery)            |
| Ensures data arrives in correct order       | No ordering, packets may arrive out of sequence |
| Slower                                      | Faster                                          |
| Higher Overhead                             | Lower Overhead                                  |
| Use : Web browsing, Email, file transfer    | Use : audio streaming, online gaming, VoIP      |


# Network Layer
Data in the transport layer, we have data in segments
In network layer, data travels in packets ✅
Data link layer has frames

Here we work with routers (This is where the data is transferred from one computer to another)
Every router is connected to multiple routers
Every router has its own network address
The packet contains the network address of the destination (and other data as well)
Using forwarding table of each router, we can find a path travelling which the data can receive the destination router
This is called **Hop by Hop forwarding**

![[Pasted image 20250819230353.png]]


The forwarding table comes inside routing table (routing table may have multiple paths) 
Forwarding table contains only one path.
Hopping does not happen on individual routers (like personal routers)
It happens on the ISPs (Internet Service Providers) 
For addressing it does not use individual addresses, but uses Blocks of addresses.

addressing :
for example in an IP address:` 192.168.2.30`
`192.168.2` is the network address of your device / subnet id
`30` is the device address / host id


Q. Who is creating these routing tables ? How does router know where to forward the data packets ?
A. Control Plane : Control Plane is used to build these routing tables. 
It is similar to a graph. Every router is a node and connections between them are edges.

Two types of routing to create tables : 
1. Static Routing (Adding addresses manually)
2. Dynamic Routing (When there is change in network, it changes automatically)

Path finding algorithms like dijkstra algorithm are used in routing.


#### Internet Protocol
Internet Protocol IP : This takes place in the network layer 
IP address uniquely identifies a client/server/node/router

IPv4 : 32 bit numbers, 4 words
IPv6 : 128 bit numbers

```
5. 6. 9. 14
00000101 . and so on binary representation
```

The concept of blocking (clustering) of addresses is called sub-netting.
When router forwards a packet, it needs to know the subnet of the destination

There are 3 types of classes : 
IP address of : 
CLASS A, CLASS B, CLASS C, (class D and E are also there)
Class of Ip address signifies a basic range

| CLASS | starting  | ending          |
| ----- | --------- | --------------- |
| A     | 0.0.0.0   | 127.255.255.255 |
| B     | 128.0.0.0 | 191.255.255.255 |
| C     | 192.0.0.0 | 223.255.255.255 |
| D     | 224.0.0.0 | 239.255.255.255 |
| E     | 240.0.0.0 | 255.255.255.255 |

Class A given to very large organizations and ISPs
Class B given to medium sized organizations like universities, corporations and regional ISPs
Class C given to small organizations, used in LANs and private networks
Class D used for multicast communication (one to many)
Class E used for research and experimental purposes

Subnet masking identifies the network portion of an IP address, while leaving the host portion available for device addressing.

Variable length subnets : You can set your custom subnet part length in a network 
ex : 12.0.0.0/31 -> first 31 bits are my subnet part

Reserved Addresses : 
127.0.0.0/8 => localhost (the first number is fixed, other 3 can vary)

##### IP Packets : 
Apart from data the header is of 20 bytes
It contains data like IP version, length, identification number, flags, protocols, checksum, TTL, source and destination ip address, etc.

TTL-> Time To Live : Maximum number of hops (routers) the packet can pass through before being discarded, preventing it from circulating endlessly in the network.

##### IPv6
- 2<sup>128</sup> unique ip addresses
con : 
- it is not backward compatible, devices which are configured on ipv4 cannot access websites or servers based on ipv6
- needs hardware upgrades and ISPs also need to make modifications

8 parts of hexadecimal values
a : a  : a : a : a : a : a : a 
each number is a 16 bit hexadecimal string

ex : 2405:201:a001:1054:5h77:49a3:4545:41b0

In IPv6 we can also have a subnet (fixed initial values)


### MiddleBoxes
Extra devices that also interact with the IP packets
some such devices are Firewall, Network Address Translator, Load Balancer, Proxies (forward/reverse)

###### Firewall : 
It provides filters, based on which the IP packets are filtered based on various rules.
Ex. : address, port numbers, flags, protocols, etc.
There are 2 types of firewalls :
1. stateless firewall : does not store a state
2. stateful firewall : stores the packets in its cache memory 

###### Network Address Translator : 
Converts private IP addresses to a public IP (and vice versa).
Allows multiple devices in a local network to share a single public IP.

###### Load Balancers : 
Distribute incoming network traffic across multiple servers to ensure no single server is overloaded and to improve reliability and performance.

###### Proxies : 
Acts as an intermediary between client and server - forward proxies hide client identity, while reverse proxies protect servers and can cache responses.


# DataLink Layer
DataLink layer is responsible for reliable transmission of data **frames** between two devices directly connected on the same physical link.

- It takes packets from the Network Layer and encapsulates them into frames.
- Handles MAC addressing (source & destination on the local network).
- Provides error detection (using CRC/checksum).
- Manages flow control and access to the physical medium (like Ethernet, Wi-Fi).
- Ensures that the data successfully travels over a single hop (link).

There might be many devices connected together in a LAN. 
At the datalink layer they communicate with each other using datalink layer address.
Every device will have a datalink layer address. 
This address is also called MAC address of your device. Uniquely identifying the device on a network.

What does a  frame contain ?
- contains the datalink layer address of sender and ip address of destination.

ARP : Address Resolution Protocol

# Physical Layer
Hardware stuff