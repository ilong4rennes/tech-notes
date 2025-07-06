# Unix I/O 

## Unix I/O Overview 

- A file is a sequence of bytes: $B_0 , B_1 , .... , B_k , .... , B_{m-1}$
- Cool fact: All I/O devices are represented as files: 
	- `/dev/sda2` (disk partition) 
	- `/dev/tty2` (terminal) 
	- `/dev/null` (discard all writes / read empty file) 
- Cool fact: Kernel data structures are exposed as files 
	- `cat /proc/$$/status` 
	- `ls -l /proc/$$/fd/` 
	- `ls –RC /sys/devices | less`
- Kernel offers a set of basic operations for all files 
	- Opening and closing files 
		- `open()` and `close()` 
	- Reading and writing a file 
		- `read()` and `write()` 
	- Look up information about a file (size, type, last modification time, …) 
		- `stat(), lstat(), fstat()` 
	- Changing the current file position (seek) 
		- indicates next offset into file to read or write 
		- `lseek()` ![[Screen Shot 2024-07-19 at 15.08.40.png]]

## File Types 

- Each file has a type indicating its role in the system 
	- Regular file: Stores arbitrary data 
	- Directory: Index for a related group of files 
	- Socket: For communicating with a process on another machine 
- Other file types beyond our scope 
	- Named pipes (FIFOs) 
	- Symbolic links 
	- Character and block devices

### Regular Files 

- A regular file contains arbitrary data 
- Applications often distinguish between text and binary files 
	- Text files contain human-readable text 
	- Binary files are everything else (object files, JPEG images, …) 
	- Kernel doesn’t care! It’s all just bytes! 
- Text file is sequence of text lines 
	- Text line is sequence of characters terminated (not separated!) by end of line indicator 
	- Characters are defined by a text encoding (ASCII, UTF-8, EUC-JP, …) 
- End of line (EOL) indicators: 
	- All “Unix”: Single byte `0x0A` 
		- line feed (LF) 
	- DOS, Windows: Two bytes `0x0D 0x0A` 
		- Carriage return (CR) followed by line feed (LF) 
		- Also used by many Internet protocols 
	- C library translates to '`\n`'

### Directories 

- Directory consists of an array of entries (also called links) 
	- Each entry maps a filename to a file 
- Each directory contains at least two entries 
	- . (dot) maps to the directory itself 
	- .. (dot dot) maps to the parent directory in the directory hierarchy (next slide) 
- Commands for manipulating directories 
	- `mkdir`: create empty directory 
	- `ls`: view directory contents 
	- `rmdir`: delete empty directory

#### Directory Hierarchy 

- All files are organized as a hierarchy anchored by root directory named / (slash)
![[Screen Shot 2024-07-19 at 15.16.52.png]]
- Kernel maintains current working directory (cwd) for each process 
	- Modified using the cd command

#### Pathnames 

- Locations of files in the hierarchy denoted by pathnames 
	- Absolute pathname starts with ‘/’ and denotes path from root 
		- `/home/droh/hello.c` 
	- Relative pathname denotes path from current working directory 
		- `../droh/hello.c`
![[Screen Shot 2024-07-19 at 15.21.04.png]]

## Opening Files 

- Opening a file informs the kernel that you are getting ready to access that file![[Screen Shot 2024-07-19 at 15.21.23.png]]
- Returns a small identifying integer file descriptor 
	- fd == -1 indicates that an error occurred 
- Each process begins life with three open files 
	- 0: standard input (stdin) 
	- 1: standard output (stdout) 
	- 2: standard error (stderr) 
	- These could be files, pipes, your terminal, or even a network connection!

Lots of ways to call open:
![[Screen Shot 2024-07-19 at 15.22.42.png]]

## Closing Files 

- Closing a file informs the kernel that you are finished accessing that file![[Screen Shot 2024-07-19 at 15.23.18.png]]

- Take care not to close any file more than once 
	- Same as not calling free() twice on the same pointer 
