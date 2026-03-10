**Computer Arithmetic**: Ints are not Integers, Floats are not Reals
- x ^ 2 >= 0 is true for floats but not always for integers.

Even with overflow, Integer Arithmetic is commutative and associative.
```
300 * 400 * 500 * 600 -> 1640261632
400 * 500 * 600 * 300 -> 1640261632
```

This does not apply to floats which can have some numbers disappearing.
```
(1e20 + -1e20) + 3.14 -> 3.14
1e20 + (-1e20 + 3.14) -> 1e20
```

**Assembly**: Understanding assembly is key to machine-level execution model.

**Memory Systems**: Memory is not unbounded. Memory referencing bugs can be hard to find. For example in C, there is no bound checking on arrays.

```c
typedef struct {
	int a[2];
	double d;
} struct_t;

double fun(int i) {
	volatile struct_t s;
	s.d = 3.14;
	s.a[i] = 1073741824; /* May be out of bounds */
	return s.d;
}

// fun(0) -> 3.14
// fun(1) -> 3.14
// fun(2) -> 3.139999866
// fun(3) -> 2.000000610
// fun(4) -> 3.14
// fun(6) -> Segmentation Fault
```

**Performance Outside Asymptotic Complexity**: Memory access pattern can improve the performance.

```c
void copyij(int src[2048][2048], int dst[2048][2048]) {
	int i,j;
	for (i=0; i<2048; i++)
		for (j=0; j<2048; j++)
			dst[i][j] = src[i][j]
}

// Runtime: 4 ms

void copyij(int src[2048][2048], int dst[2048][2048]) {
	int i,j;
	for (j=0; j<2048; j++)
		for (i=0; i<2048; i++)
			dst[i][j] = src[i][j]
}

// Runtime: 81 ms
```

**Networking and Concurrency**
Concepts of I/O. Programming with socket interface to communicate with any machine.

### Abstraction Levels of Computer Systems

1. **Hardware**
   - **Definition:** The physical components of a computer system.
   - **Components:**
     - **CPU:** Central Processing Unit, executes instructions.
     - **Memory:** RAM (Random Access Memory), stores data temporarily.
     - **Storage:** Hard drives, SSDs, stores data permanently.
     - **I/O Devices:** Input (keyboard, mouse), output (monitor, printer).

2. **Firmware**
   - **Definition:** Low-level software embedded in hardware devices.
   - **Components:**
     - **BIOS/UEFI:** Basic Input/Output System, initializes hardware during booting.
     - **Device Firmware:** Software controlling specific hardware components (e.g., network cards, hard drives).

3. **Operating System (OS)**
   - **Definition:** System software that manages hardware resources and provides services for application software.
   - **Components:**
     - **Kernel:** Core part of the OS, manages CPU, memory, and devices.
     - **System Libraries:** Collections of functions and routines that applications can use.
     - **System Utilities:** Programs for system maintenance and performance (e.g., file management, process monitoring).
     - **User Interface:** Command-line interface (CLI) or graphical user interface (GUI) for user interaction.

4. **Middleware**
   - **Definition:** Software that provides common services and capabilities to applications beyond what's offered by the OS.
   - **Components:**
     - **Database Management Systems (DBMS):** Software for managing databases.
     - **Web Servers:** Software for serving web pages.
     - **Application Servers:** Platforms for running application programs.

5. **Application Software**
   - **Definition:** Programs that perform specific tasks for users.
   - **Components:**
     - **Productivity Software:** Word processors, spreadsheets.
     - **Web Browsers:** Software for accessing the internet.
     - **Media Players:** Software for playing audio and video.
     - **Games:** Software for entertainment and gaming.

6. **User Interface (UI)**
   - **Definition:** The means by which users interact with a computer system.
   - **Components:**
     - **Graphical User Interface (GUI):** Visual elements like windows, icons, and buttons.
     - **Command-Line Interface (CLI):** Text-based interface for entering commands.

### Summary
- **Hardware** is the physical foundation.
- **Firmware** provides initial control and low-level interaction with hardware.
- The **Operating System (OS)** sits on top of firmware, managing hardware resources and providing a platform for applications.
- **Middleware** offers additional services and capabilities to support applications.
- **Application Software** directly serves user needs.
- The **User Interface (UI)** allows users to interact with the computer system.
