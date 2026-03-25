### Executable and Linkable Format (ELF)
[Blog post](https://www.intezer.com/blog/research/executable-linkable-format-101-part1-sections-segments/)
##### ELF Header 
- The ELF header is denoted by an __Elfxx_Ehdr__ structure. Mainly, this contains general information about the binary. 
-   __e_ident__: Array of 16 bytes containing identification flags about the file, which serve to decode and interpret the file’s contents. Examples of these identification flags include
	- EI_MAG0-3: ELF magic:
	- EI_CLASS: File class.
	- EI_DATA: File’s data encoding.
	- EI_VERSION: File’s version.
	- EI_OSABI: OS/ABI identification.
	- EI_ABIVERSION: ABI version
	- EI_PAD: Start of padding bytes.
	- EI_NIDENT: Size of ei_ident.

![[Pasted image 20230103192039.png]]
-   e_type: Type of executable.
-   e_machine: File’s architecture.
-   e_version: Object file version.
-   e_entry:  Entry point of application.
-   e_phoff: File offset of the Program Header Table.
-   e_shoff: File offset of the Section Header Table.
-   e_flags: Processor-specific flags associated with the file.
-   e_ehsize: ELF header size.
-   e_phentsize: Program Header entry size in Program Header Table.
-   e_phnum: Number of Program Headers.
-   e_shentsize: Section Header entry size in Section Header Table.
-   e_shnum: Number of Section Headers.
-   e_shstrndx: index in Section Header Table Denoting Section dedicated to Hold Section names.
```bash
readelf -h <executable>
```
##### Program/Segment Headers
- Program headers specify information needed to prepare the program for execution.
- Magic bytes: `7f 45 4c 46` -> ELF
- Program Headers are not needed on linktime.
- Every ELF binary contains a Program Header Table which comprises of a single __Elfxx_Phdr__ structure per existing segment. Definitions of these structure’s fields are the following:
	- p_type: Segment type.
	- p_flags: Segment attributes.
	-   p_offset: File offset of segment.
	-   p_vaddr: Virtual address of segment.
	-   p_paddr: Physical address of segment.
	-   p_filesz: Size of segment on disk.
	-   p_memsz: Size of segment in memory.
	-   P_align: segment alignment in memory.

![[Pasted image 20230103193603.png]]

Common Segment types:
- PT_NULL: unassigned segment (usually first entry of Program Header Table).
-   PT_LOAD: Loadable segment.
-   PT_INTERP: Segment holding .interp section.
-   PT_TLS: Thread Local Storage segment (Common in statically linked binaries).
-   PT_DYNAMIC: Holding .dynamic section.
```bash
readelf -l <executable>
```
##### Section Headers
- Sections comprise all information needed for linking a target object file in order to build a working executable.
- Useful for information for introspection, debugging etc. (metadata)
- Sections are needed on linktime but they are not needed on runtime.
- In every ELF executable, there is a Section Header Table. This table is an array of Elfxx_Shdr structures, having one __Elfxx_Shdr__ entry per section.
	- sh_name: index of section name in section header string table.
	- sh_type: section type.
	- sh_flags: section attributes.
	-   sh_addr: virtual address of section.
	-   sh_offset: section offset in disk.
	-   sh_size: section size.
	-   sh_link: section link index.
	-   sh_Info: Section extra information.
	-   sh_addralign: section alignment.
	-   sh_entsize: size of entries contained in section.

![[Pasted image 20230103192334.png]]
Some common sections are the following:
-  .text: code.
-   .data: initialised data.
-   .rodata: initialised read-only data.
-   .bss: uninitialized data.
-   .plt: PLT (Procedure Linkage Table) (IAT equivalent).
-   .got: GOT entries dedicated to dynamically linked global variables.
-   .got.plt: GOT entries dedicated to dynamically linked functions.
-   .symtab: global symbol table.
-   .dynamic: Holds all needed information for dynamic linking.
-   .dynsym: symbol tables dedicated to dynamically linked symbols.
-   .strtab: string table of .symtab section.
-   .dynstr: string table of .dynsym section.
-   .interp: RTLD embedded string.
-   .rel.dyn: global variable relocation table.
-   .rel.plt: function relocation table.
```bash
readelf -S <executable>
```
##### Symbols
Binaries and libraries that use dynamically loaded libraries rely symbols to find libraries, resolve function calls into those libraries, etc.
- Each symbol is represented as an instance of __Elfxx_Sym__ struct.
![[Pasted image 20230103221909.png]]
-   st_name: index in string table of symbol name. If this field is not initialized, then the symbol doesn’t have a name
-   st_info: contains symbol bind and type attributes. Binding attributes determine the linkage visibility and behavior when a given symbol is referenced by an external object. The most common symbol binds are the following:
    -   STB_LOCAL: Symbol is not visible outside the _ELF_ object containing the symbol definition.
    -   STB_GLOBAL: Symbol is visible to all object files.
    -   STB_WEAK: Representing global symbols, but their definition can be overridden.

The most common symbol types are the following:

- STT_NOTYPE: symbol type is not specified
-   STT_OBJECT: symbol is a data object (variable).
-   STT_FUNC: symbol is a code object (function).
-   STT_SECTION: symbol is a section.

In order to retrieve these fields, a set of bitmasks are used. These bitmasks include:
-   ELF64_ST_BIND(info)    ((info) >> 4)
-   ELF64_ST_TYPE(info)    ((info) & 0xf)

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