# 🌐 QB 08: Computer Networks — Question Bank with Answer Keys

> **Course:** Computer Networks  
> **Target:** VTU MCA Semester 2 (PCC)  
> **Scheme:** VTU 2022 / 2024 Scheme  
> **Contents:** OSI vs TCP/IP, CRC Error Detection Numericals, IP Subnetting & CIDR Calculations, TCP 3-Way Handshake, Routing Algorithms, 15 MCQs with Explanations

---

## 📑 Syllabus Outline (VTU 5 Modules)
- **Module 1:** Network Topologies, OSI 7-Layer Model vs TCP/IP Model, Physical Media, Transmission Impairments.
- **Module 2:** Data Link Layer: Framing, Flow Control (Stop-and-Wait, Go-Back-N, Selective Repeat), Error Control (CRC, Hamming Code), CSMA/CD.
- **Module 3:** Network Layer: IPv4 Addressing, Subnetting & CIDR, IPv6, Routing Algorithms (Distance Vector Routing, Link State Routing), NAT, ARP/ICMP.
- **Module 4:** Transport Layer: UDP vs TCP, TCP 3-Way Handshake & Connection Termination, Flow Control, Congestion Control (Slow Start, AIMD).
- **Module 5:** Application Layer & Network Security: DNS, HTTP/HTTPS, SMTP, SSL/TLS, Firewalls, Symmetric vs Asymmetric Cryptography.

---

## 🎯 SECTION 1: High-Yield MCQs with Answer Key

### Q1. Which layer of the OSI model handles data compression, encryption, and syntax translation?
- A) Session Layer
- B) Presentation Layer
- C) Transport Layer
- D) Application Layer  
**Answer: B**  
**Explanation:** The Presentation Layer (Layer 6) is responsible for syntax conversion, data formatting, encryption/decryption, and compression.

---

### Q2. What is the size of an IPv4 address and an IPv6 address respectively?
- A) 16 bits and 32 bits
- B) 32 bits and 128 bits
- C) 64 bits and 128 bits
- D) 32 bits and 64 bits  
**Answer: B**  
**Explanation:** IPv4 addresses are 32 bits (4 octets); IPv6 addresses are 128 bits (16 octets / 8 hexadecimal quartets).

---

### Q3. How many usable host IP addresses are available in a `/26` IPv4 subnet?
- A) 64
- B) 62
- C) 30
- D) 126  
**Answer: B**  
**Explanation:** A `/26` prefix leaves $32 - 26 = 6$ host bits. Total addresses = $2^6 = 64$. Subtracting 2 (Network ID and Broadcast ID) leaves $64 - 2 = \mathbf{62}$ usable host IPs.

---

### Q4. Which transport layer protocol is connectionless, does not guarantee delivery, but provides minimal latency for real-time video/gaming?
- A) TCP
- B) UDP
- C) SCTP
- D) ICMP  
**Answer: B**  
**Explanation:** UDP (User Datagram Protocol) avoids handshake overhead and retransmissions, making it ideal for low-latency streaming and DNS lookups.

---

### Q5. What is the standard flag sequence sent by the client to initiate a TCP 3-Way Handshake?
- A) `ACK`
- B) `SYN`
- C) `SYN-ACK`
- D) `FIN`  
**Answer: B**  
**Explanation:** The client begins connection setup by sending a packet with the `SYN` (Synchronize) control flag set and an initial sequence number ($ISN_C$).

---

### Q6. What protocol resolves an IP address into a Physical MAC address on a local area network?
- A) DNS
- B) DHCP
- C) ARP (Address Resolution Protocol)
- D) RARP  
**Answer: C**  
**Explanation:** ARP broadcasts a query asking "Who has IP $X.X.X.X$?" and receives a unicast reply with the corresponding hardware MAC address.

---

### Q7. In Cyclic Redundancy Check (CRC), if the generator polynomial is of degree $k$, how many zero bits are appended to the data frame before division?
- A) $k - 1$
- B) $k$
- C) $k + 1$
- D) $2k$  
**Answer: B**  
**Explanation:** Exactly $k$ zero bits (equal to the degree of the generator polynomial) are appended to the data bits prior to binary modulo-2 division.

---

### Q8. Which TCP congestion control phase exponentially doubles the congestion window (`cwnd`) every Round-Trip Time (RTT)?
- A) Congestion Avoidance
- B) Slow Start
- C) Fast Recovery
- D) Time-Wait  
**Answer: B**  
**Explanation:** During Slow Start, `cwnd` begins at 1 MSS and doubles every RTT ($1 \to 2 \to 4 \to 8 \dots$) until it hits the `ssthresh` (slow start threshold).

