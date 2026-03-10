## Computer Networks 

- A network is a hierarchical system of boxes and wires organized by geographical proximity 
	- LAN (Local Area Network) spans a building or campus 
		- Ethernet is most prominent example 
	- WAN (Wide Area Network) spans country or world 
		- Typically high-speed point-to-point (mostly optical) links 
	- Also: SAN (Storage area network), MAN (Metropolitan), etc., etc. 
- An internetwork (internet) is an interconnected set of networks 
	- The Global IP Internet (uppercase “I”) is the most famous example of an internet (lowercase “i”) 
- Let’s see how an internet is built from the ground up

### Hardware Organization of a Network Host
![[Screen Shot 2024-07-23 at 20.13.26.png]]
### Old Lowest Level: Ethernet Segment 
![[Screen Shot 2024-07-18 at 12.49.21.png]]
- Ethernet segment consists of a collection of hosts connected by wires (twisted pairs) to a hub 
- Spans room or floor in a building 
- Operation 
	- Each Ethernet adapter has a unique 48-bit address (MAC address) 
		- E.g., 00:16:ea:e3:54:e6 
	- Hosts send bits to any other host in chunks called frames 
	- Hub slavishly copies each bit from each port to every other port 
		- Every host sees every bit 
	- Note: Hubs are obsolete. Bridges (switches, routers) became cheap enough to replace them

### Next Level: Bridged Ethernet Segment
![[Screen Shot 2024-07-18 at 12.49.59.png]]
- Spans building or campus 
- Bridges cleverly learn which hosts are reachable from which ports and then selectively copy frames from port to port

### Conceptual View of LANs 

- For simplicity, hubs, bridges, and wires are often shown as a collection of hosts attached to a single wire: ![[Screen Shot 2024-07-18 at 12.50.45.png]]
### Next Level: internets 

- Multiple incompatible LANs can be physically connected by specialized computers called routers 
- The connected networks are called an internet (lower case)
![[Screen Shot 2024-07-18 at 12.51.26.png]]

### Logical Structure of an internet
![[Screen Shot 2024-07-23 at 20.40.21.png]]
- Ad hoc interconnection of networks 
	- No particular topology 
	- Vastly different router & link capacities 
- Send packets from source to destination by hopping through networks 
	- Router forms bridge from one network to another 
	- Different packets may take different routes

## The Notion of an internet Protocol 

- How is it possible to send bits across incompatible LANs and WANs? 
- Solution: protocol software running on each host and router 
	- Protocol is a set of rules that governs how hosts and routers should cooperate when they transfer data from network to network. 
	- Smooths out the differences between the different networks

### What Does an internet Protocol Do? 

- Provides a naming scheme 
	- An internet protocol defines a uniform format for host addresses 
	- Each host (and router) is assigned at least one of these internet addresses that uniquely identifies it 
- Provides a delivery mechanism 
	- An internet protocol defines a standard transfer unit (packet) 
	- Packet consists of header and payload 
		- Header: contains info such as packet size, source and destination addresses 
		- Payload: contains data bits sent from source host

### Transferring internet Data Via Encapsulation
![[Screen Shot 2024-07-24 at 01.29.44.png]]

### Other Issues 

- We are glossing over a number of important questions: 
	- What if different networks have different maximum frame sizes? (segmentation) 
	- How do routers know where to forward frames? 
	- How are routers informed when the network topology changes? 
	- What if packets get lost? 
- These (and other) questions are addressed by the area of systems known as computer networking

## Internet
### Global IP Internet (upper case) 

- Most famous example of an internet 
- Based on the TCP/IP protocol family 
	- IP (Internet Protocol) 
		- Provides *basic naming scheme* and unreliable *delivery capability* of packets (datagrams) from *host-to-host* 
	- UDP (User Datagram Protocol) 
		- Uses IP to provide *unreliable* datagram delivery from *process-to-process* 
	- TCP (Transmission Control Protocol) 
		- Uses IP to provide *reliable* byte streams from *process-to-process* over *connections*
