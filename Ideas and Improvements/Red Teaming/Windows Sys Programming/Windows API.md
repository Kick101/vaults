 - Process not responding - thread that manages UI not checks it's message queue for at least 5 sec
 - Session 0 - SYSTEM, LOCAL SERVICE SYSTEM
 - Session >=1 - USER process
 - Commit Size - Private memory of the process
 - Active memory - Current active private memory
 - Handles - Handles to objects
 - Threads -  Entity that is scheduled by the kernel to execute code on a processor

__Thread maintains__
- The state of CPU registers
- Current access mode (user mode or kernel mode)
- Two stacks: user space & kernel space
- Thread Local Storage (TLS)
- Optional security token
- Optional message queue & Windows(rectangle box around UI) the thread creates
- Priority, used in thread scheduling
- State: running, ready, waiting
 
__Architecture__
- Kernel32.dll is in User Mode, subsystem dll. 
- NTDLL.dll lowest layer in user mode, implements native windows API
- NTDLL.dll invoked into Kernel mode
- Executive layer in kernel mode, holds managers like I/O, memory, plugin play, configuration
- Executive layer invokes syscalls
- Device drivers, file system drivers and kernel beneath executive layer
- Hardware Abstraction Layer (HAL)

![[Pasted image 20240128011230.png|500]]

#### NTDLL.DLL Kernel Gate
![[Pasted image 20240128011405.png|500]]

#### Function Call Flow
![[Pasted image 20240128011734.png|500]]

#### Windows Subsystem API
- Some are C based and DCOM
- .NET: C#, VB.NET, F#, C++/CLI
- Windows Runtime (WinRT): Built on top of COM

---
### Application Basics

