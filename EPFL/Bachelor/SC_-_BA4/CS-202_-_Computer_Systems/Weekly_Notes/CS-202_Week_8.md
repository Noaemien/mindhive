---
Week: 8
Themes: 
aliases: 
Lecture1: true
Lecture2: false
Exercises: false
---

# Application Layer
## Process
Piece of code running on a end-system, belongs to application layer
Has a process name: ex 128.156.17.23 and a port number: ex 80

## Interface
Point where two systems, subjects, orgs, etc.. meet and interact
## Designing an application

### Design the architecture
#### Client server architecture
- Client generates service requests
-  Server: process that is always running, reachable at a fixed, known process name, answers service requests
-  time increases linearly with number of clients
#### Peer-to-peer architecture
- A peer may act as both server and client
	- generates service requests
	- answer (or deny) requests
- Runs on personally owned end systems
- time increases sub-linearly with the number of clients
##### How to retrieve content
- Learn metadata file ID
- Find metadata file location
- Get metadata file (from web server or peer), read data file IDS
- Find data file locations
- Get data files (from peers)

Metadata file can be stored on web server on by a peer. (ex bit-torrent metadata file is .torrent)

**Finding file location**:
- Tracker:
	- An end-system that knows the locations of the files
		- the IP addresses of the peers that store each file
- Distributed Hash Table (DHT):
	- A distributed system that know the location of the files
		- the IP addresses of the peers that store each file
- Tracker vs DHT
	- Different implementation of same service
		- input: file ID
		- output: IP's of peers that have file
		- Tracker is centralized, DHT is distributed
		- 
### Design the communication protocol

### Choose the transport-layer technology
#### Reliable data delivery
- Deliver message to destination process or signal failure
	- detect and recover from packet loss or corruption
#### Guaranteed Performance
- Minimum throughput
	- Throughput-sensitive applications
	- e.g., video-conferencing
- Maximum end-to-end packet delay
	- delay-sensitive applications,
	- e.g., emergency services, voice, gaming
#### Guaranteed security
- Confidentiality
	- message is revealed only to the destination
- Authenticity
	- message indeed came from claimed source
- Data integrity
	- message is not changed along the way



#### Internet transport layer protocols

##### Transmission Control Protocol (TCP)
- reliable, in-order data delivery, flow control, congestion control
- Keeps re-transmitting packet until it reaches destination or signal to app it has failed
	- TCP code at destination keeps state on source
	- TCP code at source keeps state on the destination
##### User Data gram Protocol (UDP)
- detection of packet corruption




