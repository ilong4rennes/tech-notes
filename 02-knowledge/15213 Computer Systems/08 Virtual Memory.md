## Basic Idea of Virtual Memory

### Processes

- Definition: A process is an instance of a running program. 
	- One of the most profound ideas in computer science 
	- Not the same as “program” or “processor” 
- Unix: A parent process creates a new child process by calling `fork` 
	- Child is (sort of) a copy of the parent 
	- `fork` returns twice—once in each process 
		- Different return value in each 
- Parent can wait for child to finish by calling `waitpid`
	- For now, think of this as “what `main` returns to

How does this work? ![[Screen Shot 2024-06-18 at 13.44.55.png]]
**Solution: Virtual Memory**

### A System Using Physical Addressing

![[Screen Shot 2024-06-18 at 13.46.09.png]]
- Used in “simple” systems like embedded microcontrollers in devices like cars, elevators, and digital picture frames
### A System Using Virtual Addressing

![[Screen Shot 2024-06-18 at 13.46.18.png]]
- Used in all modern servers, laptops, and smart phones 
- One of the great ideas in computer science

### VM as a Tool for Memory Management

- Key idea: **each process has its own virtual address space**
	- It can view memory as a simple linear array 
	- Mapping function scatters addresses through physical memory 
		- Well-chosen mappings can improve locality
![[Screen Shot 2024-06-18 at 13.54.09.png]]
- Simplifying memory allocation 
	- Each virtual page can be mapped to any physical page 
	- A virtual page can be stored in different physical pages at different times 
- Sharing code and data among processes 
	- Map virtual pages to the same physical page (here: PP 6)
- Linking 
	- Each program has similar virtual address space 
	- Code, data, and heap always start at the same addresses.
- Loading 
	- `execve` allocates virtual pages for `.text` and `.data` sections & creates PTEs marked as invalid 
	- The `.text` and `.data` sections are copied, page by page, on demand by the virtual memory system
![[Screen Shot 2024-06-18 at 14.07.22.png]]

## VM Address
### Address Spaces

**Linear address space**: Ordered set of contiguous non-negative integer addresses. ${0, 1, 2, 3 … }$

**Virtual address space**: Set of `N = 2^n` virtual addresses. Typically larger than physical address space. The same for all processes running on the system. ${0, 1, 2, 3 … N-1}$

**Physical address space**: Set of `M = 2^m` physical addresses. Amount of DRAM present in the system. ${0, 1, 2, 3 … M-1}$

### Why Virtual Memory (VM)? 
- Uses main memory efficiently 
	- Use DRAM as a cache for parts of a virtual address space 
- Simplifies memory management 
	- Each process gets the same uniform linear address space 
- Isolates address spaces 
	- One process can’t interfere with another’s memory 
	- User program cannot access privileged kernel information and code

## Page Table, Address Translation
### VM Address Translation

- Virtual Address Space V = {0, 1, …, N–1} 
- Physical Address Space P = {0, 1, …, M–1} 
- Address Translation 
	- MAP: V → P U {∅} 
	- For virtual address a: 
		- MAP(a) = a’ if data at virtual address a is at physical address a’ in P 
		- MAP(a) = ∅ if data at virtual address a is not in physical memory – Either invalid or stored on disk

### Enabling Data Structure: Page Table

- A page table is an array of page table entries (PTEs) that maps virtual pages to physical pages. 
	- Per-process kernel data structure in DRAM (each process will have its own mapping)
![[Screen Shot 2024-06-18 at 14.23.03.png]]

![[Screen Shot 2024-06-18 at 14.24.40.png]]

### Page Hit

- Page hit: reference to VM word that is in physical memory (DRAM cache hit)
![[Screen Shot 2024-06-18 at 14.30.53.png]]
![[Screen Shot 2024-06-18 at 14.32.05.png]]
1 - Processor sends virtual address to MMU 
2-3 - MMU fetches PTE from page table in memory 
4 - MMU sends physical address to cache/memory 
5 - Cache/memory sends data word to processor

### Page Fault

- Page fault: reference to VM word that is not in physical memory (DRAM cache miss)
![[Screen Shot 2024-06-18 at 14.35.57.png]]
![[Screen Shot 2024-06-18 at 14.38.27.png]]

1 - Processor sends virtual address to MMU 
2-3 - MMU fetches PTE from page table in memory 
4 - Valid bit is zero, so MMU triggers page fault exception 
5 - Handler identifies victim (some page of physical memory) (and, if dirty, pages it out to disk) 
6 - Handler pages in new page and updates PTE in memory 
7 - Handler returns to original process, restarting faulting instruction

