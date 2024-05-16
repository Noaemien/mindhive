---
Week: 7
Themes: 
aliases: 
Lecture1: true
Lecture2: true
Exercises: false
---

# Notes
## Lecture 1

## What's on an Internet path ?
Internet path is owned and managed by **Internet Service Providers** (ISPs)

![[Pasted_image_20240408094745.png|500]]

Regional ISP's can have a peering relationship to bypass tier-1 ISP fees.
ISP's don't have to have direct physical connection between each other. Can be connected using IXP (Internet Exchange Provider). 
Content / cloud providers can peer with tier-1 ISP's.
Lower tier ISP's used caches to minimise requests to higher tier ISP's.

### Network system calls
- send
- recv
- connect
- listen
- accept
The *Application Programming Interface* (API) exposed to processes for Internet communication

![[Pasted_image_20240408104056.png|500]]
### Layers
Provides an abstraction of what lies below to layer above

### Names (identifiers)
For network interfaces: DNS names, IP addresses, MAC addresses
For processes: network-interface name + port number
## Lecture 2

### Basic network performance metrics
- Packet loss
	- the fraction of packets that are lost on the way from src to dest
- Packet delay
	- the time it takes for a packet to get from src dst
	- Many components: transmission, propagation, queuing, processing
	- Depends on network topology, link properties, switch operation, queue capacity, other traffic
- Average throughput
	- the average rate at which dst recieves data
	- in bits per second (bps)

> Packet delay matters for small messages but average throughput matters for bulk transfers

Bottleneck link:
- the link where traffic flows at the slowest rate.
- Could be because of link's transmission rate or because of queuing delay

### Queuing delay
- Assuming infinites queue
	- Approaches infinity, if arrival rate > departure rate
	- depends on burst size otherwise


### Switch contents
- Queue
	- stores packets
- Forwarding table
	- store meta-data
	- indicate where to send each packet

### Resource management
- Packet switching
	- packets treated *on demand*
	- admission control & forwarding decision: per packet
	- Unpredictable performance
	- Efficient use of ressources
	- Can serve more users using statistical multiplexing (not all users share ressource at the same time but most users are not active all the time)
- "Connection switching"
	- resources *reserved* per active connection
	- admission control & forwarding decision: per connection
	- Predictable performance
	- Inefficient use of ressources

### Internet vulnerabilities
- Eavesdropping (sniffing)
- Impersonation (spoofing)
- Denial of service (dos-ing)
- Malware
