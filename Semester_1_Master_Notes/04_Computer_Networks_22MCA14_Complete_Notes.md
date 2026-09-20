# Master Study Notes: Computer Networks
## Course Code: 22MCA14 / MMC105 | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: Network Topologies, OSI 7-Layer Reference Model vs TCP/IP 4-Layer Architecture, Transmission Media, Framing Methods, Error Detection (Parity, Checksum, CRC Modulo-2 Division) & Hamming Code.
* **Module 2**: Data Link Control & MAC Sublayer, Flow Control (Stop-and-Wait, Go-Back-N, Selective Repeat), Random Access (ALOHA, CSMA, CSMA/CD Ethernet IEEE 802.3, CSMA/CA Wi-Fi IEEE 802.11).
* **Module 3**: Network Layer, IPv4 Addressing, Classful vs CIDR, Subnetting Calculations, IPv6, Routing Algorithms (Distance Vector & Link State Dijkstra), ARP, ICMP, NAT, DHCP.
* **Module 4**: Transport Layer Services, Port Numbers, UDP vs TCP, TCP Segment Format, TCP 3-Way Handshake & 4-Way Teardown, TCP Flow Control & Congestion Control (Slow Start, Congestion Avoidance, Fast Retransmit).
* **Module 5**: Application Layer Protocols (DNS, HTTP/HTTPS, SMTP, POP3, FTP), Socket Programming Architecture, Runnable Python TCP Client-Server Socket Implementation.

---

# MODULE 1: NETWORK MODELS & PHYSICAL/DATA LINK LAYER

## 1.1 OSI 7-Layer Reference Model vs. TCP/IP Model

```
       OSI Reference Model              TCP/IP Architecture
   +-------------------------+      +-------------------------+
 7 |    Application Layer    |  ->  |                         |
 6 |   Presentation Layer    |  ->  |    Application Layer    |
 5 |      Session Layer      |  ->  | (HTTP, DNS, SMTP, FTP)  |
   +-------------------------+      +-------------------------+
 4 |     Transport Layer     |  ->  |     Transport Layer     |
   |                         |      |       (TCP, UDP)        |
   +-------------------------+      +-------------------------+
 3 |      Network Layer      |  ->  |      Internet Layer     |
   |                         |      |      (IPv4, IPv6, ICMP) |
   +-------------------------+      +-------------------------+
 2 |     Data Link Layer     |  ->  |  Network Interface /    |
 1 |     Physical Layer      |  ->  |  Host-to-Network        |
   +-------------------------+      +-------------------------+
```

### Layer Responsibilities (OSI):
1. **Physical Layer**: Bit transmission over physical medium; defines voltage levels, pinouts, data rates.
2. **Data Link Layer**: Node-to-node hop delivery, framing, MAC addressing, error and flow control.
3. **Network Layer**: Host-to-host packet routing across heterogeneous networks using IP logical addressing.
4. **Transport Layer**: End-to-end process-to-process delivery, port multiplexing, segmentation, flow control, and error recovery.
5. **Session Layer**: Establishing, managing, and terminating dialogue sessions; checkpointing and synchronization.
6. **Presentation Layer**: Translation, data encoding, compression, and cryptography (TLS/SSL).
7. **Application Layer**: End-user services (browsing, email, remote login).

---

## 1.2 Framing & Error Detection
* **Framing**: Dividing continuous raw bit stream into discrete manageable blocks called Frames.
  * *Bit Stuffing*: When flag sequence `01111110` is used, sender automatically injects a `0` after any sequence of five consecutive `1`s. Receiver removes the stuffed `0`.
* **Cyclic Redundancy Check (CRC)**: Polynomial code based on binary modulo-2 arithmetic (XOR without carries).
  * Given data bit sequence $D(x)$ of length $k$, and generator polynomial $G(x)$ of degree $r$:
  * Append $r$ zeros to data: $D \cdot 2^r$.
  * Divide $D \cdot 2^r$ by $G$ using modulo-2 division.
  * Remainder $R$ of length $r$ is the **CRC Checksum**.
  * Transmitted Frame $T = (D \cdot 2^r) \oplus R$.

