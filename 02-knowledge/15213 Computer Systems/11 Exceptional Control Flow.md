# Exceptional Control Flow 

## Altering the Control Flow

- Control Flow: CPU core simply reads and executes (interprets) a sequence of instructions
- Up to now: two mechanisms for changing control flow: 
	- Jumps and branches 
	- Call and return 
	- React to changes in **program state** 
- Insufficient for a useful system: Difficult to react to changes in **system state** 
	- Data arrives from a disk or a network adapter 
	- Instruction divides by zero 
	- User hits Ctrl-C at the keyboard 
	- System timer expires 
- System needs mechanisms for “exceptional control flow”

## Exceptional Control Flow 

- Exists at all levels of a computer system 
- Low level mechanisms 
	1. Exceptions 
		- Change in control flow in response to a system event (i.e., change in system state)
		- Implemented using combination of hardware and OS software 
- Higher level mechanisms 
	2. Process context switch: Implemented by OS software and hardware timer 
	3. Signals: Implemented by OS software 
	4. Nonlocal jumps: `setjmp()` and `longjmp()` ▪ Implemented by C runtime library
# Exceptions 

## Exceptions 

- An exception is a transfer of control to the OS kernel in response to some event (i.e., change in processor state) 
	- Kernel is the memory-resident part of the OS 
	- Examples of events: Divide by 0, arithmetic overflow, page fault, I/O request completes, typing Ctrl-C![[Screen Shot 2024-07-15 at 00.01.46.png]]

## Exception Tables![[Screen Shot 2024-07-15 at 00.03.13.png]]

## Taxonomy of Hardware ECF 
![[Screen Shot 2024-07-15 at 00.03.48.png]]

### Asynchronous Exceptions (Interrupts) 

- Caused by events external to the processor 
	- Indicated by setting the processor’s interrupt pin 
	- Handler returns to “next” instruction 
- Examples: 
	- Timer interrupt 
		- Every few ms, an external timer chip triggers an interrupt 
		- Used by the kernel to take back control from user programs 
	- I/O interrupt from external device 
		- Hitting Ctrl-C at the keyboard 
		- Arrival of a packet from a network 
		- Arrival of data from a disk

### Synchronous Exceptions 

- Caused by events that occur as a result of executing an instruction: 
	- Traps 
		- Intentional, set program up to “trip the trap” and do something 
		- Examples: system calls, gdb breakpoints 
		- Returns control to “next” instruction 
	- Faults 
		- Unintentional but possibly recoverable 
		- Examples: page faults (recoverable), protection faults (unrecoverable), floating point exceptions 
		- Either re-executes faulting (“current”) instruction or aborts 
	- Aborts 
		- Unintentional and unrecoverable 
		- Examples: illegal instruction, parity error, machine check 
		- Aborts current program

## System Calls 
![[Screen Shot 2024-07-15 at 00.21.47.png]]
### System Call Example: Opening File
![[Screen Shot 2024-07-15 at 00.51.49.png]]
Almost like a function call
- Transfer of control
- On return, executes next instruction
- Passes arguments using calling convention
- Gets result in `%rax`

One important exception!
- Executed by Kernel
- Different set of privileges
- And other differences:
	- E.g., “address" of“function" is in `%rax`
	- Uses `errno`
	- Etc.

### Fault Example: Page Fault 
![[Screen Shot 2024-07-15 at 00.57.30.png]]

### Fault Example: Invalid Memory Reference 
![[Screen Shot 2024-07-15 at 00.58.43.png]]

# Signals 

## ECF Exists at All Levels of a System 

- Exceptions ▪ Hardware and operating system kernel software 
- Process Context Switch ▪ Hardware timer and kernel software 
- Signals ▪ Kernel software and application software 
- Nonlocal jumps ▪ Application code

## Problem with Simple Shell Example

- Background jobs will become zombies when they terminate
- Solution: Exceptional control flow 
	- The kernel will interrupt regular processing to alert us when a background process completes 
	- In Unix, the alert mechanism is called a signal

## Signals 

- A signal is a small message that notifies a process that an event of some type has occurred in the system 
	- Akin to exceptions and interrupts 
	- Sent from the kernel (sometimes at the request of another process) to a process 
	- Signal type is identified by small integer ID’s (1-30) 
	- Only information in a signal is its ID and the fact that it arrived![[Screen Shot 2024-07-15 at 18.08.22.png]]

## Signal Concepts: Sending a Signal 

- Kernel sends a signal to a destination process by updating some state in the context of the destination process 
- Kernel sends a signal for one of the following reasons: 
	- Kernel has detected a system event such as divide-by-zero (SIGFPE) or the termination of a child process (SIGCHLD) 
	- Another process has invoked the kill system call to explicitly request the kernel to send a signal to the destination process

## Signal Concepts: Receiving a Signal 

- A destination process receives a signal when it is forced by the kernel to react in some way to the signal 
- Some possible ways to react: 
	- Ignore the signal (do nothing) 
	- Terminate the process (with optional core dump) 
	- Catch the signal by executing a user-level function called signal handler 
		- Akin to a hardware exception handler being called in response to an asynchronous interrupt: ![[Screen Shot 2024-07-15 at 19.42.40.png]]

## Signal Concepts: Pending and Blocked Signals