- Closing a file can fail! 
	- Well, not exactly fail—the file is still closed 
	- The OS is taking this opportunity to report a delayed error from a previous write operation 
	- You might silently lose data if you don’t check!

## Reading Files

- Reading a file copies bytes from the current file position to memory, and then updates file position![[Screen Shot 2024-07-19 at 15.35.00.png]]
- Returns number of bytes read from file `fd` into `buf` 
	- Return type `ssize_t` is signed integer 
	- `nbytes < 0` indicates that an error occurred 
	- Short counts (`nbytes < sizeof(buf)`) are possible and are not errors!
## Writing Files

- Writing a file copies bytes from memory to the current file position, and then updates current file position ![[Screen Shot 2024-07-19 at 19.12.30.png]]
- Returns number of bytes written from `buf` to file `fd` 
	- `nbytes < 0` indicates that an error occurred 
	- As with reads, short counts are possible and are not errors!
## Simple Unix I/O example

Copying stdin to stdout, one byte at a time![[Screen Shot 2024-07-19 at 19.34.14.png]]
![[Screen Shot 2024-07-19 at 19.35.18.png]]
## On Short Counts 

- Def: **Short counts** occur when fewer bytes are read or written than requested. This can happen for various reasons in different situations.
- Short counts can occur in these situations: 
	- Encountering (end-of-file) EOF on reads 
	- Reading text lines from a terminal 
	- Reading and writing network sockets, pipes, etc. 
- Short counts never occur in these situations: 
	- Reading from disk files (except for EOF) 
	- Writing to disk files 
- Best practice is to always allow for short counts.

# Standard I/O 

## Standard I/O Functions 

- The C standard library (libc.so) contains a collection of higher-level standard I/O functions 
	- Documented in Appendix B of K&R 
- Examples of standard I/O functions: 
	- Opening and closing files (`fopen` and `fclose`) 
	- Reading and writing bytes (`fread` and `fwrite`) 
	- Reading and writing text lines (`fgets` and `fputs`) 
	- Formatted reading and writing (`fscanf` and `fprintf`)

## Standard I/O Streams 

- Standard I/O models open files as streams 
	- In C programming, when you work with files or input/output operations, they are treated as streams.
	- A stream is an abstraction that includes a file descriptor (a unique identifier for an open file) and a buffer in memory (a temporary storage area for data).
- C programs begin life with three open streams (defined in stdio.h) 
	- `stdin` (standard input): Used for reading input (usually from the keyboard).
	- `stdout` (standard output): Used for writing output (usually to the screen).
	- `stderr` (standard error): Used for writing error messages (also usually to the screen).
![[Screen Shot 2024-07-23 at 16.00.49.png]]

## Buffered I/O: Motivation 

- Applications often read/write one character at a time 
	- `getc, putc, ungetc` 
	- `gets, fgets` 
		- Read line of text one character at a time, stopping at newline 
- Implementing as Unix I/O calls expensive 
	- `read` and `write` require Unix kernel calls 
		- 10,000 clock cycles 
- Solution: Buffered read 
	- Use Unix read to grab block of bytes 
	- User input functions take one byte at a time from buffer 
	- Refill buffer when empty
![[Screen Shot 2024-07-23 at 16.01.07.png]]

## Buffering in Standard I/O 

- Standard I/O functions use buffered I/O
![[Screen Shot 2024-07-23 at 16.01.25.png]]
- The buffer is flushed to the output file descriptor (fd) when a newline character `\n` is encountered.
- It can also be flushed by calling `fflush`, exiting the program, or returning from the `main` function.
- The `write(1, buf, 6);` line shows how the buffer contents are written to the output. Here, `1` is the file descriptor for standard output (stdout), `buf` is the buffer, and `6` is the number of bytes to write.

## Standard I/O Buffering in Action 

- You can see this buffering in action for yourself, using the always fascinating Linux `strace` program:
![[Screen Shot 2024-07-23 at 16.01.50.png]]
# Which I/O when 

## Pros and Cons of Unix I/O 

