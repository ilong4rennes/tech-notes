# Basic concepts 

## Dynamic Memory Allocation

- Programmers use dynamic memory allocators (such as malloc) to acquire virtual memory (VM) at runtime 
	- For data structures whose size is only known at runtime 
- Dynamic memory allocators manage an area of process VM known as the heap
![[Screen Shot 2024-06-20 at 12.03.22.png]]

- Allocator maintains heap as collection of variable sized blocks, which are either allocated or free 
- Types of allocators 
	- Explicit allocator: application allocates and frees space 
		- e.g., malloc and free in C 
	- Implicit allocator: application allocates, but does not free space 
		- e.g., new and garbage collection in Java

### `malloc`
![[Screen Shot 2024-06-20 at 12.05.42.png]]
![[Screen Shot 2024-06-20 at 12.06.42.png]]
### Heap Visualization Convention

- 1 square = 1 “word” = 8 bytes
![[Screen Shot 2024-06-23 at 05.10.48.png]]
![[Screen Shot 2024-06-23 at 05.11.34.png]]
### Constraints

- Applications 
	- Can issue arbitrary sequence of `malloc` and `free` requests 
	- `free` request must be to a `malloc`’d block 
- Explicit Allocators 
	- Can’t control number or size of allocated blocks 
	- Must respond immediately to malloc requests 
		- i.e., can’t reorder or buffer requests 
	- Must allocate blocks from free memory 
		- i.e., can only place allocated blocks in free memory 
	- Must align blocks so they satisfy all alignment requirements 
		- 16-byte (x86-64) alignment on 64-bit systems 
	- Can manipulate and modify only free memory 
	- Can’t move the allocated blocks once they are `malloc`’d 
		- i.e., compaction is not allowed. Why not?

## Performance Goal

### Maximize Throughput

- Given some sequence of `malloc` and `free` requests: $R_0, R_1, ..., R_k, ..., R_{n-1}$
- Goals: maximize throughput and peak memory utilization 
	- These goals are often conflicting 
- Throughput: 
	- Number of completed requests per unit time 
	- Example: 5,000 `malloc` calls and 5,000 `free` calls in 10 seconds 
	- Throughput is 1,000 operations/second

### Minimize Overhead

- Given some sequence of `malloc` and `free` requests: $R_0, R_1, ..., R_k, ..., R_{n-1}$
- After $k$ requests we have: 
- Def: **Aggregate payload $P_k$** 
	- `malloc(p)` results in a block with a *payload* of p bytes 
	- The *aggregate payload* $P_k$ is the sum of currently allocated payloads 
	- The *peak aggregate payload* $\max\limits_{i \leq k} P_i$ is the maximum aggregate payload at any point in the sequence up to request 
- Def: **Current heap size $H_k$**
	- Assume heap only grows when allocator uses `sbrk`, never shrinks 
- Def: **Overhead, $O_k$**
	- Fraction of heap space NOT used for program data 
	- $O_k = \left( {H_k}/{\max\limits_{i \leq k} P_i} \right) - 1.0$

### Benchmark Example

![[Screen Shot 2024-06-23 at 05.46.17.png]] ![[Screen Shot 2024-06-23 at 05.46.46.png]]
### Typical Benchmark Behavior

![[Screen Shot 2024-06-23 at 05.49.03.png]]

## Fragmentation

- Poor memory utilization caused by fragmentation 
	- Internal fragmentation 
	- External fragmentation

### Internal Fragmentation

1. **Definition:**
   - Internal fragmentation occurs when the memory allocated is larger than the memory requested. For example, if a program needs 100 bytes but gets 120 bytes, the extra 20 bytes represent internal fragmentation.
   - For a given block, **internal fragmentation occurs if payload is smaller than block size** ![[Screen Shot 2024-06-23 at 06.32.44.png]]

2. **Diagram Explanation:**
   - The diagram shows a block of memory, divided into three parts:
     - **Internal Fragmentation**: Unused space at the beginning and end of the block.
     - **Payload**: The actual memory used by the program.

