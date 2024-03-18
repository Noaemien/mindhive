---
aliases:
  - CPU
---

# The von Neumann Architecture
## Structure of the CPU
### Arithmetic / Logic Unit
Performs operations
### Control Unit
Controls what happens next
### Registers
The CPU has at least one register, the instruction pointer (IP) (also known as the Program Counter PC) which interprets instructions at address IP

## Key enhancements

### Limited Direct Execution (CPU privileges)

**Direct execution:** processes executed directly on CPU without any restrictions

CPU privileges are necessary for security as they prohibit untrusted actors to perform potentially dangerous operations.
Protects applications from:
- other applications code
- affecting the OS
- unfairly utilizing resources
- accessing/modifying unallocated resources
#### Kernel Mode
Has the ability to perform restricted operations such as I/O related operations.
#### User Mode
Restricted access.

##### When CPU tries to execute privileged instruction from user mode
- Instruction does not execute
- The instruction traps (#General protection fault on x86)
- The OS takes control 
#### Context switch
**User to kernel:** CPU saves registers, to per-process stack, raises privilege level to kernel
**Kernel to user**: CPU lowers privilege, restores registers

### [[Memory Management Unit (MMU)]] 
Circuit contained in CPU that enables the translation of virtual addresses to physical addresses

# CPU virtualization
## Goal
Give each process the illusion of **exclusive CPU access**

## Reality
The CPU is shared resource among all processes

CPU is shared using [[Ressource sharing]]

