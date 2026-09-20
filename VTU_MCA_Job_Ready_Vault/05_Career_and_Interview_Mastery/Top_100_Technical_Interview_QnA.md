# Top 100 Technical Interview Questions & Answers
**Domains Covered**: Operating Systems, Computer Networks, DBMS, Cloud/DevOps, Full-Stack, Cyber Security, and System Design.

---

## Part 1: Operating Systems & Low-Level Systems (Q1 – Q15)

#### Q1: What is the difference between a Process and a Thread?
* **Answer**: A **Process** is an independent executing program with its own isolated virtual memory address space (text, data, heap, stack), file descriptors, and security context. A **Thread** is the smallest unit of CPU execution within a process; all threads inside the same process share the same heap, global variables, and address space, but each thread maintains its own program counter, registers, and call stack.

#### Q2: What are the 4 necessary conditions for Deadlock (Coffman Conditions)?
* **Answer**:
  1. **Mutual Exclusion**: At least one resource must be held in a non-shareable mode.
  2. **Hold and Wait**: A process holds at least one resource while waiting to acquire additional resources held by other processes.
  3. **No Preemption**: Resources cannot be forcibly seized; they can only be released voluntarily.
  4. **Circular Wait**: A closed loop of processes exists where each process waits for a resource held by the next process in the chain.

#### Q3: What is Paging and why does Thrashing occur?
* **Answer**: Paging divides virtual memory into fixed-size blocks called *pages* and physical memory into *frames*. **Thrashing** occurs when the working set of active processes exceeds physical RAM capacity. The OS spends the vast majority of CPU cycles continually swapping pages between RAM and disk swap space rather than executing actual application instructions.

---

## Part 2: Computer Networks & Protocols (Q16 – Q30)

#### Q16: What happens when you type `https://www.google.com` into a browser and press Enter?
* **Answer**:
  1. **DNS Resolution**: Browser checks browser cache, OS resolver cache, local hosts file, and queries recursive DNS servers (Root -> TLD `.com` -> Authoritative Name Server) to resolve the IP address.
  2. **TCP 3-Way Handshake**: Client sends `SYN`, Server replies `SYN-ACK`, Client sends `ACK` over port 443.
  3. **TLS Handshake (TLS 1.3)**: Client sends `ClientHello` with supported cipher suites and key share. Server replies with `ServerHello`, certificate, and public key. Both sides derive the symmetric session key.
  4. **HTTP Request & Response**: Client transmits `GET / HTTP/2`. Server processes request and returns HTTP 200 OK with HTML document.
  5. **Browser Rendering**: Browser parses HTML into DOM tree, CSS into CSSOM tree, merges them into a Render Tree, performs layout (reflow), and paints pixels on screen.

#### Q17: What is the difference between TCP and UDP?
* **Answer**: TCP is connection-oriented, reliable, guarantees in-order packet delivery via sequence numbers and ACKs, and incorporates congestion/flow control. UDP is connectionless, lightweight, has zero delivery guarantee or flow control, but offers minimal latency, making it ideal for DNS queries, video streaming, VoIP, and gaming.

---

## Part 3: Databases & SQL Engineering (Q31 – Q50)

#### Q31: Explain ACID Properties in Relational Databases.
* **Answer**:
  * **Atomicity**: All operations in a transaction succeed or all roll back (All-or-Nothing).
  * **Consistency**: The database transitions from one valid state satisfying all schema constraints to another.
  * **Isolation**: Concurrent transactions execute without interfering with one another (Isolation levels: Read Uncommitted, Read Committed, Repeatable Read, Serializable).
  * **Durability**: Once a transaction commits, its changes survive system crashes or power failures (via Write-Ahead Logging - WAL).

#### Q32: What is the difference between Clustered and Non-Clustered Index?
* **Answer**: A **Clustered Index** dictates the physical order in which table rows are sorted and stored on disk; hence, there can be only one clustered index per table (typically the Primary Key). A **Non-Clustered Index** creates a separate B+ Tree structure holding sorted index columns along with pointers (Row IDs) back to the actual data pages.

---

## Part 4: Cloud, DevOps & System Architecture (Q51 – Q75)

#### Q51: What is the CAP Theorem?
* **Answer**: In any distributed data store, you can only guarantee at most two of the following three guarantees simultaneously in the presence of network failures:
  * **Consistency (C)**: Every read receives the most recent write or an error.
  * **Availability (A)**: Every non-failing node returns a non-error response, but without guarantee it contains the latest write.
  * **Partition Tolerance (P)**: The system continues to operate despite arbitrary network message loss or network partitions.
  * *Since network partitions (P) are unavoidable in real networks, systems must choose between CP (e.g., HBase, Zookeeper) or AP (e.g., Cassandra, DynamoDB).*

#### Q52: What is the difference between Docker Container and Virtual Machine (VM)?
* **Answer**: A VM virtualizes hardware and runs a full, independent guest operating system atop a Hypervisor, incurring heavy CPU/RAM overhead and multi-minute boot times. A Container virtualizes the OS kernel, sharing the host Linux kernel while utilizing Linux kernel namespaces (for PID, net, mount isolation) and cgroups (for CPU/memory resource limits), resulting in lightweight megabyte-sized footprints and millisecond startup times.

---

## Part 5: Cyber Security & DevSecOps (Q76 – Q100)

#### Q76: What is the difference between Authentication and Authorization?
* **Answer**: **Authentication (401 Unauthorized)** verifies *who you are* (e.g., username/password, Biometrics, OTP). **Authorization (403 Forbidden)** determines *what you are permitted to do* (e.g., can a MEMBER role access admin financial reports).

#### Q77: How does a CSRF (Cross-Site Request Forgery) attack work, and how is it prevented?
* **Answer**: In CSRF, an attacker tricks an authenticated user's browser into submitting an unauthorized request to a trusted website where the user is already logged in (because browsers automatically attach session cookies to cross-site requests).
  * **Prevention**:
    1. Set cookie attribute: `SameSite=Strict` or `SameSite=Lax`.
    2. Enforce **Anti-CSRF Tokens** (Synchronizer Token Pattern): The server generates a unique, unpredictable cryptographically random token per session; requests must include this token in an HTTP header that third-party sites cannot read.