3. **Causes:**
   - **Overhead of maintaining heap data structures**: Managing memory involves some extra space or resources, which can lead to unused memory within allocated blocks.
   - **Padding for alignment purposes**: Memory is often aligned to certain boundaries for faster access, which can leave small gaps.
   - **Explicit policy decisions**: Sometimes, memory management systems intentionally allocate larger blocks than needed for various reasons, such as anticipating future requests.

4. **Dependency:**
   - Depends on the pattern of **previous** memory requests
	   - easy to measure

![[Screen Shot 2024-06-23 at 06.38.56.png]]

### External Fragmentation

- Occurs when there is enough aggregate heap memory, but no single free block is large enough
![[Screen Shot 2024-06-23 at 06.41.38.png]]
- Depends on the pattern of future requests 
	- difficult to measure

![[Screen Shot 2024-06-23 at 06.42.20.png]]

### Implementation Issues 

- How do we know how much memory to free given just a pointer? 
- How do we keep track of the free blocks? 
- What do we do with the extra space when allocating a structure that is smaller than the free block it is placed in? 
- How do we pick a block to use for allocation -- many might fit? 
- How do we reuse a block that has been freed?

# Keeping Track of Free Blocks

Standard Method of knowing how much to free
- Keep the length (in bytes) of a block in the word preceding the block. 
	- Including the header 
	- This word is often called the header field or header 
- Requires an extra word for every allocated block
![[Screen Shot 2024-06-23 at 07.02.30.png]]

![[Screen Shot 2024-06-23 at 06.59.59.png]]
## Method 1: Implicit free lists

- For each block we need both size and allocation status 
	- Could store this information in two words: wasteful! 
- Standard trick 
	- When blocks are aligned, some low-order address bits are always 0 
	- Instead of storing an always-0 bit, use it as an allocated/free flag 
	- When reading the Size word, must mask out this bit
![[Screen Shot 2024-06-23 at 07.20.25.png]]

### Detailed Implicit Free List Example

![[Screen Shot 2024-06-23 at 11.03.27.png]]

### Implicit List: Data Structures
![[Screen Shot 2024-06-23 at 18.16.09.png]]
### Implicit List: Header access
![[Screen Shot 2024-06-23 at 18.16.33.png]]
### Implicit List: Traversing list
![[Screen Shot 2024-06-23 at 18.32.54.png]]
### Implicit List: Finding a Free Block
![[Screen Shot 2024-06-23 at 18.34.24.png]]
- First fit: 
	- Search list from beginning, choose **first** free block that fits: 
	- Can take linear time in total number of blocks (allocated and free) 
	- In practice it can cause “splinters” at beginning of list 
- Next fit: 
	- Like first fit, but search list starting where previous search finished 
	- Should often be faster than first fit: avoids re-scanning unhelpful blocks 
	- Some research suggests that fragmentation is worse 
- Best fit: 
	- Search the list, choose the best **free** block: fits, with fewest bytes left over 
	- Keeps fragments small—usually improves memory utilization 
	- Will typically run slower than first fit 
	- Still a greedy algorithm. No guarantee of optimality
#### Comparing Strategies
![[Screen Shot 2024-06-23 at 18.44.59.png]]
### Implicit List: Allocating in Free Block

- Allocating in a free block: splitting 
	- Since allocated space might be smaller than free space, we might want to split the block
![[Screen Shot 2024-06-23 at 18.46.34.png]]

#### Splitting Free Block

![[Screen Shot 2024-06-23 at 18.57.58.png]]
### Implicit List: Freeing a Block

- Simplest implementation: 
	- Need only clear the “allocated” flag 
	- But can lead to “false fragmentation”
![[Screen Shot 2024-06-23 at 19.00.45.png]]
#### Coalescing

- Join (coalesce) with next/previous blocks, if they are free 
	- Coalescing with next block

![[Screen Shot 2024-06-23 at 19.01.50.png]]
- How do we coalesce with previous block? 
	- How do we know where it starts? 
	- How can we determine whether its allocated?
#### Bidirectional Coalescing

