# Processes 

### Process

- **Definition:** A process is like a program that is running on your computer.
- **Example:** When you open your web browser, a process starts to handle everything you do in the browser.
- **Lifecycle:**
    - **Start:** When you launch an application, the operating system creates a new process.
    - **Run:** The process executes instructions and performs tasks.
    - **End:** When you close the application, the process is terminated.
### Multiprocessing
![[Screen Shot 2024-07-09 at 15.59.18.png]]
- Computer runs many processes simultaneously 
	- Applications for one or more users 
		- Web browsers, email clients, editors, … 
	- Background tasks 
		- Monitoring network & I/O devices
### Processes
![[Screen Shot 2024-07-09 at 16.02.07.png]]
- Definition: A process is an instance of a running program. 
- Process provides each program with two key abstractions: 
	- Private address space 
		- Each program seems to have exclusive use of main memory. 
		- Provided by kernel mechanism called **virtual memory** 
	- Logical control flow 
		- Each program seems to have exclusive use of the CPU 
		- Provided by kernel mechanism called **context switching**

### Control Flow

- Processors do only one thing: 
	- From startup to shutdown, each CPU core simply reads and executes a sequence of machine instructions, one at a time * 
	- This sequence is the CPU’s control flow (or flow of control)
![[Screen Shot 2024-07-09 at 16.16.57.png]]
### Context Switching
- Processes are managed by a shared chunk of memoryresident OS code called the **kernel** 
	- Important: the kernel is not a separate process, but rather runs as part of some existing process. 
- Control flow passes from one process to another via a **context switch**
![[Screen Shot 2024-07-09 at 16.30.50.png]]
### Context Switching (Uniprocessor)
![[Screen Shot 2024-07-09 at 17.03.41.png]]
- Single processor executes multiple processes concurrently 
	- Process executions interleaved (multitasking) 
	- Address spaces managed by virtual memory system (like last week) 
	- Register values for nonexecuting processes saved in memory
![[Screen Shot 2024-07-09 at 17.07.46.png]]
- Save current registers in memory
![[Screen Shot 2024-07-09 at 17.08.35.png]]
- Schedule next process for execution
![[Screen Shot 2024-07-09 at 17.02.47.png]]
- Load saved registers and switch address space (context switch)

### Context Switching (Multicore)
![[Screen Shot 2024-07-09 at 17.11.23.png]]
### User View of Concurrent Processes

- Concurrent: execution overlaps in time
- Sequential: OW
![[Screen Shot 2024-07-09 at 17.15.43.png]]
### Traditional (Uniprocessor) Reality
![[Screen Shot 2024-07-09 at 17.19.18.png]]
- The CPU executes instructions in sequence
![[Screen Shot 2024-07-09 at 17.20.34.png]]
# System Calls 

### System Call

- **Definition:** A system call is a way for a program to ask the operating system to do something.
- **Example:** If your program needs to read a file from the disk, it uses a system call to ask the operating system to read the file.
- **Process:**
    - **Request:** The program sends a request to the operating system.
    - **Execution:** The operating system performs the requested action.
    - **Response:** The operating system sends the result back to the program.

- Whenever a program wants to cause an effect outside its own process, it must ask the kernel for help 
- Examples: 
	- Read/write files 
	- Get current time 
	- Allocate RAM (sbrk) 
	- Create new processes
![[Screen Shot 2024-07-09 at 17.41.29.png]]
### System Call Error Handling 
- Almost all system-level operations can fail 
	- Only exception is the handful of functions that return void 
	- You must explicitly check for failure 
- On error, most system-level functions return −1 and set global variable errno to indicate cause. 
- Example:![[Screen Shot 2024-07-10 at 07.28.46.png]]
### Error-reporting functions 
- Can simplify somewhat using an error-reporting function:
![[Screen Shot 2024-07-10 at 07.45.13.png]]
- Not always appropriate to exit when something goes wrong.
### Error-handling Wrappers
- We simplify the code we present to you even further by using Stevens-style error-handling wrappers:
![[Screen Shot 2024-07-10 at 07.46.12.png]]
- NOT what you generally want to do in a real application
# Process Control 
### Obtaining Process IDs 
- `pid_t getpid(void)` ▪ Returns PID of current process 
- `pid_t getppid(void)` ▪ Returns PID of parent process
### Process States 
At any time, each process is either: 
- Running 
	- Process is either executing instructions, or it could be executing instructions if there were enough CPU cores. 