---

### Q9. On which port does HTTPS (HTTP Secure) listen by default?
- A) Port 80
- B) Port 21
- C) Port 443
- D) Port 53  
**Answer: C**  
**Explanation:** Plaintext HTTP uses port 80; encrypted HTTPS over TLS uses port 443. (DNS uses 53, FTP uses 21).

---

### Q10. What is the "Count-to-Infinity" problem associated with?
- A) Link State Routing
- B) Distance Vector Routing (Bellman-Ford)
- C) OSPF
- D) Spanning Tree Protocol  
**Answer: B**  
**Explanation:** In Distance Vector routing, when a link fails, routing updates circulate in a slow incrementing loop because routers lack global topology vision (mitigated by Split Horizon and Poison Reverse).

---

## 🏛️ SECTION 2: Short Answer Concepts (4–6 Marks)

### Q11. Compare the OSI 7-Layer Reference Model with the TCP/IP Protocol Architecture.
**Answer:**

```text
          OSI MODEL (7 Layers)               TCP/IP MODEL (4/5 Layers)
     +-----------------------------+       +-----------------------------+
  7  |      Application Layer      |  \    |                             |
  6  |     Presentation Layer      |   --> |      Application Layer      |
  5  |        Session Layer        |  /    |  (HTTP, DNS, SSH, SMTP)     |
     +-----------------------------+       +-----------------------------+
  4  |       Transport Layer       | ----> |       Transport Layer       |
     |          (TCP, UDP)         |       |          (TCP, UDP)         |
     +-----------------------------+       +-----------------------------+
  3  |        Network Layer        | ----> |       Internet Layer        |
     |         (IP, ICMP)          |       |         (IPv4, IPv6)        |
     +-----------------------------+       +-----------------------------+
  2  |       Data Link Layer       |  \    |     Network Access Layer    |
     +-----------------------------+   --> |   (Ethernet, Wi-Fi, MAC)    |
  1  |       Physical Layer        |  /    |                             |
     +-----------------------------+       +-----------------------------+
```

| Criterion | OSI Model | TCP/IP Model |
|---|---|---|
| **Approach** | Theoretical conceptual model designed by ISO. | Practical implementation-first model (Internet backbone). |
| **Layers** | 7 Layers. | 4 Layers (or 5 layers in modern hybrid teaching). |
| **Separation** | Strictly separates services, interfaces, and protocols. | Does not strictly separate protocols from services. |
| **Session & Presentation**| Has dedicated Session and Presentation layers. | Functions handled directly inside the Application layer. |

---

### Q12. Explain the TCP 3-Way Handshake connection establishment with a sequence diagram.
**Answer:**

```text
     CLIENT                                          SERVER
  (CLOSED / LISTEN)                                 (LISTEN)
       |                                               |
       |  1. SYN (seq = x)                             |
       |---------------------------------------------->|  Server receives SYN,
       |                                               |  allocates TCB buffer
       |                                               |
       |  2. SYN-ACK (seq = y, ack = x + 1)            |
       |<----------------------------------------------|
       |                                               |
       |  3. ACK (ack = y + 1)                         |
       |---------------------------------------------->|  Connection Established!
       |                                               |
  (ESTABLISHED)                                   (ESTABLISHED)
       | <============= Data Transfer ===============> |
```

1. **Step 1 (SYN):** Client chooses an Initial Sequence Number ($x$) and sends a packet with `SYN=1` to the server requesting a connection.
2. **Step 2 (SYN-ACK):** Server acknowledges by sending `ACK = x + 1` and provides its own Initial Sequence Number ($y$) with flags `SYN=1, ACK=1`.
3. **Step 3 (ACK):** Client confirms receipt by sending `ACK = y + 1`. The connection is now active, and bidirectional data exchange begins.

---

## 🏛️ SECTION 3: VTU Model Numerical Problems (10–12 Marks)

### Q13. [VTU Model QP - CRC Error Detection Problem]
**A bit stream `1101011011` is transmitted using the standard CRC generator polynomial:**  
$$G(x) = x^4 + x + 1$$  
**(a) Find the transmitted frame (Codeword) using modulo-2 division.**  
**(b) If the 3rd bit from the left is inverted during transmission, show how the receiver detects the error.**

**Answer:**

#### Step 1: Generator Bit Sequence & Zero Padding
- Polynomial $G(x) = x^4 + x^1 + x^0 = 1 \cdot x^4 + 0 \cdot x^3 + 0 \cdot x^2 + 1 \cdot x^1 + 1 \cdot x^0$.  
  **Divisor:** `10011` (Degree $k = 4$, length = 5 bits).