- Boundary tags - Knuth73 
	- Replicate size/allocated word at “bottom” (end) of free blocks 
	- Allows us to traverse the “list” backwards, but requires extra space 
	- Important and general technique
![[Screen Shot 2024-06-23 at 19.17.02.png]]
#### Implementation with Footers
![[Screen Shot 2024-06-23 at 19.19.59.png]]
![[Screen Shot 2024-06-23 at 19.20.39.png]]
### Splitting Free Block: Full Version

![[Screen Shot 2024-06-23 at 22.20.10.png]]
### Constant Time Coalescing
![[Screen Shot 2024-06-23 at 19.19.37.png]]
#### Case 1
![[Screen Shot 2024-06-23 at 22.24.53.png]]
#### Case 2
![[Screen Shot 2024-06-23 at 22.25.26.png]]
#### Case 3
![[Screen Shot 2024-06-23 at 22.25.38.png]]
#### Case 4
![[Screen Shot 2024-06-23 at 22.25.58.png]]
### Heap Structure
![[Screen Shot 2024-06-23 at 22.29.14.png]]
- Dummy footer before first header 
	- Marked as allocated 
	- Prevents accidental coalescing when freeing first block 
- Dummy header after last footer 
	- Prevents accidental coalescing when freeing final block
### Top-Level Malloc Code
![[Screen Shot 2024-06-23 at 22.32.18.png]]
### Top-Level Free Code
![[Screen Shot 2024-06-23 at 22.33.10.png]]
### Disadvantages of Boundary Tags

- Internal fragmentation 
- Can it be optimized? 
	- Which blocks need the footer tag? 
	- What does that mean? Only need footer in free block
### No Boundary Tag for Allocated Blocks

- Boundary tag needed only for free blocks 
- When sizes are multiples of 16, have 4 spare bits
![[Screen Shot 2024-06-23 at 23.08.34.png]]
#### Case 1
![[Screen Shot 2024-06-23 at 23.10.01.png]]
#### Case 2
![[Screen Shot 2024-06-23 at 23.12.57.png]]
#### Case 3
![[Screen Shot 2024-06-23 at 23.13.10.png]]
#### Case 4
![[Screen Shot 2024-06-23 at 23.13.24.png]]

### Summary of Key Allocator Policies

- Placement policy: 
	- First-fit, next-fit, best-fit, etc. 
	- Trades off lower throughput for less fragmentation 
	- Interesting observation: segregated free lists (next lecture) approximate a best fit placement policy without having to search entire free list 
- Splitting policy: 
	- When do we go ahead and split free blocks? 
	- How much internal fragmentation are we willing to tolerate? 
- Coalescing policy: 
	- Immediate coalescing: coalesce each time free is called 
	- Deferred coalescing: try to improve performance of free by deferring coalescing until needed.
### Summary of Implicit Lists

- Implementation: very simple 
- Allocate cost: 
	- linear time worst case 
- Free cost: 
	- constant time worst case 
	- even with coalescing 
- Memory Overhead 
	- will depend on placement policy 
	- First-fit, next-fit or best-fit 
- Not used in practice for malloc/free because of lineartime allocation 
	- used in many special purpose applications 
- However, the concepts of splitting and boundary tag coalescing are general to all allocators

## Method 2: Explicit free lists 

- Maintain list(s) of free blocks, not all blocks 
	- Luckily we track only free blocks, so we can use payload area 
	- The “next” free block could be anywhere 
		- So we need to store forward/back pointers, not just sizes 
	- Still need boundary tags for coalescing 
		- To find adjacent blocks according to memory order
![[Screen Shot 2024-06-24 at 00.29.50.png]]
![[Screen Shot 2024-06-24 at 00.30.37.png]]
### Allocating From Explicit Free Lists

![[Screen Shot 2024-06-24 at 01.07.36.png]]
### Freeing With Explicit Free Lists

- Insertion policy: Where in the free list do you put a newly freed block? 
- Unordered 
	- LIFO (last-in-first-out) policy 
		- Insert freed block at the beginning of the free list 
	- FIFO (first-in-first-out) policy 
		- Insert freed block at the end of the free list 
	- Pro: simple and constant time 
	- Con: studies suggest fragmentation is worse than address ordered 