- Blocked / Sleeping 
	- Process cannot execute any more instructions until some external event happens (usually I/O). 
- Stopped 
	- Process has been prevented from executing by user action (control-Z). 
- Terminated / Zombie 
	- Process is finished. Parent process has not yet been notified.
### Terminating Processes 
- Process becomes terminated for one of three reasons: 
	- Receiving a signal whose default action is to terminate (next lecture) 
	- Returning from the `main` routine 
	- Calling the `exit` function 
- `void exit(int status)` 
	- Terminates with an exit status of status 
	- Convention: normal return status is 0, nonzero on error 
	- Another way to explicitly set the exit status is to return an integer value from the main routine 
- `exit` is called **once** but **never** returns.
### Creating Processes 
- Parent process creates a new running child process by calling fork 
- int fork(void) 
	- Returns 0 to the child process, child’s PID to parent process 
	- Child is almost identical to parent: 
		- Child get an identical (but separate) copy of the parent’s virtual address space. 
		- Child gets identical copies of the parent’s open file descriptors 
		- Child has a different PID than the parent 
- fork is called once but returns twice
### Conceptual View of `fork`
![[Screen Shot 2024-07-10 at 08.34.17.png]]
- Make complete copy of execution state 
	- Designate one as parent and one as child 
	- Resume execution of parent or child 
	- (Optimization: Use copy-on-write to avoid copying RAM)
### `fork` Example
![[Screen Shot 2024-07-10 at 08.39.11.png]]
### Modeling fork with Process Graphs 
- A process graph is a useful tool for capturing the partial ordering of statements in a concurrent program: 
	- Each vertex is the execution of a statement 
	- a -> b means a happens before b 
	- Edges can be labeled with current value of variables 
	- `printf` vertices can be labeled with output 
	- Each graph begins with a vertex with no inedges 
- Any topological sort of the graph corresponds to a feasible total ordering. 
	- Total ordering of vertices where all edges point from left to right

Example:![[Screen Shot 2024-07-10 at 08.48.40.png]]
![[Screen Shot 2024-07-10 at 08.49.40.png]]
Two consecutive `forks`:
![[Screen Shot 2024-07-10 at 09.16.18.png]]
Nested `forks` in parent:
![[Screen Shot 2024-07-10 at 09.16.36.png]]
Nested `forks` in children:![[Screen Shot 2024-07-10 at 09.43.18.png]]
### Reaping Child Processes
- Idea 
	- When process terminates, it still consumes system resources 
		- Examples: Exit status, various OS tables 
	- Called a “zombie” 
		- Living corpse, half alive and half dead 
- Reaping 
	- Performed by parent on terminated child (using `wait` or `waitpid`) 
	- Parent is given exit status information 
	- Kernel then deletes zombie child process 
- What if parent doesn’t reap? 
	- If any parent terminates without reaping a child, then the orphaned child should be reaped by init process (pid == 1) 
		- Unless it was init that terminated! Then need to reboot… 
	- So, only need explicit reaping in long-running processes 
		- e.g., shells and servers

Zombie Example:![[Screen Shot 2024-07-10 at 10.01.54.png]]

Non-terminating Child Example: ![[Screen Shot 2024-07-10 at 10.02.16.png]]

### `wait`: Synchronizing with Children 
- Parent reaps a child with one of these system calls: 
- `pid_t wait(int *status) `
	- Suspends current process until one of its children terminates 
	- Returns PID of child, records exit status in status 
- `pid_t waitpid(pid_t pid, int *status, int options) `
	- More flexible version of wait: 
	- Can wait for a specific child or group of children 
	- Can be told to return immediately if there are no children to reap
![[Screen Shot 2024-07-10 at 10.13.55.png]]
### `wait`: Status codes
- Return value of `wait` is the pid of the child process that terminated 
- If status != NULL, then the integer it points to will be set to a value that indicates the exit status 
	- More information than the value passed to exit 
	- Must be decoded, using macros defined in sys/wait.h 
		- `WIFEXITED, WEXITSTATUS, WIFSIGNALED, WTERMSIG, WIFSTOPPED, WSTOPSIG, WIFCONTINUED` 
		- See textbook for details