- Data bit stream ($M$): `1101011011` (Length = 10 bits).
- Append $k = 4$ zeros to the data:  
  **Padded Data:** `11010110110000`

#### Step 2: Binary Modulo-2 Division (XOR Division)

```text
             1100001010  (Quotient)
       -------------------------
10011 ) 11010110110000
        10011
        -----
         10011
         10011
         -----
          000010110
               10011
               -----
                010100
                 10011
                 -----
                  011100
                   10011
                   -----
                    1111  <-- Remainder (CRC Checksum = 1110)
```
Let's do division step-by-step carefully:
1. `11010` XOR `10011` = `01001` $\to$ bring down `1`: `10011`
2. `10011` XOR `10011` = `00000` $\to$ bring down next bits `0, 1, 1, 0`: `01101`
3. Bring down next bit `1`: `11011` XOR `10011` = `01000`
4. Bring down `0`: `10000` XOR `10011` = `00011`
5. Bring down `0`: `001100` $\to$ Bring down `0`: `01110`
6. Final 4-bit remainder (CRC checksum) = **`1110`**

#### Step 3: Transmitted Codeword
$$\text{Codeword} = \text{Data} + \text{Remainder} = \mathbf{11010110111110}$$

#### Step 4: Receiver Error Detection
- Transmitted: `11010110111110`
- 3rd bit from left is flipped: `11`**`1`**`10110111110`
- The receiver divides the received corrupted bitstream by the same generator `10011`.
- Because the bits were flipped, the modulo-2 division yields a **non-zero remainder** ($\ne 0000$).
- Since the remainder is non-zero, the receiver **detects the error** and drops the packet.

---

### Q14. [VTU Model QP - IP Subnetting & CIDR Calculation]
**An organization is granted the IPv4 block `192.168.10.0/24`. The network administrator needs to create 4 equal-sized subnets for 4 university departments (MCA, MBA, B.Tech, Admin).**  
**For each subnet, determine:**  
**(a) Subnet Mask (Dotted Decimal & Slash notation)**  
**(b) Number of Total Addresses & Number of Usable Host IP Addresses**  
**(c) Network Address, Usable Host IP Range, and Directed Broadcast Address.**

**Answer:**

#### Step 1: Subnetting Requirements
- Base network: `192.168.10.0/24` (Class C private address).
- Original prefix length: $24$ bits.
- Number of required subnets: $4 = 2^2 \implies$ We must borrow **2 bits** from the host portion.
- New prefix length: $24 + 2 = \mathbf{/26}$.

#### Step 2: Subnet Mask
- Borrowed bits: `11000000` in the 4th octet = $128 + 64 = 192$.
- **Subnet Mask:** $\mathbf{255.255.255.192}$ (or `/26`).

#### Step 3: Address Allocations
- Host bits remaining: $32 - 26 = 6$ bits.
- **Total addresses per subnet:** $2^6 = \mathbf{64}$.
- **Usable host addresses per subnet:** $2^6 - 2 = \mathbf{62}$ (subtracting Network ID and Broadcast ID).
- Block size (Increment): **64**.

#### Step 4: Department-Wise Subnet Breakdown Table

| Subnet # | Department | Network Address | First Usable Host IP | Last Usable Host IP | Directed Broadcast Address |
|:---:|---|---|---|---|---|
| **1** | MCA Dept | `192.168.10.0` | `192.168.10.1` | `192.168.10.62` | `192.168.10.63` |
| **2** | MBA Dept | `192.168.10.64` | `192.168.10.65` | `192.168.10.126` | `192.168.10.127` |
| **3** | B.Tech Dept | `192.168.10.128` | `192.168.10.129` | `192.168.10.190` | `192.168.10.191` |
| **4** | Admin Dept | `192.168.10.192` | `192.168.10.193` | `192.168.10.254` | `192.168.10.255` |

---

## 💡 Key Takeaway Checklist for Exam Day
- [ ] For subnetting questions, remember: usable hosts is ALWAYS $2^H - 2$. Never forget to subtract 2.
- [ ] Draw the TCP 3-Way Handshake clearly with arrows, sequence numbers, and state transitions.
- [ ] In CRC problems, remember that subtraction in modulo-2 arithmetic is identical to XOR: $1 \oplus 1 = 0, \; 0 \oplus 0 = 0, \; 1 \oplus 0 = 1, \; 0 \oplus 1 = 1$.
