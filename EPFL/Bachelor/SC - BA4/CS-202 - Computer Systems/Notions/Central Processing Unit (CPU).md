
# The von Neumann Architecture
## Structure of the CPU
### Arithmetic / Logic Unit
Performs operations
### Control Unit
Controls what happens next
### Registers
The CPU has at least one register, the instruction pointer (IP) (also known as the Program Counter PC) which interprets instructions at address IP

## Key enhancements

### CPU privileges
CPU privileges are necessary for security as they prohibit untrusted actors to perform potentially dangerous operations
#### Kernel Mode
Has the ability to perform restricted operations such as I/O related operations.
#### User Mode
Restricted access.

### [[Memory Management Unit (MMU)]] 
Circuit contained in CPU that enables the translation of virtual addresses to physical addresses

# CPU virtualization
## Goal
Give each process the illusion of **exclusive CPU access**

## Reality
The CPU is shared resource among all processes

CPU is shared using [[Ressource sharing]]

