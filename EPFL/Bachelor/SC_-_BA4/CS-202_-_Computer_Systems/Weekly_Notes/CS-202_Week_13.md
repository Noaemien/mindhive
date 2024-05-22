---
Week: 13
Themes: 
aliases: 
Lecture1: true
Lecture2: true
Exercises: false
---

# Revisiting Virtualization
> [!Note] Quote
> *Any problem in computer science can be solved by adding a level of indirection. But this usually creates another problem*

Virtualisation applies indirection to layering when the layered abstraction is compatible with the layer it relies on

## Virtualization mechanisms
### Multiplexing
- Exposing a single resource among multiple virtual entities
- Example: CPU shared among multiple processes

### Aggregation
- Making multiple different resources look like one single virtual resource
- Example: Disk volumes
- Primary use:
	- A computer has multiple DIMM (Memory Module)
		- The memory controller aggregates the physical memory into a single (typically contiguous) namespace
	- A computer has many hard drives
		- RAID (Redundant Array of Inexpensive Disks) make them appear as a single volume
		- ![[Pasted image 20240522084823.png|500]]
		- n disks
		- M sectors on physical disk
		- N sectors on virtual disk
		- **Raid 0 (Striping)**: M = N * n sectors on physical disk
		- **Raid 1 (Mirroring)**: M = N, n = 2
		- **Raid 5 (Distributed parity encoding**: M = N * (n-1), n >= 3
	- A network has multiple links connecting servers
		- Link aggregation and multipathing create a virtual link with more bandwidth

### Emulation
- Using software to emulate a resource different from the underlying resource
- Example: a serial port over a USB port, or over ssh


# Virtual Machines

## Why virtual machines
Very popular in the very early days of computing
- Hardware was expensive
- Operating systems were primitive
Back in favor since 2000
- To ensure compatibility with the OS of choice
- To consolidate servers
- As a foundation for multi-tenancy (different owners)

> [!Note] Definition
> A virtual machine is an efficient isolated duplicate of the real machine

##  Terminology

### Operating systems
- Guest operating system: running inside a virtual machine
- Host operating system: running directly on the hardware - combined with the VMM
### Memory
- Virtual memory: as commonly understood
- Guest physical memory: physical memory of the VM
- Host physical memory: (actual physical memory)

## Types
- Type 1: VMM is also the host OS
	- Example: Xen, vSphere
	- ![[Pasted image 20240522092852.png|200]]
- Type 2: VMM optional extension to host OS
	- Example: KVM and linux
	- ![[Pasted image 20240522092919.png|200]]

## Constructing a VMM
P = Computer system
V = computer system (== virtual machine)

### Multiplexing and emulating
**Multiplex** CPU and memory, with specific hardware support
- Each VM has its own virtual CPU (= virtual core)
	- P=core; V=core
	- Requires system-level limited direct execution
- Each VM has its own guest physical memory
	- P = physical memory; V = physical memory
	- Relies on extended page tables
**Emulate** I/O devices