---
tags:
  - ai-generated
---

In Linux socket programming (and more broadly in POSIX socket APIs), the _socket type_ parameter (typically the second argument to the `socket()` call) determines how data is transmitted between peers. Two of the most commonly used socket types are:

1. **`SOCK_STREAM`**
2. **`SOCK_DGRAM`**

Although these are most frequently discussed in the context of the Internet protocol suite (TCP/IP), the same types can apply to other address families (e.g., `AF_VSOCK` for virtual sockets, `AF_UNIX` for local IPC, etc.). Below is an in-depth look at what these types mean and how they work.

---

## 1. `SOCK_STREAM` (Stream Sockets)

### Key Characteristics

- **Connection-oriented**: Before sending data, the client and server must establish a connection (like a “handshake” in TCP).
- **Reliable**: Data sent is guaranteed to be delivered in order and without loss, duplication, or corruption, as long as the connection remains valid (in the TCP/IP world, TCP handles these guarantees).
- **Byte stream**: Data is treated as a continuous stream of bytes; there are no inherent message boundaries. You may call `send()` multiple times, but the receiver could potentially read the same data in multiple `recv()` calls, or part of one send might be combined with part of another, depending on how the bytes arrive.
- **Flow control**: The protocol ensures that neither side is overwhelmed with more data than it can handle (again, TCP does this via a sliding window mechanism).
- **Ordered delivery**: The receiving side receives the bytes in the same order they were sent.

### Common Use Cases

- **Web servers and clients** (HTTP over TCP)
- **SSH** (secure shell connections)
- **FTP** (file transfer protocol)
- **Database connections** (e.g., PostgreSQL, MySQL over TCP)
- **vsock** with `SOCK_STREAM` for reliable host-guest communication

### Typical Example (TCP)

```c
// Pseudocode for a TCP server using SOCK_STREAM
int server_fd, new_socket;
struct sockaddr_in address;
int opt = 1;

// Create a socket
server_fd = socket(AF_INET, SOCK_STREAM, 0);

// Optionally set some socket options, e.g., to reuse port
setsockopt(server_fd, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof(opt));

// Bind the socket to an address and port
memset(&address, 0, sizeof(address));
address.sin_family = AF_INET;
address.sin_addr.s_addr = INADDR_ANY;
address.sin_port = htons(8080);
bind(server_fd, (struct sockaddr *)&address, sizeof(address));

// Listen for incoming connections
listen(server_fd, 5);

// Accept a client connection
new_socket = accept(server_fd, NULL, NULL);

// Now new_socket is a SOCK_STREAM socket connected to the client
```

---

## 2. `SOCK_DGRAM` (Datagram Sockets)

### Key Characteristics

- **Connection-less**: There is no handshake or persistent connection. Each datagram is independent of others. (Though some address families may offer “connected” mode for convenience, it still remains datagram-based under the hood.)
- **Unreliable / Best-effort delivery**: The network stack attempts to deliver the packets, but it does not guarantee delivery, ordering, or protection against duplication. Packets can be lost or arrive out of order.
- **Message-oriented**: Each `sendto()` call corresponds to one datagram, and each `recvfrom()` call receives exactly one datagram (if it fits in the buffer). The boundaries between messages are preserved—unlike `SOCK_STREAM`.
- **No inherent flow control**: The protocol (e.g., UDP in IP networks) does not provide flow control. The application must handle congestion or rate-limiting if needed.
- **Lower overhead**: Because there’s no need to maintain a connection or handle reliability at the protocol level, datagram sockets generally have lower overhead than stream sockets.

### Common Use Cases

- **DNS lookups** (UDP is often used by DNS)
- **Real-time applications** that can tolerate occasional packet loss (VoIP, online gaming, live video streaming)
- **Broadcast or Multicast** scenarios (e.g., discovery protocols)
- **vsock** with `SOCK_DGRAM` when you want connection-less, message-oriented communication between host and VM or among VMs

### Typical Example (UDP)

```c
// Pseudocode for a UDP server using SOCK_DGRAM
int sockfd;
struct sockaddr_in servaddr, cliaddr;
char buffer[1024];

// Create a datagram socket
sockfd = socket(AF_INET, SOCK_DGRAM, 0);

// Bind to an address and port
memset(&servaddr, 0, sizeof(servaddr));
servaddr.sin_family = AF_INET;
servaddr.sin_addr.s_addr = INADDR_ANY;
servaddr.sin_port = htons(8080);
bind(sockfd, (const struct sockaddr *)&servaddr, sizeof(servaddr));

// Receive from any client
socklen_t len = sizeof(cliaddr);
int n = recvfrom(sockfd, buffer, sizeof(buffer), 0,
                 (struct sockaddr *)&cliaddr, &len);

// buffer now contains the received datagram data
```

---

## 3. SOCK_STREAM vs. SOCK_DGRAM in Practice

1. **Reliability**:
    
    - `SOCK_STREAM` (e.g., TCP) guarantees delivery in the correct order, checks for errors, and retransmits lost packets.
    - `SOCK_DGRAM` (e.g., UDP) does not guarantee delivery or ordering; the application must handle possible losses or duplicates.
2. **Connection Handling**:
    
    - `SOCK_STREAM`: Requires a connection to be established before data exchange (e.g., a TCP handshake).
    - `SOCK_DGRAM`: No persistent connection; data can be sent to multiple recipients by specifying different destination addresses.
3. **Data Boundaries**:
    
    - `SOCK_STREAM`: No concept of fixed message boundaries—just a continuous byte stream.
    - `SOCK_DGRAM`: Each send call corresponds to exactly one message, which the receiver can read with a single recv call (if the buffer is large enough).
4. **Performance**:
    
    - `SOCK_STREAM`: Higher overhead due to connection maintenance, retransmissions, flow control, etc.
    - `SOCK_DGRAM`: Lower overhead, but the application must handle any reliability or re-transmission if needed.

---

## 4. Using `SOCK_STREAM` and `SOCK_DGRAM` with Other Address Families

- **AF_VSOCK** (vsock):
    
    - You can create vsock connections with `SOCK_STREAM` for reliable, connection-oriented communication between a host and guest or between guests. It behaves similarly to TCP in that it’s reliable and preserves order, but the traffic does _not_ go over a physical network interface.
    - You can also create vsock sockets with `SOCK_DGRAM`, which provides connection-less, datagram-based semantics. This is particularly useful if you have a use case where reliability or ordering is not strictly necessary, but low-latency message passing is desired between VMs or between the host and a VM.
- **AF_UNIX** (Unix domain sockets):
    
    - `SOCK_STREAM` here provides a stream-based IPC mechanism, similar to TCP but only on the same host.
    - `SOCK_DGRAM` preserves message boundaries for local IPC, though less commonly used than the stream variety.

---

## Summary

- **`SOCK_STREAM`**: Connection-oriented, reliable, byte-stream-based communication. Data is sent and received in a continuous stream, with no inherent message boundaries. Typically backed by TCP over IP, or a similar reliable mechanism in other address families like vsock.
    
- **`SOCK_DGRAM`**: Connection-less, best-effort (unreliable), message-oriented communication. Each message is discrete; order is not guaranteed. Typically backed by UDP over IP, or an equivalent datagram mechanism in other address families like vsock or Unix domain sockets.
    

Choosing between `SOCK_STREAM` and `SOCK_DGRAM` depends on your application’s requirements for reliability, ordering, overhead, and how you want to handle data boundaries.