#### Handling Page Fault 

- Page miss causes page fault (an exception)
![[Screen Shot 2024-06-18 at 14.48.14.png]]
- Page miss causes page fault (an exception) 
- Page fault handler selects a victim to be evicted (here VP 4)
![[Screen Shot 2024-06-19 at 20.30.58.png]]
![[Screen Shot 2024-06-19 at 20.31.53.png]]
- Offending instruction is restarted: page hit!
- Key point: Waiting until the miss to copy the page to DRAM is known as demand paging

### Allocating Pages

- Allocating a new page (VP 5) of virtual memory.
![[Screen Shot 2024-06-19 at 20.49.01.png]]

## VM as a Tool

### VM as a Tool for Memory Protection 

- Extend PTEs with permission bits 
- MMU checks these bits on each access

![[Screen Shot 2024-06-19 at 20.52.35.png]]

### VM is also Caching

- Programs allocate virtual address ranges 
	- Implicitly via binaries / libraries 
	- Explicitly through heap / stack 
- The operating system decides which virtual pages should be resident (i.e., in physical memory) 
	- OS manages the placement / replacement policies between DRAM and disk

### VM as a Tool for Caching

- Conceptually, virtual memory is an array of N contiguous bytes stored on disk. 
- The contents of the array on disk are cached in physical memory (DRAM cache) 
	- These cache blocks are called pages (size is $P = 2^p$ bytes)

![[Screen Shot 2024-06-20 at 01.07.56.png]]

### DRAM Cache Organization 

- DRAM cache organization driven by the enormous miss penalty 
	- DRAM is about 10x slower than SRAM 
	- Disk is about 10,000x slower than DRAM 
- Consequences 
	- Large page (block) size: typically 4 KB, sometimes 4 MB 
	- Fully associative 
		- Any VP can be placed in any PP 
		- Requires a “large” mapping function – different from cache memories 
	- Highly sophisticated, expensive replacement algorithms 
		- Too complicated and open-ended to be implemented in hardware 
	- Write-back rather than write-through

### Integrating VM and Cache

![[Screen Shot 2024-06-20 at 01.14.05.png]]

### Locality to the Rescue Again

- Virtual memory seems terribly inefficient, but it works because of locality. 
- At any point in time, programs tend to access a set of active virtual pages called the working set 
	- Programs with better temporal locality will have smaller working sets 
- If (working set size < main memory size) 
	- Good performance for one process after compulsory misses 
- If ( SUM(working set sizes) > main memory size ) 
	- Thrashing: Performance meltdown where pages are swapped (copied) in and out continuously

## Speeding up Translation with a TLB 

- Page table entries (PTEs) are cached in L1 like any other memory word 
	- PTEs may be evicted by other data references 
	- PTE hit still requires a small L1 delay 
- Solution: Translation Lookaside Buffer (TLB) 
	- Small set-associative hardware cache in MMU 
	- Maps virtual page numbers to physical page numbers 
	- Contains complete page table entries for small number of pages

### Accessing the TLB

![[Screen Shot 2024-06-20 at 03.57.32.png]]

### TLB Hit
![[Screen Shot 2024-06-20 at 03.59.51.png]]
### TLB Miss
![[Screen Shot 2024-06-20 at 04.00.19.png]]

### Summary of Address Translation Symbols 

- Basic Parameters 
	- $N = 2^n$ : Number of addresses in virtual address space 
	- $M = 2^m$ : Number of addresses in physical address space 
	- $P = 2^p$ : Page size (bytes) 
- Components of the virtual address (VA) 
	- TLBI: TLB index 
	- TLBT: TLB tag 
	- VPO: Virtual page offset 
	- VPN: Virtual page number 
- Components of the physical address (PA) 
	- PPO: Physical page offset (same as VPO) 
	- PPN: Physical page number

## Multi-Level Page Tables

- Suppose: 4KB ($2^{12}$) page size, 48-bit address space, 8-byte PTE 
- Problem: Would need a 512 GB page table! ($2^{48} \times 2^{-12} \times 2^3 = 2^{39}$ bytes)
- Common solution: Multi-level page table 
- Example: 2-level page table 
	- Level 1 table: each PTE points to a page table (always memory resident) 
	- Level 2 table: each PTE points to a page (paged in and out like any other data)
![[Screen Shot 2024-06-23 at 00.14.41.png]]

### Translating with a k-level Page Table
![[Screen Shot 2024-06-23 at 00.15.42.png]]

### TLBs and k-level Page Tables 