- A signal is pending if sent but not yet received 
	- There can be at most one pending signal of each type 
	- Important: Signals are not queued 
		- If a process has a pending signal of type k, then subsequent signals of type k that are sent to that process are discarded 
- A process can block the receipt of certain signals 
	- Blocked signals can be sent, but will not be received until the signal is unblocked 
	- Some signals cannot be blocked (SIGKILL, SIGSTOP) or can only be blocked when sent by other processes (SIGSEGV, SIGILL, etc) 
- A pending signal is received at most once

## Signal Concepts: Pending/Blocked Bits 

- Kernel maintains `pending` and `blocked` bit vectors in the context of each process 
	- `pending`: represents the set of pending signals 
		- Kernel sets bit k in `pending` when a signal of type k is sent 
		- Kernel clears bit k in `pending` when a signal of type k is received 
	- `blocked`: represents the set of blocked signals 
		- Can be set and cleared by using the `sigprocmask` function 
		- Also referred to as the signal mask.

## Sending Signals: Process Groups 

- Every process belongs to exactly one process group![[Screen Shot 2024-07-15 at 19.59.38.png]]

## Sending Signals with /bin/kill Program

![[Screen Shot 2024-07-15 at 21.02.12.png]]
## Sending Signals from the Keyboard

![[Screen Shot 2024-07-15 at 21.12.47.png]]
![[Screen Shot 2024-07-15 at 21.19.39.png]]
## Sending Signals with kill Function
![[Screen Shot 2024-07-15 at 21.34.42.png]]
## Receiving Signals

- Suppose kernel is returning from an exception handler and is ready to pass control to process p![[Screen Shot 2024-07-15 at 21.36.43.png]]
- Kernel computes `pnb = pending & ~blocked `
	- The set of pending nonblocked signals for process p 
- If `(pnb == 0) `
	- Pass control to next instruction in the logical flow for p 
- Else 
	- Choose least nonzero bit k in `pnb` and force process p to receive signal k 
	- The receipt of the signal triggers some action by p 
	- Repeat for all nonzero k in `pnb `
	- Pass control to next instruction in logical flow for p
### Default Actions

- Each signal type has a predefined default action, which is one of: 
	- The process terminates 
	- The process stops until restarted by a SIGCONT signal 
	- The process ignores the signal
### Installing Signal Handlers

- The `signal` function modifies the default action associated with the receipt of signal `signum`: 
	- `handler_t *signal(int signum, handler_t *handler)`
- Different values for handler: 
	- SIG_IGN: ignore signals of type `signum` 
	- SIG_DFL: revert to the default action on receipt of signals of type `signum` 
	- Otherwise, handler is the address of a user-level signal handler 
		- Called when process receives signal of type `signum` 
		- Referred to as “installing” the handler 
		- Executing handler is called “catching” or “handling” the signal 
		- When the handler executes its return statement, control passes back to instruction in the control flow of the process that was interrupted by receipt of the signal
![[Screen Shot 2024-07-15 at 22.17.24.png]]

### Signals Handlers as Concurrent Flows

- A signal handler is a separate logical flow (not process) that runs concurrently with the main program 
- But, this flow exists only until returns to main program
![[Screen Shot 2024-07-16 at 01.17.41.png]]
![[Screen Shot 2024-07-16 at 04.14.04.png]]
### Nested Signal Handlers

- Handlers can be interrupted by other handlers
![[Screen Shot 2024-07-16 at 04.15.40.png]]
## Blocking and Unblocking Signals

- Implicit blocking mechanism 
	- Kernel blocks any pending signals of type currently being handled 
	- e.g., a SIGINT handler can’t be interrupted by another SIGINT 
- Explicit blocking and unblocking mechanism 
	- `sigprocmask` function 
- Supporting functions 
	- `sigemptyset` – Create empty set 
	- `sigfillset` – Add every signal number to set 
	- `sigaddset` – Add signal number to set 
	- `sigdelset` – Delete signal number from set

### Temporarily Blocking Signals
![[Screen Shot 2024-07-16 at 04.20.15.png]]
## Signal Handlers
### Safe Signal Handling

- Handlers are tricky because they are concurrent with main program and share the same global data structures 
	- Shared data structures can become corrupted.

Guidelines for Writing Safe Handlers
- G0: Keep your handlers as simple as possible 
	- e.g., set a global flag and return 
- G1: Call only async-signal-safe functions in your handlers 
	- `printf`, `sprintf`, `malloc`, and `exit` are not safe! 
- G2: Save and restore `errno` on entry and exit 
	- So that other handlers don’t overwrite your value of errno 
- G3: Protect accesses to shared data structures by temporarily blocking all signals 
	- To prevent possible corruption 
- G4: Declare global variables as `volatile` 
	- To prevent compiler from storing them in a register 
- G5: Declare global flags as `volatile sig_atomic_t` 
	- flag: variable that is only read or written (e.g. flag = 1, not flag++) 
	- Flag declared this way does not need to be protected like other globals

### Async-Signal-Safety

- Function is async-signal-safe if either reentrant (e.g., all variables stored on stack frame, CS:APP3e 12.7.2) or noninterruptible by signals

### Correct Signal Handling 

- Blocks all signals while running critical code
- Must wait for all terminated child processes 
	- Put wait in a loop to reap all terminated children![[Screen Shot 2024-07-18 at 21.06.36.png]]

# Nonlocal Jumps