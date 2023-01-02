### Executable and Linkable Format (ELF)
[Blog post](https://www.intezer.com/blog/research/executable-linkable-format-101-part1-sections-segments/)
##### Program/Segment Headers
Program headers specify information needed to prepare the program for execution. Most important entry types.
Magic bytes: `7f 45 4c 46` -> ELF
- __INTERP:__ Defines the library that should be used to load this ELF into memory.
- __LOAD:__ Defines a part of the file that should be loaded into memory.

##### Section Headers
Useful for information for introspection, debugging etc.
Section headers are not necessary part of the ELF. They are metadata.
- __.text__: executable code
- __.plt__ & __.got__: resolve & dispatch library calls
- __.data__: pre-initialised global writable data (global arrays with initial values)
- __.rodata__: global read-only data (strings constants)
- __.bss__: uninitialised global writable data (global arrays without initial values)

##### Symbols
Binaries and libraries that use dynamically loaded libraries rely symbols to find libraries, resolve function calls into those libraries, etc.

---
#### Interacting with ELF
- _gcc_ to make ELF
- _readelf_ to parse ELF headers
- _objdump_ to parse the ELF header & disassemble the source code
- _nm_ to view ELF symbols
- _patchelf_ to change some ELF properties
- _objcopy_ to swap out ELF sections
- _strip_ to remove otherwise-helpful information (such as symbols)
- _kaitai struct_ to look through ELF interactively
- _ldd_ print shared object dependencies

##### objcopy
```bash
# Dump binary section
objcopy --dump-section .rodata=binary.out binary
# Change binary
vi -b binary.out 
# Update binary
objcopy --update-section .rodata=binary.out binary
```
##### patchelf
```bash
# Change intrepreter
patchelf --set-interpreter /some/interpreter ./binary
```
---
#### Linux Process Loading
__Process Attributes__
- State (running, waiting, stopped, zombie)
- Priority (and other scheduling info)
- Parent, siblings, children
- Shared resources (files, pipes, sockets)
- Virtual memory space
- Security Context
	- effective uid & gid
	- saved uid & gid
	- capabilities

__Process mitosis__
- _Fork_ & _clone_ are syscalls that create nearly exact copy of the calling process: a part and a child
- _execve_ syscall to replace itself with another process.
>- Example:
>	- When `/bin/cat` executed
>	- Bash (shell) forks itself into the old parent process and the child process
>	- The child process _execve_  `bin/cat`, becoming `/bin/cat`

__Loading Files__
- Linux kernel reads the beginning of the file
- If the file starts with `#!` the kernel extracts the interpreter from the rest of that line and executes this interpreter with the original file as an argument.
- If the file matches a format in `/proc/sys/fs/binfmt_misc`, the kernel executes the interpreter specified for that format with the original file as an argument.

__Loading dynamically linked ELF__
-  