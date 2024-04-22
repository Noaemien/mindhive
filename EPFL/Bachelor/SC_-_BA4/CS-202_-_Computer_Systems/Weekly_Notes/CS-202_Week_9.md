---
Week: 9
Themes: 
aliases: 
Lecture1: true
Lecture2: false
Exercises: false
---

# Notes

## Lecture 1 - DNS & Security
### Application Programming Interface
- Syscall API: Interface between application programmer and OS for doing privileged stuff
- Accessing peripheral devices
- Communicating over the internet

### Network interface
- Interface between an **end-system** and the **network**
- A piece of hardware or software that **sends and recieves packets**
- E.g. the network card of your laptop is a hardware network interface

### DNS Name
- Identifies a **network interface** = identifies an end-system
- Also called a **hostname**
	- an end-system is also called a host

### URL
- Identifies a **web resource/object**
	- e.g epfl.ch/labs/nal/publications
- Format: DNS name + file name
	- epfl.ch identifies a network interface
	- /labs/nal/publications identifies a file

### Process name / address
- Identifies a process
	- e.g epfl.ch:443
	- e.g 128.178.222.83:80
- Format: **DNS name or IP address + port number**
	- epfl.ch and 128.178.222.83 identify network interfaces
	- 443 and 80 identify web-server processes running behind the corresponding network interfaces

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

(See slides 30)

### Security
#### Confidentiality
- When Alice sends a message to Bob, only Alice and Bob can see the contents of the message
- An eavesdropper Eve, sitting on the communication channel, cannot see the content


## Lecture 2

