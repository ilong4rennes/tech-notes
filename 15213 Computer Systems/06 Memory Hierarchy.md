# The memory abstraction 

Modern Connection between CPU and Memory: ![[Screen Shot 2024-06-09 at 23.24.01.png]]
Memory Read Transaction: Load operation: `movq A, %rax`
Memory Write Transaction: Store operation: `movq %rax, A`

# RAM : main memory building block 

**Volatile memories**

| .              | SRAM  | DRAM        |
| -------------- | ----- | ----------- |
| Trans. per-bit | 6     | 1           |
| Access time    | 1x    | 10x         |
| Needs refresh  | No    | Yes         |
| Cost           | 100x  | 1x          |
| Applications   | Cache | Main memory |
| Volatile       | Yes   | Yes         |

<ins>SRAM</ins>: Each bit is stored in a bistable memory cell that can stay in either of two voltage configurations as long as it is powered.

<ins>DRAM</ins>: Each bit is stored as charge on a capacitor. Memory system periodically refreshes every bit of memory by reading and rewriting it.

# Locality of reference 

- **Principle of Locality**: Many Programs tend to use data and instructions with addresses near or equal to those they have used recently. 
- **Temporal locality**: Recently referenced items are likely to be referenced again in the near future 
- **Spatial locality**: Items with nearby addresses tend to be referenced close together in time

# The memory hierarchy 

![[Pasted image 20240609234057.png]]
- **Cache**: A smaller, faster storage device that acts as a staging area for a subset of the data in a larger, slower device.
- Fundamental idea of a memory hierarchy: For each k, the faster, smaller device at level k serves as a cache for the larger, slower device at level k+1. 
- Why do memory hierarchies work? 
	- Because of locality: programs tend to access the data at level k more often than they access the data at level k+1. 
	- Thus, the storage at level k+1 can be slower, and thus larger and cheaper per bit. 
- Big Idea (Ideal): The memory hierarchy creates a large pool of storage that costs as much as the cheap storage near the bottom, but that serves data to programs at the rate of the fast storage near the top.

## Cache Concepts

![[Screen Shot 2024-06-10 at 02.16.12.png]]
**Hit**![[Screen Shot 2024-06-10 at 02.16.34.png]]
**Miss** ![[Screen Shot 2024-06-10 at 02.16.57.png]]
**Types of cache misses**
- <ins>Cold miss</ins>: Cache is initially empty.
- <ins>Capacity miss</ins>: Occurs when active cache blocks (working set) is larger than the cache.
- <ins>Conflict miss</ins>: 
	- Most caches limit blocks at level k+1 to a small subset (sometimes a singleton) of the block positions at level k. 
		- E.g. Block i at level k+1 must be placed in block (i mod 4) at level k. 
	- Conflict misses occur when the level k cache is large enough, but multiple data objects all map to the same level k block. 
		- E.g. Referencing blocks 0, 8, 0, 8, 0, 8, ... would miss every time.

# Storage technologies and trends

**Magnetic Disks**
- Disks consist of platters, each with two surfaces. 
- Each surface consists of concentric rings called tracks. 
- Each track consists of sectors separated by gaps.

**Nonvolatile memories**

<ins>ROM</ins>: Read-only memory.

<ins>PROM</ins>: Can be programmed exactly once.

<ins>EPROM</ins>: Can be bulk cleared to zero and rewritten. Requires a separate device to write _ones_.

<ins>EEPROM</ins>: Can be bulk cleared to zero and rewritten in-place without a separate device.

<ins>Flash memory</ins>: Based on EEPROMs. Partial erase capability which wears out over time.

