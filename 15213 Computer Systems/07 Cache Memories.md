# Cache memory organization and operation 

- Cache memories are small, fast SRAM-based memories managed automatically in hardware
	- Hold frequently accessed blocks of main memory 
- CPU looks first for data in cache
![[Screen Shot 2024-06-10 at 02.59.35.png]]
## General Cache Organization

![[Pasted image 20240610030113.png]]
$\text{Cache Size} = S \times E \times B \text{ data bytes}$ 
## Cache Read

1. Locate set (set index)
2. Check if any line in set has matching tag (tag)
3. Yes + line valid: hit 
4. Locate data starting at offset

No match or not valid (= miss): 
- One line in set is selected for eviction and replacement 
- Replacement policies: random, least recently used (LRU) - lowest locality, …

## Cache Write

- **Write Hit**
    
    When we write a word _w_ that is already cached (**write hit**), first **the cache updates its copy of** **_w_**_._ Then, cache can do 2 things about updating the copy of _w_ in **the next lower level cache** :
    
    - **Write-through**
	    - write immediately to memory
	    - Advantage : Being simple
	    - Disadvantage : causing bus traffic **with every write**.
    - **Write-back**
	    - Write-back defers the update as long as possible by writing the updated block to the next lower level **only when it is evicted from the cache by the replacement algorithm**.
	    - defer write to memory until replacement of line
	    - Advantage : **Reducing** the amount of bus traffic.
	    - Disadvantage : **Additional complexity** - the cache must maintain an additional **dirty bit** for each cache line that **indicates whether or not the cache block has been modified**.

- **Write Miss**
    
    - **Write-allocate**
	    - load into cache, update line in cache
	    - **Write-back** caches are typically write-allocate.
	    - Advantage : Can exploit **spatial locality** of writes
	    - Disadvantage : **Every miss results in a block transfer** from the next lower level to the cache.
    - **No-write-allocate**
	    - writes straight to memory, does not load into cache
	    - **Write-through** caches are typically no-write-allocate.

To write cache-friendly programs, **assume the write-back, write-allocate caches**. - **Modern** computers generally uses **write-back caches** **at all levels**, and it is **symmetric to the way reads are handled**, in that it tries to exploit **locality**.

**Practical Write-back Write-allocate**
![[Screen Shot 2024-06-10 at 03.58.09.png]]
- A write to address X is issued 
- If it is a hit 
	- Update the contents of block 
	- Set dirty bit to 1 (bit is sticky and only cleared on eviction) 
- If it is a miss 
	- Fetch block from memory (per a read miss) 
	- The perform the write operations (per a write hit) 
- If a line is evicted and dirty bit is set to 1 
	- The entire block of $2^b$ bytes are written back to memory 
	- Dirty bit is cleared (set to 0) 
	- Line is replaced by new contents

## Intel Core i7 Cache Hierarchy

![[Screen Shot 2024-06-10 at 04.09.06.png]]

## Cache Performance Metrics

- Miss Rate 
	- Fraction of memory references not found in cache (misses / accesses) = 1 – hit rate 
	- Typical numbers (in percentages): ▪ 3-10% for L1 ▪ can be quite small (e.g., < 1%) for L2, depending on size, etc. 
- Hit Time 
	- Time to deliver a line in the cache to the processor 
		- includes time to determine whether the line is in the cache 
	- Typical numbers: ▪ 4 clock cycle for L1 ▪ 10 clock cycles for L2 
- Miss Penalty 
	- Additional time required because of a miss 
		- typically 50-200 cycles for main memory (Trend: increasing!)

- Average access time = HT + MR * MP
	- 99% hits is twice as good as 97%
	- This is why “miss rate” is used instead of “hit rate”
- Cache friendly code: <5% miss rate
	- Make the common case go fast 
		- Focus on the inner loops of the core functions 
	- Minimize the misses in the inner loops 
		- Repeated references to variables are good (temporal locality) 
		- Stride-1 reference patterns are good (spatial locality)
# Performance impact of caches 
## The memory mountain 

- Read throughput (read bandwidth): Number of bytes read from memory per second (MB/s)
- Memory mountain: Measured read throughput as a function of spatial and temporal locality.
![[Screen Shot 2024-06-10 at 04.55.11.png]]

## Rearranging loops to improve spatial locality 

**Matrix Multiplication** ![[Screen Shot 2024-06-10 at 05.09.14.png]]
**Miss rate analysis**
- Assume: 
	- Block size = 32B (big enough for four doubles) 
	- Matrix dimension (N) is very large: Approximate 1/N as 0.0
	- Cache is not even big enough to hold multiple rows

**Layout of C Arrays in Memory**
- C arrays allocated in row-major order 
	- each row in contiguous memory locations 
	- `a[i][j] = a[i * N + j] `where N is the number of columns 
- Stepping through columns in one row: 
	- `for (i = 0; i < N; i++) sum += a[0][i]; `
	- accesses successive elements 
	- if block size (B) > sizeof($a_{ij}$) bytes, exploit spatial locality: **miss rate = sizeof($a_{ij}$) / B** 
- Stepping through rows in one column: 
	- `for (i = 0; i < n; i++) sum += a[i][0]; `
	- accesses distant elements 
	- no spatial locality! **miss rate = 1 (i.e. 100%)**

![[Screen Shot 2024-06-11 at 16.29.31.png]]
## Using blocking to improve temporal locality
### Matrix Multiplication
![[Screen Shot 2024-06-11 at 16.49.04.png]]

**Cache Miss Analysis**
- Assume: 
	- Matrix elements are doubles 
	- Cache block = 8 doubles 
	- Cache size C << n (much smaller than n) 
- First iteration: n/8 + n = 9n/8 misses
- Second iteration: n/8 + n = 9n/8 misses 
- Total misses: $9n/8 \times n^2 = (9/8) n^3$ 

### Blocked Matrix Multiplication
![[Screen Shot 2024-06-11 at 16.51.30.png]]

**Cache Miss Analysis**

- Assume: 
	- Cache block = 8 doubles 
	- Cache size C << n (much smaller than n) 
	- Three blocks fit into cache: $3B^2 < C$
- First (block) iteration: 
	- $B\times B/8$ misses for each block 
	- $2n/B \times B^2 /8 = n\cdot B/4$ (omitting matrix c)
	- Second (block) iteration: $2n/B x B^2 /8 = n\cdot B/4$
- Total misses: $n\cdot B/4 \times (n/B)^2 = n^3 /(4B)$

### Blocking Summary 

- No blocking: $(9/8) n^3$ misses 
- Blocking: $(1/(4B)) n^3$ misses 
- Use largest block size $B$, such that $B$ satisfies $3B^2 < C$
	- Fit three blocks in cache! Two input, one output. 
- Reason for dramatic difference: 
	- Matrix multiplication has inherent temporal locality: 
		- Input data: $3n^2$ , computation $2n^3$
		- Every array elements used O(n) times! 
	- But program has to be written properly