- Address-ordered policy 
	- Insert freed blocks so that free list blocks are always in address order: 
		- addr(prev) < addr(curr) < addr(next) 
	- Con: requires search 
	- Pro: studies suggest fragmentation is lower than LIFO/FIFO

### Freeing With a LIFO Policy

#### Case 1
![[Screen Shot 2024-06-24 at 01.23.16.png]]
#### Case 2
![[Screen Shot 2024-06-24 at 01.28.01.png]]
#### Case 3
![[Screen Shot 2024-06-24 at 01.38.42.png]]
#### Case 4
![[Screen Shot 2024-06-24 at 01.39.22.png]]
### Summary

- Comparison to implicit list: 
	- Allocate is linear time in number of free blocks instead of all blocks 
		- Much faster when most of the memory is full 
	- Slightly more complicated allocate and free 
		- Need to splice blocks in and out of the list 
	- Some extra space for the links (2 extra words needed for each block) 
		- Does this increase internal fragmentation?

## Method 3: Segregated free lists 

- Have several free lists, one for each **size class** of blocks ![[Screen Shot 2024-06-24 at 01.49.54.png]]
- Which blocks go in which size classes is a design decision 
	- Can have major impact on both utilization and throughput 
	- Common choices include: 
	- One class for each small size (16, 32, 48, 64, …) 
	- At some point switch to powers of two: $[2^i+1, 2^{i+1}]$
- The list for the largest blocks must have no upper limit (well, $2^{64}$)
### Seglist Allocator 

- Given an array of free lists, each one for some size class 
- To allocate a block of size n: 
	- Search appropriate free list for block of size m ≥ n (i.e., first fit) 
	- If an appropriate block is found: ▪ Split block and place fragment on appropriate list ▪ If no block is found, try next larger class 
	- Repeat until block is found 
- If no block is found: 
	- Request additional heap memory from OS (using `sbrk()`) 
	- Allocate block of n bytes from this new memory 
	- Place remainder as a single free block in appropriate size class.
- To free a block: 
	- Coalesce and place on appropriate list 
- Advantages of seglist allocators vs. non-seglist allocators (both with first-fit) 
	- Higher throughput 
		- log time for power-of-two size classes vs. linear time 
	- Better memory utilization 
		- First-fit search of segregated free list approximates a best-fit search of entire heap. 
		- Extreme case: Giving each block its own size class is equivalent to best-fit.
## Memory-related perils and pitfalls

### Dereferencing bad pointers 
![[Screen Shot 2024-06-24 at 02.26.49.png]]
### Reading uninitialized memory 
![[Screen Shot 2024-06-24 at 02.27.13.png]]
### Overwriting memory 
![[Screen Shot 2024-06-24 at 02.27.42.png]]
![[Screen Shot 2024-06-24 at 02.30.16.png]]
![[Screen Shot 2024-06-24 at 02.30.27.png]]
![[Screen Shot 2024-06-24 at 02.31.20.png]]
![[Screen Shot 2024-06-24 at 02.32.24.png]]
### Referencing nonexistent variables 
![[Screen Shot 2024-06-24 at 02.32.43.png]]
### Freeing blocks multiple times 
![[Screen Shot 2024-06-24 at 02.32.58.png]]
### Referencing freed blocks 
![[Screen Shot 2024-06-24 at 02.33.13.png]]
### Failing to free blocks
![[Screen Shot 2024-06-24 at 02.33.40.png]]
![[Screen Shot 2024-06-24 at 02.35.09.png]]
### Dealing With Memory Bugs 

- Debugger: gdb 
	- Good for finding bad pointer dereferences 
	- Hard to detect the other memory bugs 
- Data structure consistency checker 
	- Runs silently, prints message only on error 
	- Use as a probe to zero in on error 
- Binary translator: valgrind 
	- Powerful debugging and analysis technique 
	- Rewrites text section of executable object file 
	- Checks each individual reference at runtime 
	- Bad pointers, overwrites, refs outside of allocated block 
- glibc malloc contains checking code