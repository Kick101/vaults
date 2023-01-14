
```asm
rbp: Base Pointer, points to the bottom of the current stack frame
rsp: Stack Pointer, points to the top of the current stack frame
rip: Instruction Pointer, points to the instruction to be executed

General Purpose Registers
These can be used for a variety of different things
rax:
rbx:
rcx:
rdx:
rsi:
rdi:
r8:
r9:
r10:
r11:
r12:
r13:
r14:
r15:
```

First few args are passed by these registers:
```asm
rdi:    First Argument
rsi:    Second Argument
rdx:    Third Argument
rcx:    Fourth Argument
r8:     Fifth Argument
r9:     Sixth Argument
rax:    Return Value
```

>With the `x86` elf architecture, arguments are passed on the stack.
```txt
+-----------------+---------------+---------------+------------+
| 8 Byte Register | Lower 4 Bytes | Lower 2 Bytes | Lower Byte |
+-----------------+---------------+---------------+------------+
|   rbp           |     ebp       |     bp        |     bpl    |
|   rsp           |     esp       |     sp        |     spl    |
|   rip           |     eip       |               |            |
|   rax           |     eax       |     ax        |     al     |
|   rbx           |     ebx       |     bx        |     bl     |
|   rcx           |     ecx       |     cx        |     cl     |
|   rdx           |     edx       |     dx        |     dl     |
|   rsi           |     esi       |     si        |     sil    |
|   rdi           |     edi       |     di        |     dil    |
|   r8            |     r8d       |     r8w       |     r8b    |
|   r9            |     r9d       |     r9w       |     r9b    |
|   r10           |     r10d      |     r10w      |     r10b   |
|   r11           |     r11d      |     r11w      |     r11b   |
|   r12           |     r12d      |     r12w      |     r12b   |
|   r13           |     r13d      |     r13w      |     r13b   |
|   r14           |     r14d      |     r14w      |     r14b   |
|   r15           |     r15d      |     r15w      |     r15b   |
+-----------------+---------------+---------------+------------+

```
---
##### Words
A word is just two bytes of data. A dword is four bytes of data. A qword is eight bytes of data.

---
### Flags
A flag is a particular bit of this register. If it is set or not, will typically mean something.
```asm
00:     Carry Flag
01:     always 1
02:     Parity Flag
03:     always 0
04:     Adjust Flag
05:     always 0
06:     Zero Flag
07:     Sign Flag
08:     Trap Flag
09:     Interruption Flag     
10:     Direction Flag
11:     Overflow Flag
12:     I/O Privilege Field lower bit
13:     I/O Privilege Field higher bit
14:     Nested Task Flag
15:     Resume Flag
```
---
### Instructions
__mov__
```asm
mov rax, rdx ; move data from rdx to rax
```
__dereference__
```asm
mov rax, [rdx] ; move value pointed by the rdx into rax

mov [rax], rdx ; move rdx data into whatever the memory pointed by rax
```
__lea__ (Load effective address)
```asm
lea rdi, [rbx+0x10] ; calculates the address and moves that address
```
__add__
```asm
add rax, rdx ; adds two values and stored into 1st
```
__sub__
```asm
sub rsp, 0x10 ; sub the 2nd operand from 1st and store in 1st
```
__xor__ (and, or) 
```asm
xor rdx, rax ; binary xor and store it in 1st
```
__push__
```asm
push rax ; Grow stack size by 8 for x64 or 4 for x86 then push contents onto stack
```
__pop__
```asm
pop rax ; pop data off the stack and into argument. Stack shrinks 
```
__jmp__
```asm
jmp 0x602010 ; Jump to an instruction address.
```
__call & ret__
- Similar to `jmp`. But this also push values of `rbp`, `rip` onto stack, then jump to whatever address it is given.
- Used for calling functions. After the function is finished, a `ret` instruction is called which uses the pushed values of `rbp` and `rip` . It can continue execution right where it left off.

__cmp__
- Similar to sub instruction. Except it doesn't store the result in the first argument.
- It checks if the result is less than zero, greater than zero, or equal to zero. Depending on the value it will set the flags accordingly.

__jnz/jz__
-  `jump if not zero` & `jump if zero` (`jnz/jz`). They will only execute the jump depending on the status of the `zero` flag. 
- For `jz` executes if the `zero` flag is set. The opposite is true for `jnz`.

__jle__
- Jump if less or equal

---
### Binaries
__Objdump__
```bash
objdump -D <executable> -M intel | less
```
__loop__
```asm
804840c:       mov    DWORD PTR [ebp-0xc],0x0  ; x = 0
8048413:       jmp    804842c <main+0x31>  
8048415:       sub    esp,0x8                  ; stack increase
8048418:       push   DWORD PTR [ebp-0xc]      ; push x onto stack
804841b:       push   0x80484c0                ; push this value onto stack
8048420:       call   80482d0 <printf@plt>     ; printf 0x80484c0
8048425:       add    esp,0x10  
8048428:       add    DWORD PTR [ebp-0xc],0x1  
804842c:       cmp    DWORD PTR [ebp-0xc],0x13  ; x <= 13
8048430:       jle    8048415 <main+0x1a>       ; jump if above true 
```