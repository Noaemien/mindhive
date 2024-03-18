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
**Executable Code**: [[Central Processing Unit (CPU) | CPU]] instructions.
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

## Memory image (address space)

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
- Program counter:
	- Points into text segment, current line to execute
- Current operands
- Stack pointer:
	- Points to top of stack

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
The process is running on a CPU. This means it is executing instructions

### Ready
The process is ready to run but for some reason CPU has not chosen to run it yet (could be for scheduling reasons).

### Blocked
The process triggered an operation that makes it not ready to run because it is waiting on the completion of a another event. Example: Process gets blocked for an I/O request to disk.

## State Transitions
![[processStateTransition.png|600]]




# Process Control Blocks
Also known as `task_struct` on linux

## Elements
- Process ID (PID)
- Process state / context
- Pointers to parent process (`cat /proc/<pid>/status`)
- CPU context
- Pointer to address space (`cat /proc/<pid>/maps`)
- IO status information

# Process API
Main methods

## System calls

Each system call has a specific number

### Commands
#### fork()
Executes a child process that is a copy of the parent process.
Child has a different process ID
**Returns** child PID (as an int):
- If executing in parent process:
	- returns pid > 0 if child is created
	- returns pid -1 if there is an error. The new process is not created
- returns 0 if executing in child process

##### Execution
- OS allocates data structures for the child process
- OS makes a copy of the caller's (parent) memory / address space
- OS also copies other states, such as [[File Descriptors|file descriptors]], from the parent process
- The state of the child process is set to READY
- Parent and child execute in their **own separate copy** of the memory
- `fork`is implemented by the OS

#### exec()
Executes a new program within the current process, replacing the previous executable program.

Replaces the memory (address space), loads new program from the disk --> Code, data, heap and stack are replaced by new program.

Only PID and STDIN, STDOUT and STDERR which allows parent to redirect/rewire child's output.

The new program can pass command line arguments and environment (can inherit them from parent).

Commonly used after fork() to start a new program.
#### exit()
When a process terminates, it executes exit(), either directly on its own, or indirectly via library code

Resumes execution of waiting parent process

**Returns nothing**

#### wait()
A parent process uses `wait()` to *suspend its execution* until one of its children terminates. The parent process then gets the exit status of the terminated child()

If no child is running then wait() has no effect

**Returns**:
- PID of terminated child process
- -1 if parent has no children processes

#### Implications

Process Termination Scenarios:
- It calls `exit()` on itself
- OS terminates a misbehaving process

Terminated process exists as a zombie.
When a parent process calls `wait()`, the zombie child is cleaned up or **reaped** (removed from the process table to prevent the accumulation of zombie processes and ensure efficient resource management )
If a parent terminates before child, the child becomes an **orphan**
**init** (pid: 1) process adopts orphans and reaps them


Transfer execution to OS, during this time the process is paused.

### Execution steps
- Process executes a special **trap** instruction
	- Process states are saved onto per-process-stack (Instruction pointer, flags, general purpose registers)
	- CPU jumps into kernel mode and raises the privilege level at the same time
	- Now privileged operations can be performed
- When finished OS executes return-from-trap instruction
	- CPU returns to calling process and lowers privilege level
	- Restores states of process
	- privileged operations can no longer be performed

### Traps (Exceptions)
- Handle internal program errors
	- Overflow, division by zero, accessing not allowed memory regions
- Exception are produced by CPU while executing instructions
- **Synchronous**
- During Trap, kernel code checks validity of system call number and executes corresponding code

#### Trap/Interrupt Table
OS configures hardware at boot time. What code to execute when certain events happen
![[Pasted image 20240318150952.png|500]]