- TLBs cache the complete virtual to physical mapping 
	- Regardless of the levels of page tables, the TLB stores the VPN -> PPN

## Summary

- Programmer’s view of virtual memory 
	- Each process has its own private linear address space 
	- Cannot be corrupted by other processes 
- System view of virtual memory 
	- Uses memory efficiently by caching virtual memory pages 
	- Efficient only because of locality 
	- Simplifies memory management and programming 
	- Simplifies protection by providing a convenient interpositioning point to check permissions
- Memory Accesses with VM ![[Screen Shot 2024-06-23 at 00.20.55.png]]
- the problem with k-level page tables ![[Screen Shot 2024-06-23 at 00.24.19.png]]

## Page Faults

### OS tracks VM “areas”
![[Screen Shot 2024-06-23 at 00.32.25.png]]
### Types of Faults

- This is a legal address 
	- Hard / Major faults – “normal” page faults 
	- Soft / minor faults – the OS took the page away, but has not reused it 
- The address is legal, but … 
	- The type of access is wrong, so “protection exception” 
- The address is not legal 
	- Segmentation fault or bus error
## Concrete examples of virtual memory systems 

### “Simple memory system” from CSAPP 9.6.4 

- Addressing 
	- 14-bit virtual addresses 
	- 12-bit physical address 
	- Page size = 64 bytes ![[Screen Shot 2024-06-23 at 00.38.30.png]]
- Simple Memory System TLB ![[Screen Shot 2024-06-23 at 00.39.25.png]]
- Simple Memory System Page Table ![[Screen Shot 2024-06-23 at 01.01.05.png]]
- Simple Memory System Cache ![[Screen Shot 2024-06-23 at 01.04.32.png]]
- Address Translation Example ![[Screen Shot 2024-06-23 at 01.06.21.png]]
### Intel Core i7 Memory System
![[Screen Shot 2024-06-23 at 01.19.17.png]]
### End-to-end Core i7 Address Translation

![[Screen Shot 2024-06-23 at 01.20.42.png]]

### Core i7 Level 1-3 Page Table Entries

![[Screen Shot 2024-06-23 at 01.26.35.png]]
### Core i7 Level 4 Page Table Entries

![[Screen Shot 2024-06-23 at 01.26.45.png]]
### Core i7 Page Table Translation

![[Screen Shot 2024-06-23 at 01.26.56.png]]
### Cute Trick for Speeding Up L1 Access
![[Screen Shot 2024-06-23 at 01.27.09.png]]
## Nifty things virtual memory makes possible 

### Memory-mapped files (RAM as cache for disk) 

- Paging = every page of a program’s physical RAM is backed by some page of disk* 
- Normally, those pages belong to **swap space** (area on disk used for storing inactive pages of memory)
- But what if some pages were backed by files?
	- This setup allows programs to treat file data as if it is part of the main memory, facilitating faster access and manipulation because it avoids the need for read and write system calls.
- * This is how it used to work 20 years ago. Nowadays, not always true.

![[Screen Shot 2024-06-23 at 02.17.27.png]]
![[Screen Shot 2024-06-23 at 02.18.03.png]]
### Copy-on-write sharing

- **Forking and Memory Duplication**: `fork` creates a new process by copying the entire address space of the parent process 
	- It is slow ![[Screen Shot 2024-06-23 at 03.40.30.png]]
- **Copy-on-Write Mechanism**:
	- Just duplicate the page tables 
	- Mark everything read only 
	- Copy only on write faults ![[Screen Shot 2024-06-23 at 02.19.56.png]] ![[Screen Shot 2024-06-23 at 02.20.08.png]]
	- **Initial Setup:** To optimize, the operating system uses a method called copy-on-write. Upon forking, the child process initially shares the same memory pages as the parent, with all pages marked as read-only.
	- **Modification Trigger:** If either the parent or the child tries to modify any of these shared pages, the operating system detects that (due to the page being read-only), and only then makes a copy of that specific page for the process doing the modification.
	- **Result:** This means no memory is copied unnecessarily. Copies are only made when modifications occur, drastically reducing the amount of duplicated memory and enhancing the system's efficiency.

## Summary

- Multi-level page tables reduce total memory consumption of page tables 
- Translation lookaside buffers reduce time cost of translation 
- Real systems have 3 to 5 levels of page table 
- Virtual memory makes nifty things possible 
	- Memory protection and process isolation 
	- Paging/swapping (disk as extra RAM) 
	- Memory-mapped files (RAM as cache for disk) 
	- Copy-on-write sharing