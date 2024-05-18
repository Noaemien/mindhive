---
Week: 9
Themes: 
aliases: 
Lecture1: true
Lecture2: true
Exercises: false
---

# Lecture 1 - DNS & Security
## Application Programming Interface
- Syscall API: Interface between application programmer and OS for doing privileged stuff
- Accessing peripheral devices
- Communicating over the internet

## Network interface
- Interface between an **end-system** and the **network**
- A piece of hardware or software that **sends and recieves packets**
- E.g. the network card of your laptop is a hardware network interface

## DNS Name
- Identifies a **network interface** = identifies an end-system
- Also called a **hostname**
	- an end-system is also called a host

## URL
- Identifies a **web resource/object**
	- e.g epfl.ch/labs/nal/publications
- Format: DNS name + file name
	- epfl.ch identifies a network interface
	- /labs/nal/publications identifies a file

## Process name / address
- Identifies a process
	- e.g epfl.ch:443
	- e.g 128.178.222.83:80
- Format: **DNS name or IP address + port number**
	- epfl.ch and 128.178.222.83 identify network interfaces
	- 443 and 80 identify web-server processes running behind the corresponding network interfaces

## Web request revisited
- You enter URL into web client
- Web client extracts DNS name
- Translates DNS name to IP address
- Forms web-server process name

### HTTP Get


### The Domain Name System (DNS)
#### Designing an application
- Design the architecture
	- how many processes, which process does what ?
- Design the communication **protocol**
	- what message sequences may be exchanged ?
- Choose the **transport**: TCP or UDP?
	- what does the app need from the transport layer

##### Designing the architecture
- **Scalability**
	- Ability to grow
	- As the system grows, it maintains its properties at a **reasonable cost**
- **Hierarchy**
	- Information organized in one or more **trees**
	- Each node **interacts** with (and keeps *state* about) its children and its parent(s)
	- Universal technique for **scalability**
		- Bottom: authoritative servers
			- responsible for bla.xxx, knows how all \*.yyy.xxx DNS names
		- Middle: top-level domain (TLD) servers
			- responsible .xxx, knows how to reach \*.xxx authoritative servers
		- Top: root servers
			- knows how to reach .\* TLD servers
		
- **Caching**
	- Universal technique for improving performance
	- Challenge: ensure fresh data
	- Solution: expiration date or max caching age + dynamic check

DNS client generates DNS queries
Local DNS server (or resolver) generates DNS responses
Hierarchy of DNS servers help local DNS servers generate DNS responses


![[Pasted image 20240422094825.png|500]]

##### Designing the communication protocol
**DNS protocol**
- Query: request for an RR
	- Resource Record (RR)
		- mapping, e.g., DNS name -> IP address
		- many types: A, AAA, CNAME, MX, SOA, ...
- Response: answer(s) to query
- A DNS client or server sends a query; a DNS server sends a response

##### Choosing the Transport: TCP or UDP
- UDP for short exchanges
	- typically between DNS client and server
	- does not make sanse to pay the performance penalty for TCP connection
- TCP for longer exchanges
	- between DNS servers

### DNS attack vectors
#### Impersonation
- If Persa responds faster than the DNS server with her own IP address, the DNS Client will send information to the wrong DNS server
#### Denial of server
- If denis sends loads of requests to root DNS server or TLD server, it can deny service to other dns clients or increase response time.

#### Trashing the cache
- Slows down DNS server response


### Security
#### Confidentiality
- When Alice sends a message to Bob, only Alice and Bob can see the contents of the message
- An eavesdropper Eve, sitting on the communication channel, cannot see the content

#### Authenticity
- Avoid Persa (Impersonation) who can send an arbitrary message using Alice's IP address
- A key is sent with the address encrypted using secret key to verify authenticity
- To avoid sending twice the amount of content we send a MAC (Message Authentication Code), which is a hash of the key and the message, instead of the key
- Use of nonce to prevent replay attacks
	- Alice appends MAC or digital signature of nonce + message
	- Bob verifies it is correct

# Lecture 2

## The Transport Layer 

### Packet structure
![[Pasted image 20240518131013.png]]

### Process-to-process communication

#### Message transmission
##### Multiplexing
- Upon receiving a new message from a process, create new packets
- identify the correct IP addresses and ports
##### Demultiplexing
- many processes running in app layer
- upon receiving a new packet from the network, identify the correct destination process
#### UDP

Each UDP socket has a unique (IP address, port #) tuple
A process may use the same UDP socket to communicate with many remote processes
#### TCP

Listening & connection sockets
Each connection socket has a unique (local IP, local port, remote IP, remote port) tuple
A process must use a different TCP connection socket per remote process



### Reliable data delivery