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
Transport Layer 