---

# MODULE 2: DATA LINK CONTROL & MAC SUBLAYER

## 2.1 Sliding Window ARQ Protocols

| Feature | Stop-and-Wait ARQ | Go-Back-N (GBN) ARQ | Selective Repeat (SR) ARQ |
| :--- | :--- | :--- | :--- |
| **Sender Window Size ($W_s$)** | $1$ | $2^m - 1$ ($N > 1$) | $2^{m-1}$ |
| **Receiver Window Size ($W_r$)**| $1$ | $1$ (Strictly 1) | $2^{m-1}$ (Equal to $W_s$) |
| **Out-of-Order Frames** | Discarded | Discarded immediately | Buffered in memory |
| **Retransmission on Loss** | Single lost frame | Retransmits frame + all subsequent frames in window | Retransmits **only** the single lost frame |
| **Acknowledgment Type** | Individual ACK | Cumulative ACK | Individual (Selective) ACK |

---

## 2.2 Multiple Access Protocols
* **Pure ALOHA**: Frames transmitted at any random time. Throughput $S = G \cdot e^{-2G}$. Max efficiency = $18.4\%$ at $G = 0.5$.
* **Slotted ALOHA**: Time divided into discrete slots equal to frame transmission time. Frames transmitted only at slot boundaries. Throughput $S = G \cdot e^{-G}$. Max efficiency = $36.8\%$ at $G = 1.0$.

### CSMA/CD (Carrier Sense Multiple Access with Collision Detection):
* Standard for Ethernet (IEEE 802.3).
* *Rule*: "Listen before talk, and listen while talk."
* If two hosts transmit simultaneously, a **Collision** occurs. Both immediately abort transmission, broadcast a 32-bit **Jam Signal**, and wait for a random backoff time calculated by the **Binary Exponential Backoff Algorithm**:
  $$\text{Backoff Time } T = K \times 51.2 \, \mu\text{s}, \quad \text{where } K \in [0, 2^{\min(k, 10)} - 1]$$
* **Minimum Frame Size Condition**: To ensure collision detection before frame transmission finishes:
  $$\text{Transmission Time } T_{\text{tx}} \ge 2 \times \text{Propagation Time } T_{\text{prop}} \implies \text{Frame Length } L \ge 2 \times R \times \frac{D}{v}$$

---

# MODULE 3: NETWORK LAYER & IP ADDRESSING

## 3.1 IPv4 Addressing & CIDR Subnetting
* 32-bit integer formatted as 4 octets separated by dots (e.g., `192.168.1.1`).
* **CIDR Notation (`/n`)**: $n$ indicates the number of continuous prefix bits reserved for the Network ID; the remaining $32 - n$ bits are Host bits.
* Number of Total IP Addresses in Subnet = $2^{32 - n}$.
* Number of Usable Host IP Addresses = $2^{32 - n} - 2$ (Subtracting Network ID and Directed Broadcast ID).

---

## 3.2 Routing Algorithms
1. **Distance Vector Routing (Bellman-Ford Algorithm)**:
   * Each router shares its entire routing table only with its immediate direct neighbors periodically.
   * *Formula*: $D_x(y) = \min_v \{ c(x, v) + D_v(y) \}$.
   * *Limitation*: Suffers from the **Count-to-Infinity Problem** during link failures (mitigated by Split Horizon & Poison Reverse).
