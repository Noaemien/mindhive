---
Week: 6
Themes:
  - Mount Points
  - I/O
aliases: 
Lecture1: true
Lecture2: false
Exercises: false
---

# Notes

## Lecture 1

### Mount Points

#### Multiple file systems

**All systems are mapped to a common root**
**Linux/Unix** file systems can be mapped anywhere into a single tree

**Mount:** Allow multiple file systems on multiple volumes to form a single logical hierarchy
A mount point is an abstraction

#### Benefits:
##### Diverse filesystem types can co-exist in the same namespace
- ext3, ext4, NTFS -> filesystems optimised for hard disks
- iso96000 -> filesystem standards for CDROM
- FAT -> the legacy, universal MS-DOS filesystem
### I/O

#### General I/O Abstraction Stack
![[Pasted image 20240327094339.png|300]]

#### OS basics: IO
Common services in form of IO:
- Load programs and data from storage
- Write data to a terminal or the screen
- Read / write packets from network
- Read data from input devices

![[Pasted image 20240327094706.png|500]]

#### Simplified hardware with storage devices
- IO controller supports IO devices and is responsible for handling their requests
- IO controller can throw interrupts to CPU
- OS handles device management and exposes uniform interface to apps
- OS processes access these devices by read/writing IO registers

#### PCI Bus
*Buses are common set of wires for communication among hardware devices plus protocols for carrying out data transfer transactions*
- Connect devices over single set of wires, connections and protocols
- PCI express -> evolution of PCI, can have 64 GB/s bandwidth for 16 lanes

#### Canonical device

Simple device with 3 registers:
- Status used to determine if it is possible or not to perform an operation 
- Command
- Data

##### CPU talking to device
1. Port-mapped IO: CPU uses designated IO ports
	- Each device has an assigned port
	- Special instructions
2. Memory-mapped IO (high performance devices): CPU uses load/store instructions
	- Hardware maps control registers into the physical address space
	- IO accomplished via load/store instructions
##### Operations parameters for IO
- Data granularity: byte vs block
	- Some devices provide single byte at a time (keyboard)
	- others provide whole blocks (disks, NICs)
- Access pattern: sequential vs random
	- some devices are accessed sequentially (tape)
	- Others can be accessed randomly (disks, CD)
##### Polling vs interrupts
- **IO interrupt:**
	- Device generates an interrupt whenever it needs service
	- Pro: Handles unpredictable events well
	- Con: Interrupts have relatively high overhead
	- Too many interrupts would freeze the system
- **Polling:**
	- OS periodically checks a device specific status register
		- IO devices put completion info in status registers
	- Pro: Low overhead
	- Con: Wastes CPU cycles if infrequent or unpredictable IO operations

Real systems use a mix of polling and interrupts


#### Transferring data to/from controller
##### PIO (programmed IO)
CPU tells the device what data is
- One instruction for each byte/word
- Efficient for few bytes, consumes CPU cycles proportional to data size

##### DMA (direct memory access)
CPU tells the device where data is
- Give controller access to memory bus
- Ask to transfer data to/from memory directly
- Efficient for large data transfers

### Device Drivers
#### Managing Device diversity
**Challenge:** Different devices have different protocols
**Device drivers** are specialized pieces of code in the kernel that interacts directly with the device
- Supports a standard, internal interface
- Same kernel IO system call can interact with different device drivers

Drivers are an example of encapsulation
- Different drivers adhere to the same API
- OS only implements the support for APIs based on device class
Requirement: well-designed interface/API

#### OS device structure

![[Pasted image 20240327103205.png|500]]

#### Device driver structure
##### Top half
accessed synchronously in call paths to initiate I/O
- system calls, eg: open/close/read/write
- by the filesystem
- by the networking stack
- ...
##### Bottom half
runs asynchronously on I/O completion
- As an interrupt routine
- Communicates with the device
Interactions between top and bottom halves are very tricky

## Lecture 2

### Persistence: Magnetic disks
- Magnetic disk:
	- Large capacity at low cost
	- Block level random access
	- Slow performance for random access
	- Good performance for streaming access
- Flash memory:
	- Capacity at intermediate cost
	- Block level random access
	- Medium performance for random writes
	- Good performance otherwise

#### How Magnetic disks work
![[Pasted image 20240327103952.png]]


#### Disk overhead
**Disk latency** = **Seek time** + **Rotation time** + **transfer time**

- **Seek**: position the head/arm over the proper track
	- To get to the track (5-15ms)
- **Rotational latency:** Wait for desired sector to rotate under read/write head (4-8ms)
	- Only need to wait for half a rotation on average
- **Transfer time:** Transfer a block of bits (sectors) under read/write head (25-50 usec)

> [!NOTE] IO Takes time
> Milliseconds of latency represent millions of instructions that can execute. Reading from disk is very slow.

-> Disk operations need to be scheduled in order to obtain high performance from the disk


#### Disk scheduling
**Goal:** Minimize the seek time

*Example Context:*
- 200 cylinder numbers
- Head pointer @65
- Queue: 150, 16, 147, 14, 72, 83
##### FIFO
Schedule disk operations in order they arrive
Example: Results in disk head moving 552 tracks

##### Shortest Seek time first
Select request with minimum seek time from current head position
(Similar to SJF)
Not optimal: suppose a cluster of request at far end of disk -> starvation
Example: head moves 221 tracks

##### Scan scheduling "elevator"
Move the head in one direction until all requests have been serviced, and then reverse
Sweeps disk back and forth
Example head moves 187 tracks

### File system performance
#### Key metrics
- Latency (of operations)
- Throughput
- I/O operations per second (IOPS)

#### How to improve performance
**Caching**
- Avoid unncessary operations

- File system buffer cache (block cache):
	- Map: {inode, block_offset} -> page frame number
	- sys_read can return without perfoming disk I/O
	- On modern systems, the file system buffer cache may use all unused memory

**Batching**
- Group operations to increase throughput
- Delay idempotent operations
- Each I/O operation is costly with limited concurrency
	- Perform fewer operations with larger transfers
	- Possible when consecutive blocks on disk belong to same inode
		- "disk fragmentation": metric of what fraction of inode content are not on consecutive locations on disk
**Delaying operations**
- A process must block on a read operation. But writes don't
	- Perform them asynchronously (typical: wait at most 30 seconds)
	- Reorder operations to maximize throughput (insert within the elevator algorithm)
	- Content would be lost if OS crashes
**Adding a level of indirection**

#### Modern file systems
- Multi-level indexing was introduced with early UNIX systems
- Early 1990s: introduction of log-structured filesystems
	- Insight: because of caching and increased memory sizes, most I/O is actually writes, not reads
	- Idea: all writes should be to a log. Then reconstruct file from log

#### Alternative view point: bypass the kernel