### Another `wait` Example
- If multiple children completed, will take in arbitrary order 
- Can use macros `WIFEXITED` and `WEXITSTATUS` to get information about exit status
![[Screen Shot 2024-07-10 at 10.23.14.png]]
### `execve`: Loading and Running Programs 
- `int execve(char *filename, char *argv[], char *envp[]) `
- Loads and runs in the current process: 
	- Executable file filename 
		- Can be object file or script file beginning with `#!interpreter` (e.g., `#!/bin/bash`) 
	- …with argument list `argv`
		- By convention `argv[0]==filename` 
	- …and environment variable list `envp` 
		- “name=value” strings (e.g., `USER=droh`) 
		- `getenv, putenv, printenv` 
- Overwrites code, data, and stack 
	- Retains PID, open files and signal context 
- Called once and never returns 
	- …except if there is an error
### `execve` Example
![[Screen Shot 2024-07-10 at 11.15.15.png]]
![[Screen Shot 2024-07-10 at 11.15.43.png]]
![[Screen Shot 2024-07-10 at 11.15.57.png]]
# Shells

### Shell

- **Definition:** A shell is a program that allows you to interact with the operating system using commands.
- **Example:** The Command Prompt on Windows or Terminal on macOS and Linux are shells.
- **Process:**
    - **Input:** You type a command into the shell (e.g., `ls` to list files).
    - **Interpretation:** The shell interprets the command and decides what to do.
    - **Execution:** The shell uses system calls to perform the command.
    - **Output:** The result of the command is displayed in the shell.

### Shell Programs
- A shell is an application program that runs programs on behalf of the user 
	- sh - Original Unix shell (Stephen Bourne, AT&T Bell Labs, 1977) 
	- csh/tcsh - BSD Unix C shell 
	- bash - “Bourne-Again” Shell (default Linux shell)
- Simple shell 
	- Described in the textbook, starting at p. 753 
	- Implementation of a very elementary shell 
	- Purpose 
		- Understand what happens when you type commands 
		- Understand use and operation of process control operations
![[Screen Shot 2024-07-10 at 12.36.51.png]]
### Simple Shell Implementation 
- Basic loop 
	- Read line from command line 
	- Execute the requested operation 
		- Built-in command (only one implemented is quit) 
		- Load and execute program from file
![[Screen Shot 2024-07-10 at 12.46.10.png]]

```c
void eval(char *cmdline) 
{ 
	char *argv[MAXARGS]; /* Argument list execve() */ 
	char buf[MAXLINE]; /* Holds modified command line */ 
	int bg; /* Should the job run in bg or fg? */ 
	pid_t pid; /* Process id */ 
	
	strcpy(buf, cmdline); 
	bg = parseline(buf, argv); 
	/* parseline will parse ‘buf’ into ‘argv’ and return whether or */
	/* not input line ended in '&' */
	
	if (argv[0] == NULL) 
		return; /* Ignore empty lines */ 

	/* If it is a ‘built in’ command, then handle it here in this program. */
	/* Otherwise fork/exec the program specified in argv[0] */
	if (!builtin_command(argv)) { 
		if ((pid = fork()) == 0) { /* Child runs user job */ 
			execve(argv[0], argv, environ); 
			// If we get here, execve failed. 
			// Start argv[0]. execve only returns on error.
			printf("%s: %s\n", argv[0], strerror(errno)); 
			exit(127); 
		} 
		
		/* Parent waits for foreground job to terminate */ 
		if (!bg) { 
			int status; 
			if (waitpid(pid, &status, 0) < 0) 
				unix_error("waitfg: waitpid error"); } 
		else 
			printf("%d %s", pid, cmdline); 
	} 
	return; 
}
```
- There is a problem with this code.

### Problem with Simple Shell Example 
- Shell designed to run indefinitely 
	- Should not accumulate unneeded resources 
		- Memory 
		- Child processes 
		- File descriptors 
- Our example shell correctly waits for and reaps foreground jobs 
- But what about background jobs? 
	- Will become zombies when they terminate 
	- Will never be reaped because shell (typically) will not terminate 
	- Could run the entire computer out of memory 
		- More likely, run out of PIDs
















