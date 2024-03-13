
# What is an OS
An OS is a **layer of software** that **interfaces** between hardware ressources and one or multiple applications running on the machine. It is a special program that should **never stop** and **never fail**. It is fully **trusted** by hardware and applications.

 ![[Pasted image 20240312210618.png|150]]

## The OS Provides
### **Protection**, **isolation** and **sharing of ressources**

#### Fault isolation
Fault isolation is intended to separate [[The Process Abstraction|Processes]] from each-other as well as from the OS
Un-allowed access results in **Segmentation fault** 

#### [[Ressource sharing]]

[[Scheduling]]
The OS needs to choose which [[The Process Abstraction|process]] to execute, as well as how much of each physical ressource to allocate to each [[The Process Abstraction|process]].

#### Communication

The OS allows processes to communicate with each other in a protected manner

### Clean and easy to use **abstraction of physical ressources**

**Virtualization** is used to give applications the illusion of exclusive CPU access along with the illusion of infinite hardware ressources.

Higher level objects such as files, users and messages are used.

#### Process Abstraction

The OS provides process abstraction to every program, enabling each process to execute in a restricted execution environment

[[The Process Abstraction|Processes]] have a nicer interface than raw hardware: Threads, Address space, Files and Sockets
The OS translates from hardware interface to application interface.
It provides each running program with its own process.
![[Pasted image 20240312211634.png|]]

### A set of **common services**

The OS provides a set of common services to standardise the design of applications.
This makes sharing easier and maximises reuse as all applications are built upon the same base.
Decouples application development from hardware.

For example:
- File Systems
- User Interface
- Network