- Accessed via a mix of Unix file I/O and functions from the *sockets interface*

### Basic Internet Components 

- Internet backbone: 
	- collection of routers (nationwide or worldwide) connected by high-speed point-to-point networks 
- Internet Exchange Points (IXP): 
	- router that connects multiple backbones (often referred to as peers) 
	- Also called Network Access Points (NAP) 
- Regional networks: 
	- smaller backbones that cover smaller geographical areas (e.g., cities or states) 
- Point of presence (POP): 
	- machine that is connected to the Internet 
- Internet Service Providers (ISPs): 
	- provide dial-up or direct access to POPs

### Internet Connection Hierarchy
![[Screen Shot 2024-07-25 at 20.15.22.png]]

### Hardware and Software Organization of an Internet Application
![[Screen Shot 2024-07-25 at 20.18.13.png]]

## A Programmer’s View of the Internet 

1. Hosts are mapped to a set of 32-bit IP addresses 
	- 128.2.203.179 
	- 127.0.0.1 (always localhost) 
2. As a convenience for humans, the Domain Name System maps a set of identifiers called Internet domain names to IP addresses: 
	- www.cs.cmu.edu “resolves to” 128.2.217.3 
3. A process on one Internet host can communicate with a process on another Internet host over a connection

### Aside: IPv4 and IPv6 

- IPv4 (Internet Protocol version 4) specified 1981 
	- 32-bit host addresses (192.0.2.43) 
	- Known to not be enough for everyone since ~1990 
- IPv6 (Internet Protocol version 6) specified 1996 
	- 128-bit addresses (2001:0db8:0:0:0:0:cafe:la7e) 
	- Intended to replace IPv4 
	- Very slow adoption due to need to replace routers (CMU’s network doesn’t support IPv6 at all!) 
- Application programmers mostly don’t have to care 
	- Sockets API makes it easy to write code that seamlessly uses either, as necessary

### (1) IP Addresses 

- 32-bit IP addresses are stored in an IP address struct 
	- IP addresses are always stored in memory in network byte order (big-endian byte order) 
	- True in general for any integer transferred in a packet header from one machine to another. 
		- E.g., the port number used to identify an Internet connection.
![[Screen Shot 2024-07-25 at 20.57.35.png]]
#### Dotted Decimal Notation 

- By convention, each byte in a 32-bit IP address is represented by its decimal value and separated by a period 
	- IP address: 0x8002C2F2 = 128.2.194.242 
- Use `getaddrinfo` and `getnameinfo` functions (described later) to convert between IP addresses and dotted decimal format.

### (2) Internet Domain Names
![[Screen Shot 2024-07-25 at 21.44.58.png]]

#### Domain Naming System (DNS) 

- The Internet maintains a mapping between IP addresses and domain names in a worldwide distributed database called DNS 
- Conceptually, programmers can view the DNS database as a collection of millions of host entries.
	- Each host entry defines the mapping between a set of domain names and IP addresses. 
	- In a mathematical sense, a host entry is an equivalence class of domain names and IP addresses.

#### Properties of DNS Mappings 

Can explore properties of DNS mappings using `nslookup` 

Each host has a locally defined domain name `localhost` which always maps to the loopback address 127.0.0.1
```
linux> nslookup localhost 
Address: 127.0.0.1
```

Use `hostname` to determine real domain name of local host:
```
linux> hostname 
whaleshark.ics.cs.cmu.edu
```

Simple case: one-to-one mapping between domain name and IP address:
```
linux> nslookup whaleshark.ics.cs.cmu.edu 
Address: 128.2.210.175
```

Multiple domain names mapped to the same IP address:
```
linux> nslookup cs.mit.edu 
Address: 18.25.0.23 
linux> nslookup eecs.mit.edu 
Address: 18.25.0.23
```

```
linux> nslookup 18.25.0.23 
23.0.25.18.in-addr.arpa name = eecs.mit.edu.
```

