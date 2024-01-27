### Processes Memory
 - Process not responding - thread that manages UI not checks it's message queue for at least 5 sec
 - Session 0 - SYSTEM, LOCAL SERVICE SYSTEM
 - Session >=1 - USER process
 - Commit Size - Private memory of the process
 - Active memory - Current active private memory
 - Handles - Handles to objects
 - Threads -  Entity that is scheduled by the kernel to execute code on a processor

__Threads maintains__
- The state of CPU registers
- Current access mode (user mode or kernel mode)
- Two stacks: user space & kernel space
- Thread Local Storage (TLS)
- Optional security token
- Optional message queue & Windows(rectange box around) the thread creates

 



---
### Threads