2. **Link State Routing (Dijkstra's Algorithm)**:
   * Each router floods the state of its own directly connected links to **every router** in the entire network using Link State Packets (LSP).
   * Every router independently builds a complete topological map and runs Dijkstra's algorithm to calculate the Shortest Path Tree.

---

# MODULE 4: TRANSPORT LAYER & TCP

## 4.1 TCP vs. UDP Comparison

| Parameter | Transmission Control Protocol (TCP) | User Datagram Protocol (UDP) |
| :--- | :--- | :--- |
| **Connection Nature** | Connection-Oriented (Handshake required) | Connectionless (Fire and forget) |
| **Reliability** | Guaranteed (ACKs, Sequence numbers, Retransmission) | Unreliable (Best-effort delivery) |
| **Data Stream** | Byte Stream oriented | Message / Datagram oriented |
| **Header Size** | 20 to 60 Bytes (Flags, Window, Options) | 8 Bytes fixed |
| **Use Cases** | Web (HTTP/HTTPS), File Transfer (FTP), Email (SMTP) | Video Streaming, DNS, VoIP, Online Gaming |

---

## 4.2 TCP Connection Management
### Three-Way Handshake (Connection Establishment):
```
Client                                                  Server
  |                                                       |
  | ------------ SYN (seq = x) -------------------------> |  (Server allocates buffers)
  |                                                       |
  | <----------- SYN-ACK (seq = y, ack = x + 1) --------- |  (Client allocates buffers)
  |                                                       |
  | ------------ ACK (seq = x + 1, ack = y + 1) --------> |  (Connection ESTABLISHED)
```

### Four-Way Teardown (Connection Release):
```
Client                                                  Server
  | ------------ FIN (seq = u) -------------------------> |
  | <----------- ACK (ack = u + 1) ---------------------- |  (Client enters FIN-WAIT-2)
  | <----------- FIN (seq = v) -------------------------- |  (Server finishes sending data)
  | ------------ ACK (ack = v + 1) ---------------------> |  (Client waits 2*MSL, then closes)
```

---

## 4.3 TCP Congestion Control Mechanisms
1. **Slow Start**: Congestion Window ($cwnd$) starts at 1 MSS. On every received ACK, $cwnd = cwnd + 1 \implies$ **Exponential Growth** ($1, 2, 4, 8, \dots$).
2. **Congestion Avoidance**: When $cwnd \ge \text{ssthresh}$ (Slow Start Threshold), $cwnd$ increases by $1 \text{ MSS}$ per RTT $\implies$ **Linear Growth (Additive Increase)**.
3. **Packet Loss Detection**:
   * *Timeout*: $ssthresh = cwnd / 2$, and $cwnd$ resets back to $1 \text{ MSS}$ (Slow Start).
   * *Triple Duplicate ACKs*: Indication of mild congestion. Triggers **Fast Retransmit** and **Fast Recovery**: $ssthresh = cwnd / 2$, $cwnd = ssthresh + 3 \text{ MSS}$.

---

# MODULE 5: APPLICATION LAYER & SOCKET PROGRAMMING

## 5.1 Application Layer Protocols
* **DNS (Domain Name System)**: Translates human domain names (e.g., `vtu.ac.in`) to IP addresses. Operates over UDP port 53.
* **HTTP / HTTPS**: Request-Response protocol on port 80 / 443. Methods: `GET`, `POST`, `PUT`, `DELETE`.
* **SMTP / POP3 / IMAP**: Push and pull email delivery systems.

---

## 5.2 Python Socket Programming Implementation

### Server Program (`tcp_server.py`):
```python
import socket

SERVER_HOST = '127.0.0.1'
SERVER_PORT = 65432

# Create IPv4 TCP Streaming Socket
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as server_socket:
    server_socket.bind((SERVER_HOST, SERVER_PORT))
    server_socket.listen(5)
    print(f"[*] Server listening on {SERVER_HOST}:{SERVER_PORT}")
    
    conn, addr = server_socket.accept()
    with conn:
        print(f"[+] Connected by {addr}")
        while True:
            data = conn.recv(1024)
            if not data:
                break
            print(f"[Received]: {data.decode('utf-8')}")
            # Echo back uppercase
            conn.sendall(data.upper())
```

### Client Program (`tcp_client.py`):
```python
import socket

SERVER_HOST = '127.0.0.1'
SERVER_PORT = 65432

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as client_socket:
    client_socket.connect((SERVER_HOST, SERVER_PORT))
    message = "Hello VTU MCA Networks"
    client_socket.sendall(message.encode('utf-8'))
    
    response = client_socket.recv(1024)
    print(f"[Server Response]: {response.decode('utf-8')}")
```

---

# 🎯 Model Exam Questions with Step-by-Step Solutions

### Q1. [Module 1 - 10 Marks]
**A bit stream `1101011011` is transmitted using the standard CRC method. The generator polynomial is $G(x) = x^4 + x + 1$.**
1. Determine the transmitted bit sequence.
2. If the 3rd bit from left is inverted during transmission, show how the receiver detects the error.

**Solution:**
1. **Binary Representation of Generator**:
   $G(x) = 1 \cdot x^4 + 0 \cdot x^3 + 0 \cdot x^2 + 1 \cdot x^1 + 1 \cdot x^0 \implies \mathbf{10011}$.
   Degree of generator $r = 4$.

2. **Append $r = 4$ Zeros to Data Stream**:
   Data $D = 1101011011 \implies \text{Dividend } = \mathbf{11010110110000}$.

3. **Modulo-2 Division (XOR Arithmetic)**:
   ```
             1100001010
   10011 ) 11010110110000
           10011
           -----
            10011
            10011
            -----
            000010110000
                10011
                -----
                 01010000
                  10011
                  -----
                   01110
   ```
   * Step-by-step subtraction:
     * $11010 \oplus 10011 = 01001$ $\to$ bring down $1 \implies 10011$
     * $10011 \oplus 10011 = 00000$ $\to$ bring down $1, 0, 1, 1 \implies 01011$
     * $10110 \oplus 10011 = 00101$ $\to$ bring down $0 \implies 01010$
     * $10100 \oplus 10011 = 00111$ $\to$ bring down $0 \implies 01110$
   * **Remainder (Checksum)** $R = \mathbf{1110}$.

4. **Transmitted Frame**:
   $\text{Transmitted Frame } T = \text{Data} + \text{Checksum} = \mathbf{11010110111110}$.

5. **Receiver Error Detection**:
   * Received data with 3rd bit inverted: $11\mathbf{1}10110111110$.
   * Dividing this by $10011$ yields a **non-zero remainder** ($\neq 0000$).
   * Because the remainder is non-zero, the receiver rejects the frame as corrupted.

---

### Q2. [Module 3 - 10 Marks]
**An ISP grants an organization the network address `200.100.50.0/24`. The organization requires 4 equal subnets.**
1. What is the new subnet mask?
2. For each subnet, find the Subnet Address, Directed Broadcast Address, and Range of Usable Host IPs.

**Solution:**
1. **Subnet Mask Calculation**:
   * To create 4 subnets: $2^k \ge 4 \implies k = 2$ bits borrowed from host portion.
   * New Prefix Length = $24 + 2 = \mathbf{/26}$.
   * New Subnet Mask = $11111111.11111111.11111111.11000000 = \mathbf{255.255.255.192}$.
   * Block size per subnet = $2^{32 - 26} = 2^6 = \mathbf{64}$ IP addresses.

2. **Subnet Distribution Table**:

| Subnet # | Subnet Network ID | First Usable Host IP | Last Usable Host IP | Directed Broadcast Address |
| :---: | :---: | :---: | :---: | :---: |
| **Subnet 1** | `200.100.50.0` | `200.100.50.1` | `200.100.50.62` | `200.100.50.63` |
| **Subnet 2** | `200.100.50.64` | `200.100.50.65` | `200.100.50.126` | `200.100.50.127` |
| **Subnet 3** | `200.100.50.128` | `200.100.50.129` | `200.100.50.190` | `200.100.50.191` |
| **Subnet 4** | `200.100.50.192` | `200.100.50.193` | `200.100.50.254` | `200.100.50.255` |
