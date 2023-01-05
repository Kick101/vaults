
### Registers
Very fast, temporary stores data

__Six General purpose registers__
- eax
- ebx
- ecx
- edx
- esi
- edi

__Reserved Registers__
- ebp: Base pointer
- esp: Stack pointer
- eip: Instruction pointer
---
### Stack
>Stack grows upwards but memory address go up as we go down
- __push__ - add element to the top of the stack
- __pop__ - remove top element from the stack
- Elements on the lower have the higher address
- Stack grows towards lower memory address
- Each invoked function has a stack frame
- Local variables of the stack frame is stored on the stack frame
- _EBP_ register contains the address of the base of the current stack frame
- _ESP_ register contains the address of the top element of the current stack frame.
- Addresses outside of the stack frame is considered as junk by the compiler
- 