- Pros 
	- Unix I/O is the most general form of I/O 
		- All other I/O packages are implemented using Unix I/O functions 
	- Unix I/O provides functions for accessing file metadata 
	- Unix I/O functions are async-signal-safe and can be used safely in signal handlers 
- Cons 
	- Dealing with short counts is tricky and error prone 
	- Efficient reading of text lines requires some form of buffering, also tricky and error prone

## Pros and Cons of Standard I/O 

- Pros: 
	- Buffering increases efficiency by decreasing the number of read and write system calls 
	- Short counts are handled automatically 
- Cons: 
	- Provides no function for accessing file metadata 
	- Standard I/O functions are not async-signal-safe, and not appropriate for signal handlers 
	- Standard I/O is not appropriate for input and output on network sockets 
		- There are poorly documented restrictions on streams that interact badly with restrictions on sockets (CS:APP3e, Sec 10.11)

## Choosing I/O Functions 

- General rule: use the highest-level I/O functions you can 
	- Many C programmers are able to do all of their work using the standard I/O functions 
	- But, be sure to understand the functions you use! 
- When to use standard I/O 
	- When working with “ordinary” files 
- When to use raw Unix I/O 
	- Inside signal handlers, because Unix I/O is async-signal-safe 
	- When you are reading and writing network sockets 
		- Libraries dedicated to buffered network I/O make this easier 
		- CS:APP `rio_*` functions; libevent, libuv, … 
	- In rare cases when you need absolute highest performance

## Aside: Working with Binary Files 

- Functions you should never use on binary files 
	- Text-oriented I/O: such as `fgets, scanf, rio_readlineb` 
		- Interpret EOL characters. 
		- Use functions like `rio_readn` or `rio_readnb` instead 
	- String functions 
		- `strlen, strcpy, strcat` 
		- Interprets byte value 0 (end of string) as special

# Metadata, sharing, and redirection

## File Metadata 

- Metadata is data about data, in this case file data 
- Per-file metadata maintained by kernel 
	- accessed by users with the `stat` and `fstat` functions
![[Screen Shot 2024-07-23 at 18.29.30.png]]

## How the Unix Kernel Represents Open Files 

- Two descriptors referencing two distinct open files. Descriptor 1 (stdout) points to terminal, and descriptor 4 points to open disk file
![[Screen Shot 2024-07-23 at 18.29.41.png]]

## File Sharing 

- Two distinct descriptors sharing the same disk file through two distinct open file table entries 
	- E.g., Calling `open` twice with the same `filename` argument
![[Screen Shot 2024-07-23 at 18.30.16.png]]

## How Processes Share Files: `fork` 

- A child process inherits its parent’s open files 
	- Note: situation unchanged by `exec` functions (use `fcntl` to change) 
- Before `fork` call:
![[Screen Shot 2024-07-23 at 18.30.33.png]]

- A child process inherits its parent’s open files 
- After fork: 
	- Child’s table same as parent’s, and +1 to each `refcnt`
![[Screen Shot 2024-07-23 at 18.31.12.png]]

## I/O Redirection 

- Question: How does a shell implement I/O redirection? `linux> ls > foo.txt `
- Answer: By calling the `dup2(oldfd, newfd)` function 
	- Copies (per-process) descriptor table entry `oldfd` to entry `newfd`
![[Screen Shot 2024-07-23 at 19.17.11.png]]

## I/O Redirection Example 

- Step #1: open file to which stdout should be redirected 
	- Happens in child executing shell code, before `exec` ![[Screen Shot 2024-07-23 at 19.17.57.png]]
- Step #2: call `dup2(4,1)` 
	- cause fd=1 (stdout) to refer to disk file pointed at by fd=4![[Screen Shot 2024-07-23 at 19.22.43.png]]

## Warm-Up: I/O and Redirection Example
![[Screen Shot 2024-07-23 at 19.22.57.png]]
## Master Class: Process Control and I/O
![[Screen Shot 2024-07-23 at 19.23.20.png]]