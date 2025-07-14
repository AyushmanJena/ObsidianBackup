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

41:32