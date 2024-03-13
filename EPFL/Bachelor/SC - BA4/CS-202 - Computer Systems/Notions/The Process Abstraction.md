---
aliases:
  - process
  - Processes
---
Abstraction provides an interface to application programmers that separates policy from mechanism.
It does this by:
- hiding unwanted properties
- adding new capabilities
- organising information

# What is a process
A process in an instance of a program. Created when running an Executable file.
## Executable files
**Executable Code**: CPU instructions.
**Data**: Information manipulated by these instructions.
Obtained by compiling a program.
## Differences between process and program
A **program** consists of static code and data. Executable on the disk that has all information to start a process.
A program is **passive**: code + data

A **process** is an instance of a program (at any time there may be 0 or more instances of a program running), has data section and stack initialized.
A process is **alive**: mutable data + registers + open files...
We can run the same program multiple times simultaneously.

A process can have multiple **threads** in the same address space
# What constitutes a process
## A unique identifier: Process ID (PID)
Unique for all different program instances. Identifies specific process.

## Memory image

![[processMemorySegments.png|300]]

### Dynamic Segment
#### Call Stack
Stores **temporary data** such as **local variables**, **function parameters** or **return addresses**.
The Stack grows from **higher addresses to lower addresses**
#### Heap

Heap is used for **dynamic memory allocation** during runtime. For example: using malloc or calloc return a pointer to heap. Heap grows from lower addresses to higher addresses

### Static Segment
#### Data Segment
Statically allocates **global variables and data structures**. These are **known at compile time**.
#### Text
Read only Text segment contains **code** and **constants**. This is the program executable machine instructions


## CPU context: registers
- Program counter
- Current operands
- Stack pointer

## File descriptors
Pointers to open files and descriptors

# How the OS creates a process

- **Loading:** OS loads the static code and data into memory

- **Memory allocation:** Allocate process memory regions (heap and stack)

- **Initialization:** Initialize tasks related to IO (setting up STDIN, STDOUT, STDERR in file descriptor table)

- **Ready:** OS sets the stage for running the process by transferring the CPU control at beginning of the program's entry point (e.g., main() function)




# Process scheduling
OS Scheduler picks one of the processes to run
	- [[Scheduler]] keeps a list of processes
	- [[Scheduler]] keeps track of process states and schedules them according to the scheduling policy

# Process States
## Three States

### Running
A process is running on a CPU. This means it is executing instructions

### Ready