Multiple domain names mapped to multiple IP addresses:
```
linux> nslookup www.twitter.com 
Address: 104.244.42.65 
Address: 104.244.42.129 
Address: 104.244.42.193 
Address: 104.244.42.1 

linux> nslookup www.twitter.com 
Address: 104.244.42.129 
Address: 104.244.42.65 
Address: 104.244.42.193 
Address: 104.244.42.1
```

Some valid domain names don’t map to any IP address:
```
linux> nslookup ics.cs.cmu.edu 
(No Address given)
```

#### A Client-Server Transaction

- Most network applications are based on the client-server model: 
	- A server process and one or more client processes 
	- Server manages some resource 
	- Server provides service by manipulating resource for clients 
	- Server activated by request from client (vending machine analogy
![[Screen Shot 2024-07-26 at 14.03.26.png]]

### (3) Internet Connections 

- Clients and servers most often communicate by sending streams of bytes over TCP connections. Each connection is: 
	- Point-to-point: connects a pair of processes. 
	- Full-duplex: data can flow in both directions at the same time, 
	- Reliable: stream of bytes sent by the source is eventually received by the destination in the same order it was sent. 
- A socket is an endpoint of a connection 
	- Socket address is an `IPaddress:port` pair 
- A port is a 16-bit integer that identifies a process: 
	- Ephemeral port: Assigned automatically by client kernel when client makes a connection request. 
	- Well-known port: Associated with some service provided by a server (e.g., port 80 is associated with Web servers)

#### Well-known Service Names and Ports 

- Popular services have permanently assigned well-known ports and corresponding well-known service names: 
	- echo servers: echo 7 
	- ftp servers: ftp 21 
	- ssh servers: ssh 22 
	- email servers: smtp 25 
	- Unencrypted Web servers: http 80 
	- SSL/TLS encrypted Web: https 443 
- Mappings between well-known ports and service names is contained in the file /etc/services on each Linux machine.

#### Anatomy of a Connection 

- A connection is uniquely identified by the socket addresses of its endpoints (socket pair) 
	- `(cliaddr:cliport, servaddr:servport)` ![[Screen Shot 2024-07-26 at 14.15.35.png]]

#### Using Ports to Identify Services
![[Screen Shot 2024-07-26 at 14.25.20.png]]

## Sockets

### Sockets Interface 

- Set of system-level functions used in conjunction with Unix I/O to build network applications. 
- Created in the early 80’s as part of the original Berkeley distribution of Unix that contained an early version of the Internet protocols. 
- Available on all modern systems 
	- Unix variants, Windows, OS X, IOS, Android, ARM

### Sockets 

- What is a socket? 
	- To the kernel, a socket is an endpoint of communication 
	- To an application, a socket is a file descriptor that lets the application read/write from/to the network 
	- Using the FD abstraction lets you reuse code & interfaces 
- Clients and servers communicate with each other by reading from and writing to socket descriptors ![[Screen Shot 2024-07-26 at 14.39.39.png]]
- The main distinction between regular file I/O and socket I/O is how the application “opens” the socket descriptors

### Socket Programming Example 

- Echo server and client 
- Server 
	- Accepts connection request 
	- Repeats back lines as they are typed 
- Client 
	- Requests connection to server 
	- Repeatedly: 
		- Read line from terminal 
		- Send to server 
		- Read reply from server 
		- Print line to terminal

### Echo Server/Client Session Example
![[Screen Shot 2024-07-26 at 15.03.08.png]]
- The client connects to the server, sends text messages, and the server echoes these messages back.
- Each connection is terminated by the client using Ctrl+D.
- The server logs each connection and the amount of data received from the client.

### Echo Server + Client Structure
![[Screen Shot 2024-07-26 at 16.09.45.png]]

#### 1. Start Server
- **open_listenfd:** The server begins by opening a listening file descriptor (`open_listenfd`). This sets up the server to listen for incoming connection requests on a specific port.

#### 2. Start Client
- **open_clientfd:** The client starts by opening a client file descriptor (`open_clientfd`). This connects the client to the server's address and port.

#### 3. Exchange Data
- **Connection Request:** The client sends a connection request to the server.
- **accept:** The server accepts the connection request from the client.
- **Client Side:**
	- **terminal read:** The client reads input from the terminal (user input).
	- **socket write:** The client writes the input to the server through the socket.
- **Server Side:**
	- **socket read:** The server reads the data sent by the client through the socket.
	- **terminal write:** The server writes the data back to the client through the socket (echoes the message).
	- **socket write:** The server sends the echoed data back to the client.

#### 4. Disconnect Client
- **close:** When the client is done sending data (end-of-file, EOF), it closes the connection.

#### 5. Drop Client
- **socket read:** The server reads the EOF signal from the client, indicating that the client has closed the connection.
- **close:** The server closes the connection on its side.

#### Summary
1. **Server Setup:** The server opens a listening port to await connections.
2. **Client Setup:** The client connects to the server using the server's address and port.
3. **Data Exchange:** The client sends messages to the server, and the server echoes them back.
4. **Client Disconnection:** The client closes the connection when done.
5. **Server Cleanup:** The server detects the client disconnection and closes its end of the connection.

### Unbuffered RIO Input/Output

#### Overview:
- **RIO (Robust I/O):** These are functions used for reading and writing data in a reliable way, particularly useful for network sockets.

#### Functions:
1. **rio_readn:**
   ```c
   ssize_t rio_readn(int fd, void *usrbuf, size_t n);
   ```
   - **Purpose:** Reads `n` bytes from file descriptor `fd` into the buffer `usrbuf`.
   - **Returns:** Number of bytes transferred if successful, 0 on end-of-file (EOF), -1 on error.
   - Returns a short count only if it encounters EOF.
   - Use it when you know exactly how many bytes you need to read.

2. **rio_writen:**
   ```c
   ssize_t rio_writen(int fd, void *usrbuf, size_t n);
   ```
   - **Purpose:** Writes `n` bytes from the buffer `usrbuf` to the file descriptor `fd`.
   - **Returns:** Number of bytes transferred if successful, -1 on error.
   - Never returns a short count.

- **Interleaving:**
  - Calls to `rio_readn` and `rio_writen` can be interleaved arbitrarily on the same descriptor, meaning you can use them in any order as needed.

### Buffered RIO Input Functions

#### Overview:
- **Buffered I/O:** Efficiently reads text lines and binary data by caching data in an internal memory buffer.

#### Functions:
1. **rio_readinitb:**
   ```c
   void rio_readinitb(rio_t *rp, int fd);
   ```
   - **Purpose:** Initializes the read buffer `rp` for the file descriptor `fd`.

2. **rio_readlineb:**
   ```c
   ssize_t rio_readlineb(rio_t *rp, void *usrbuf, size_t maxlen);
   ```
   - **Purpose:** Reads a text line of up to `maxlen` bytes from file descriptor `fd` and stores it in the buffer `usrbuf`.
   - **Returns:** Number of bytes read if successful, 0 on EOF, -1 on error.
   
3. **rio_readnb:**
   ```c
   ssize_t rio_readnb(rio_t *rp, void *usrbuf, size_t n);
   ```
   - **Purpose:** Reads `n` bytes from the file descriptor `fd` into the buffer `usrbuf`.
   - **Returns:** Number of bytes read if successful, 0 on EOF, -1 on error.

- **`rio_readlineb`:** reads a text line of up to `maxlen` bytes from file `fd` and stores the line in `usrbuf`
	- Especially useful for reading text lines from network sockets
- Stopping conditions:
	- `maxlen` bytes read.
	- EOF encountered.
	- Newline (`\n`) encountered.

### Echo Client: Main Routine
![[Screen Shot 2024-07-26 at 17.44.42.png]]
- Read and Send Messages:
	- Read input from the terminal.
	- Write input to the server.
	- Read the server's response.
	- Print the server's response to the terminal.

### Iterative Echo Server: Main Routine

![[Screen Shot 2024-07-26 at 17.45.02.png]]
- Infinite Loop to Accept Connections:
	- Set `clientlen` to the size of `sockaddr_storage`.
	- Accept a client connection.
	- Get the client's hostname and port.
	- Print a message indicating a connection was made.
	- Call the `echo` function to handle the connection.
	- Close the connection.

### Echo Server: echo function
- The server uses RIO to read and echo text lines until EOF (end-of-file) condition is encountered. 
	- EOF condition caused by client calling close(`clientfd`)![[Screen Shot 2024-07-26 at 18.55.49.png]]

### Generic Socket Address Structure

**Why Do We Need Socket Addresses?**

When computers talk to each other over a network, they need to know exactly where to send and receive information. The socket address helps with this by providing both the location of the computer and the specific program on that computer.

**Components of a Socket Address:**

1. **Protocol Family**:
    - This tells us what type of network we are using (e.g., IPv4 or IPv6).
2. **Address Data**:
    - This includes the actual IP address and port number.

- Generic socket address:
	- Used for address arguments in functions like `connect`, `bind`, and `accept`.
	- Necessary because C did not have generic pointers (`void *`) when the sockets interface was designed.
	- Adopts the Stevens convention for casting convenience.

```c
typedef struct sockaddr SA;

struct sockaddr {
    uint16_t sa_family;  // Protocol family (e.g., IPv4)
    char sa_data[14];    // Address data (IP address and port number)
};
```

![[Screen Shot 2024-07-26 at 19.05.35.png]]

When a client wants to connect to a server, it needs the server’s socket address:

- **IP Address**: "192.168.1.10" (server’s address)
- **Port Number**: "8080" (specific service on the server)

This combined information forms the socket address that tells the client exactly where to send its messages.

### Internet (IPv4) Specific Socket Address Structure

- **Internet (IPv4) specific socket address:** 
  - Used for IPv4 addresses.
  - Must cast from `struct sockaddr_in *` to `struct sockaddr *` for functions that take socket address arguments.

```c
struct sockaddr_in {
    uint16_t sin_family;  /* Protocol family (always AF_INET for IPv4) */
    uint16_t sin_port;    /* Port number in network byte order */
    struct in_addr sin_addr; /* IP address in network byte order */
    unsigned char sin_zero[8]; /* Padding to make the structure the same size as struct sockaddr */
};
```

![[Screen Shot 2024-07-26 at 19.11.26.png]]
- **sin_family:** This part of the structure indicates it's an IPv4 address (AF_INET).
- **sin_port:** This part holds the port number.
- **sin_addr:** This part holds the IP address.
- **sin_zero:** Extra padding to align the structure.

### Host and Service Conversion: `getaddrinfo` 

**What is `getaddrinfo`?**

- `getaddrinfo` is a function used to convert human-readable strings (like website names or service names) into a format that computers can use for network communication.
- It takes things like hostnames (e.g., "[www.example.com](http://www.example.com)"), IP addresses (e.g., "192.168.1.1"), and service names (e.g., "http") and converts them into socket address structures.

- `getaddrinfo` is the modern way to convert string representations of hostnames, host addresses, ports, and service names to socket address structures. 
	- Replaces obsolete `gethostbyname` and `getservbyname` funcs. 
- Advantages: 
	- Reentrant (can be safely used by threaded programs). 
	- Allows us to write portable protocol-independent code 
		- Works with both IPv4 and IPv6 
- Disadvantages 
	- Somewhat complex 
	- Fortunately, a small number of usage patterns suffice in most cases.
![[Screen Shot 2024-07-26 at 19.40.54.png]]
- Given host and service, `getaddrinfo` returns result that points to a linked list of `addrinfo` structs, each of which points to a corresponding socket address struct, and which contains arguments for the sockets interface functions. 
- Helper functions: 
	- `freeadderinfo` frees the entire linked list. 
	- `gai_strerror` converts error code to an error message.

#### Linked List Returned by `getaddrinfo`

![[Screen Shot 2024-07-27 at 00.40.54.png]]
- **SA List**:
    - SA stands for Socket Address.
    - The **SA list** is a list of socket address structures that `getaddrinfo` generates.
    - Think of it as a list of phone numbers that the function provides for you to use.
- **addrinfo Structs**:
    - These are structures that hold information about the socket addresses.
    - Each `addrinfo` struct includes:
        - **ai_canonname**: The canonical name of the host.
        - **ai_addr**: The socket address.
        - **ai_next**: A pointer to the next `addrinfo` struct in the list.
- **Socket Address Structs**:
    - These are the actual structures that contain the detailed socket address information (like the IP address and port number).

#### `addrinfo` Struct
![[Screen Shot 2024-07-26 at 21.20.44.png]]
- Each `addrinfo` struct returned by `getaddrinfo` contains arguments that can be passed directly to socket function. 
- Also points to a socket address struct that can be passed directly to `connect` and `bind` functions.

### Host and Service Conversion: `getnameinfo` 

- `getnameinfo` is the inverse of `getaddrinfo`, converting a socket address to the corresponding host and service. 
	- Replaces obsolete `gethostbyaddr` and `getservbyport` funcs. 
	- Reentrant and protocol independent.![[Screen Shot 2024-07-26 at 21.24.36.png]]

### Conversion Example
![[Screen Shot 2024-07-26 at 21.31.18.png]]
![[Screen Shot 2024-07-26 at 21.31.27.png]]

### Running hostinfo

![[Screen Shot 2024-07-26 at 21.31.48.png]]
## Setting up connections 

![[Screen Shot 2024-07-27 at 00.57.26.png]]

### Sockets Interface: socket
![[Screen Shot 2024-07-27 at 01.03.22.png]]

### Sockets Interface: bind
![[Screen Shot 2024-07-27 at 01.15.05.png]]

### Sockets Interface: listen
![[Screen Shot 2024-07-27 at 01.28.56.png]]

### Sockets Interface: accept

![[Screen Shot 2024-07-27 at 02.06.10.png]]

### Sockets Interface: connect
![[Screen Shot 2024-07-27 at 02.13.06.png]]

### `connect/accept` Illustrated

![[Screen Shot 2024-07-27 at 02.38.08.png]]

**Steps to Establish a Connection:**

1. **Server Waits for Connection Requests:**
   - **Server** creates a socket and uses `listenfd` to listen for incoming connection requests.
   - The server calls the `accept` function and waits (blocks) until a client tries to connect.
   - Think of it as the server waiting by the phone for someone to call.

2. **Client Makes a Connection Request:**
   - **Client** creates a socket (`clientfd`) and tries to connect to the server.
   - The client calls the `connect` function and waits (blocks) until the connection is established.
   - This is like the client dialing the server's phone number and waiting for the server to pick up.

3. **Connection Established:**
   - When the server receives the connection request, the `accept` function returns a new socket descriptor (`connfd`).
   - The `connfd` is used for communication between the client and server.
   - The client returns from the `connect` function, indicating the connection is established.
   - Now, both the client and server can send and receive data through their respective sockets (`clientfd` and `connfd`).

### Connected vs. Listening Descriptors 
-- the difference between two types of socket descriptors that a server uses in network programming

- Listening descriptor (a special type of socket used by the server to listen for incoming connection requests from clients)
	- End point where clients can send connection requests
	- Created once and exists for lifetime of the server 
- Connected descriptor (a socket that is used for an actual connection between a client and the server)
	- End point of the connection between client and server 
	- A new descriptor is created each time the server accepts a connection request from a client 
	- It exists only for the duration of the connection. Once the conversation (data exchange) is done, this descriptor is closed
- Why the distinction? 
	1. **Concurrent Connections:**
	   - The distinction allows servers to handle multiple client connections simultaneously.
	   - The listening descriptor waits for new requests, while each connected descriptor handles an active connection.
	2. **Example:** Each time we receive a new request, we fork a child to handle the request
	   - When a new connection request comes in, the server accepts it, creating a new connected descriptor.
	   - The server can continue to listen for new connections with the listening descriptor while using the connected descriptor to communicate with the current client.
	   - It's like having a main phone line (listening descriptor) and creating a separate line (connected descriptor) for each active call, allowing the main line to remain open for more incoming calls.

### Sockets Helper: `open_clientfd`

-- explains a function `open_clientfd` that helps a client establish a connection with a server.

#### Function Definition:

```c
int open_clientfd(char *hostname, char *port) {
    int clientfd;
    struct addrinfo hints, *listp, *p;
```

- **hostname**: The server’s name (like "www.example.com").
- **port**: The port number (like "80" for HTTP).

#### Steps in the Function:

1. **Get a list of potential server addresses:**

```c
memset(&hints, 0, sizeof(struct addrinfo));
hints.ai_socktype = SOCK_STREAM;  /* Open a connection */
hints.ai_flags = AI_NUMERICSERV;  /* using numeric port arg. */
hints.ai_flags |= AI_ADDRCONFIG;  /* Recommended for connections */
```

- **memset**: Initializes the `hints` structure to zero.
- **hints.ai_socktype = SOCK_STREAM**: Indicates we want to open a TCP connection (reliable, stream-oriented).
- **hints.ai_flags = AI_NUMERICSERV**: Indicates the port argument is numeric.
- **hints.ai_flags |= AI_ADDRCONFIG**: Uses whichever of IPv4 and IPv6 works on the computer. Good for clients.
	- The `hints` structure is of type `struct addrinfo`.

```c
getaddrinfo(hostname, port, &hints, &listp);
```

- **getaddrinfo**: Converts the hostname and port into a list of potential server addresses (`listp`).

Step 1: Initialize `hints` and Call `getaddrinfo`
![[Screen Shot 2024-07-27 at 04.09.21.png]]
Step 2: Iterate Through the List and Try to Connect
- `for (p = listp; p; p = p->ai_next):`
    - Iterates through the list of potential addresses.
- `if ((clientfd = socket(p->ai_family, p->ai_socktype, p->ai_protocol)) < 0):`
    - Tries to create a socket with the current address information.
    - If the socket creation fails, it moves to the next address (`continue`).
- `if (connect(clientfd, p->ai_addr, p->ai_addrlen) != -1):`
    - Tries to connect to the server using the current address.
    - If the connection succeeds, it breaks out of the loop (`break`).
- `Close(clientfd):`
    - If the connection fails, it closes the socket and tries the next address.

Step 3: Clean Up and Return Result
![[Screen Shot 2024-07-27 at 04.08.44.png]]

### `getaddrinfo`

![[Screen Shot 2024-07-27 at 03.41.18.png]]

- **result**: The starting point of the linked list of `addrinfo` structures.
- **addrinfo structs**: Each contains:
  - **ai_canonname**: The canonical name of the host.
  - **ai_addr**: The socket address.
  - **ai_next**: Pointer to the next `addrinfo` structure.

#### Steps:

1. **Clients:**
   - Walk this list, trying each socket address in turn, until `socket` and `connect` succeed.
   - This means the client tries each address in the list until it successfully connects to the server.

2. **Servers:**
   - Walk the list, calling `socket`, `listen`, `bind` for all addresses.
   - Use `select` to accept connections on any of them (beyond the current scope).

### Sockets Helper: `open_listenfd`

![[Screen Shot 2024-07-27 at 19.02.33.png]]
![[Screen Shot 2024-07-27 at 19.02.45.png]]
![[Screen Shot 2024-07-27 at 19.02.55.png]]

### Testing Servers Using telnet

- The telnet program is invaluable for testing servers that transmit ASCII strings over Internet connections ▪ Our simple echo server ▪ Web servers ▪ Mail servers 
- Usage: ▪ linux> telnet ▪ Creates a connection with a server running on and listening on port

## Application protocol example: HTTP



