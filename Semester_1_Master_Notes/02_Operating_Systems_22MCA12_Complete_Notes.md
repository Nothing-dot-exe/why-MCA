# Master Study Notes: Operating System Concepts & UNIX/Linux
## Course Code: 22MCA12 / MMC104 | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: OS Functions & Services, Dual-Mode Operation, System Calls, Process Concepts, States & State Transitions, Process Control Block (PCB), Context Switching, Process Creation/Termination (`fork()`, `exec()`, `wait()`, `exit()`), Inter-Process Communication (IPC).
* **Module 2**: CPU Scheduling Criteria & Algorithms (FCFS, SJF, Priority, Round Robin), Process Synchronization, Critical Section Problem, Peterson's Algorithm, Mutexes, Semaphores, Classic Synchronization Problems (Producer-Consumer, Readers-Writers, Dining Philosophers).
* **Module 3**: Deadlocks, 4 Necessary Conditions, Resource Allocation Graph (RAG), Deadlock Prevention, Avoidance (Banker's Safety & Request Algorithm), Detection & Recovery. Memory Management, Logical vs Physical Address Space, Paging, Page Table, TLB, Segmentation.
* **Module 4**: Virtual Memory, Demand Paging, Page Fault Handling, Page Replacement Algorithms (FIFO, Optimal, LRU, Second-Chance), Belady's Anomaly, Thrashing & Working-Set Model.
* **Module 5**: Secondary Storage Management, Disk Scheduling Algorithms (FCFS, SSTF, SCAN, C-SCAN, LOOK), RAID Levels (0, 1, 5, 10). Linux OS Architecture, Kernel vs Shell, File Permissions, Bash Shell Scripting & Essential System Administration Commands.

---

# MODULE 1: OS STRUCTURES & PROCESS MANAGEMENT

## 1.1 Operating System Services
1. **Program Execution**: Allocating memory, loading executables, handling CPU execution, cleanup upon exit.
2. **I/O Operations**: Abstracting device communication via standard device drivers.
3. **File System Manipulation**: Hierarchical storage, file creation/deletion, read/write/append permissions.
4. **Communications**: Inter-process communication across processes via **Shared Memory** or **Message Passing**.
5. **Error Detection & Recovery**: Detecting hardware glitches, memory bounds violations, infinite loops.
6. **Resource Allocation & Protection**: Multi-user CPU timesharing and memory partitioning.

## 1.2 Dual-Mode Operation
Modern CPU architectures implement hardware protection using a **Mode Bit**:
* **User Mode (Mode Bit = 1)**: User applications execute here. Privileged CPU instructions (e.g., direct hardware I/O, disabling interrupts) are forbidden.
* **Kernel / Privileged Mode (Mode Bit = 0)**: Operating system kernel executes here with complete unrestricted access to all CPU instructions and physical hardware.

```
+------------------+                    +--------------------+
|    User Mode     | --- System Call -> |    Kernel Mode     |
|  (Mode Bit = 1)  | <- Return/Trap --- |  (Mode Bit = 0)    |
+------------------+                    +--------------------+
```
* **System Call Execution**:
  1. User program pushes arguments and executes a software interrupt (`trap` / `syscall`).
  2. CPU hardware switches the mode bit to 0 and jumps to the kernel's interrupt vector table.
  3. The OS kernel executes the requested privileged routine (e.g., `sys_read()`).
  4. The OS resets the mode bit to 1 and returns control back to the user application.

## 1.3 Process State Lifecycle & PCB
A **Process** is a program in execution.

### Five-State Process Lifecycle:
1. **New**: The process is being loaded into memory.
2. **Ready**: Waiting in the ready queue to be allocated CPU time by the scheduler.
3. **Running**: Instructions are currently being executed on the CPU core.
4. **Waiting / Blocked**: Waiting for an event or I/O operation to complete.
5. **Terminated**: Finished execution; OS reclaims memory and data structures.

### Process Control Block (PCB) Fields:
* **Process ID (PID)**: Unique integer assigned by the OS.
* **Process State**: Current state (Ready, Running, Waiting, etc.).
* **Program Counter (PC)**: Address of the next instruction to execute.
* **CPU Registers**: Accumulators, stack pointers, index registers saved during context switch.
* **CPU Scheduling Information**: Process priority, pointer to scheduling queues.
* **Memory Management Information**: Page tables, segment tables, base and limit registers.
* **Accounting Information**: CPU execution time used, time limits.
* **I/O Status Information**: List of open file descriptors and allocated I/O devices.

## 1.4 Process Operations & System Calls
* `fork()`: Creates an exact child duplicate process. Returns `0` to the child, returns `Child PID` to the parent, and returns negative on error.
* `exec()`: Replaces the current process image with a new executable program.
* `wait()`: Parent suspends execution until one of its child processes terminates.
* `exit()`: Terminates the process and returns an exit code to the parent.
* **Zombie Process**: A process that has terminated via `exit()`, but its parent has not yet called `wait()`.
* **Orphan Process**: A child process whose parent process terminated without waiting. The `init` / `systemd` process (PID 1) adopts orphans.

---

# MODULE 2: CPU SCHEDULING & PROCESS SYNCHRONIZATION

## 2.1 Scheduling Metrics
* **Arrival Time ($AT$)**: Time when process enters the Ready Queue.
* **Burst Time ($BT$)**: CPU time required for completion.
* **Completion Time ($CT$)**: Time when process finishes execution.
* **Turnaround Time ($TAT$)**: $TAT = CT - AT$
* **Waiting Time ($WT$)**: $WT = TAT - BT$
* **Response Time ($RT$)**: Time from arrival to first CPU allocation.

---

## 2.2 Process Synchronization & The Critical Section Problem
A **Critical Section** is a code segment where shared variables, tables, or files are accessed and modified.

### Three Mandatory Requirements:
1. **Mutual Exclusion**: If process $P_i$ is executing in its critical section, no other processes can be executing in their critical sections.
2. **Progress**: If no process is in its critical section and some wish to enter, only those processes not in their remainder section can participate in deciding who enters next.
3. **Bounded Waiting**: There must be a limit on the number of times other processes are allowed to enter their critical sections after a process has made a request to enter.

### Semaphores:
A **Semaphore** $S$ is an integer variable accessed only through two atomic operations:
* `wait(S)` (or `P(S)`):
  ```c
  wait(S) {
      while (S <= 0) ; // busy wait
      S--;
  }
  ```
* `signal(S)` (or `V(S)`):
  ```c
  signal(S) {
      S++;
  }
  ```
* **Counting Semaphore**: $S \in [0, \infty)$, used to control access to a finite pool of resources.
* **Binary Semaphore (Mutex)**: $S \in \{0, 1\}$, provides mutual exclusion.

---

# MODULE 3: DEADLOCKS & MEMORY MANAGEMENT

## 3.1 Four Coffman Conditions for Deadlock
A deadlock can arise if and only if all four conditions hold simultaneously:
1. **Mutual Exclusion**: At least one resource must be held in a non-shareable mode.
2. **Hold and Wait**: A process must currently hold at least one resource and be waiting to acquire additional resources held by others.
3. **No Preemption**: Resources cannot be preempted; they can only be released voluntarily by the holding process.
4. **Circular Wait**: A closed chain of processes $\{P_0, P_1, \dots, P_n\}$ exists such that $P_0$ waits for a resource held by $P_1$, $P_1$ waits for $P_2$, and $P_n$ waits for $P_0$.

## 3.2 Banker's Deadlock Avoidance Algorithm
Let $n$ = number of processes, $m$ = number of resource types.
* **Available $[m]$**: Available instances of each resource.
* **Max $[n \times m]$**: Maximum demand of each process.
* **Allocation $[n \times m]$**: Currently allocated resources to each process.
* **Need $[n \times m]$**: Remaining resource requirement:
  $$\text{Need}[i][j] = \text{Max}[i][j] - \text{Allocation}[i][j]$$

### Safety Algorithm:
1. Initialize $\text{Work} = \text{Available}$, and $\text{Finish}[i] = \text{False}$ for $i = 0, \dots, n-1$.
2. Find an index $i$ such that:
   $$\text{Finish}[i] == \text{False} \quad \text{and} \quad \text{Need}_i \le \text{Work}$$
   If no such $i$ exists, go to step 4.
3. $\text{Work} = \text{Work} + \text{Allocation}_i$, $\text{Finish}[i] = \text{True}$. Repeat Step 2.
4. If $\text{Finish}[i] == \text{True}$ for all $i$, the system is in a **Safe State**.

---

## 3.3 Paging Architecture
* **Physical Memory**: Divided into fixed-size blocks called **Frames**.
* **Logical Address Space**: Divided into blocks of the same size called **Pages**.
* **Address Translation**: Logical address $(p, d)$ where $p$ is page number and $d$ is page offset.
  * $p$ is used as an index into the **Page Table** to find frame base address $f$.
  * Physical address is $(f \times \text{Frame Size}) + d$.
* **Translation Lookaside Buffer (TLB)**: High-speed associative hardware cache storing recently used page-to-frame translations.
* **Effective Access Time (EAT)**:
  $$\text{EAT} = h \times (t_{\text{TLB}} + t_{\text{mem}}) + (1 - h) \times (t_{\text{TLB}} + 2 \times t_{\text{mem}})$$
  Where $h$ is the TLB Hit Ratio.

---

# MODULE 4: VIRTUAL MEMORY & PAGE REPLACEMENT

## 4.1 Demand Paging & Page Faults
Pages are loaded into physical memory only when accessed during execution.
* If a process accesses a page marked *invalid* in the page table, the CPU generates a **Page Fault Trap**:
  1. OS checks internal tables to verify if memory access was valid.
  2. OS finds a free physical frame (or selects a victim page via page replacement).
  3. Disk I/O is scheduled to read the required page into the frame.
  4. Page table is updated (valid bit set to 1).
  5. Interrupted instruction is restarted.

## 4.2 Page Replacement Algorithms
1. **FIFO (First-In, First-Out)**: Replaces the oldest page loaded. Suffers from **Belady's Anomaly** (increasing frame count can sometimes increase page faults).
2. **Optimal Page Replacement (OPT)**: Replaces the page that will not be used for the longest period of time in the future. (Theoretical benchmark; impossible to implement in real OS).
3. **LRU (Least Recently Used)**: Replaces the page that has not been accessed for the longest period in the past. Implemented using Counters or Doubly Linked Stacks.

---

# MODULE 5: STORAGE MANAGEMENT & LINUX SHELL

## 5.1 Disk Scheduling Algorithms
* **FCFS**: Services requests in arrival order.
* **SSTF (Shortest Seek Time First)**: Selects request with minimum seek time from current head position (risk of starvation).
* **SCAN (Elevator)**: Disk arm moves in one direction servicing all requests until it hits the end, then reverses direction.
* **C-SCAN (Circular SCAN)**: Moves in one direction servicing requests; upon hitting the end, immediately returns to the beginning without servicing requests on the return trip.
* **LOOK / C-LOOK**: Arm only travels as far as the final request in each direction before reversing/returning.

## 5.2 Linux File Permissions & Essential Shell Scripting
* Permission bits: `r` (read = 4), `w` (write = 2), `x` (execute = 1).
  * `chmod 754 script.sh` $\implies$ Owner: `rwx` (7), Group: `r-x` (5), Others: `r--` (4).

---

# 🎯 Model Exam Questions with Step-by-Step Solutions

### Q1. [Module 2 - 10 Marks]
**Given 5 processes with arrival times and CPU burst times, calculate Average Waiting Time and Turnaround Time using (i) FCFS, (ii) SJF Non-preemptive, and (iii) Round Robin ($q = 2\text{ ms}$). All arrive at $t = 0$.**

| Process | Burst Time ($BT$) |
| :---: | :---: |
| $P_1$ | 6 ms |
| $P_2$ | 8 ms |
| $P_3$ | 7 ms |
| $P_4$ | 3 ms |

**Solution:**

#### (i) FCFS (First-Come First-Served):
* **Gantt Chart**: `| P1 (0-6) | P2 (6-14) | P3 (14-21) | P4 (21-24) |`
* Completion Times ($CT$): $P_1 = 6, P_2 = 14, P_3 = 21, P_4 = 24$.
* Waiting Times ($WT = CT - BT$):
  * $WT(P_1) = 6 - 6 = 0$
  * $WT(P_2) = 14 - 8 = 6$
  * $WT(P_3) = 21 - 7 = 14$
  * $WT(P_4) = 24 - 3 = 21$
* **Average Waiting Time**: $\frac{0 + 6 + 14 + 21}{4} = \frac{41}{4} = \mathbf{10.25\text{ ms}}$.
* **Average Turnaround Time**: $\frac{6 + 14 + 21 + 24}{4} = \frac{65}{4} = \mathbf{16.25\text{ ms}}$.

#### (ii) SJF Non-Preemptive:
* Order by shortest burst: $P_4 (3) \to P_1 (6) \to P_3 (7) \to P_2 (8)$.
* **Gantt Chart**: `| P4 (0-3) | P1 (3-9) | P3 (9-16) | P2 (16-24) |`
* Waiting Times:
  * $WT(P_4) = 0$
  * $WT(P_1) = 3$
  * $WT(P_3) = 9$
  * $WT(P_2) = 16$
* **Average Waiting Time**: $\frac{0 + 3 + 9 + 16}{4} = \frac{28}{4} = \mathbf{7.0\text{ ms}}$.

---

### Q2. [Module 3 - 10 Marks]
**A system has 5 processes $\{P_0, P_1, P_2, P_3, P_4\}$ and 3 resource types $A (10), B (5), C (7)$. Given the snapshot below, verify if the system is in a Safe State and find the safe execution sequence.**

**Allocation Matrix**:
* $P_0$: $[0, 1, 0]$ | **Max**: $[7, 5, 3]$
* $P_1$: $[2, 0, 0]$ | **Max**: $[3, 2, 2]$
* $P_2$: $[3, 0, 2]$ | **Max**: $[9, 0, 2]$
* $P_3$: $[2, 1, 1]$ | **Max**: $[2, 2, 2]$
* $P_4$: $[0, 0, 2]$ | **Max**: $[4, 3, 3]$

Total Allocated = $[7, 2, 5]$.  
**Available** = $[10-7, 5-2, 7-5] = [3, 3, 2]$.

**Solution:**
1. **Compute Need Matrix ($\text{Need} = \text{Max} - \text{Alloc}$)**:
   * $\text{Need}(P_0) = [7, 4, 3]$
   * $\text{Need}(P_1) = [1, 2, 2]$
   * $\text{Need}(P_2) = [6, 0, 0]$
   * $\text{Need}(P_3) = [0, 1, 1]$
   * $\text{Need}(P_4) = [4, 3, 1]$

2. **Step-by-Step Execution Trace**:
   * Current $\text{Work} = [3, 3, 2]$.
   * Check $P_1$: $\text{Need}(P_1) = [1, 2, 2] \le [3, 3, 2]$. **Satisfied!**
     * $\text{Work} = [3, 3, 2] + [2, 0, 0] = [5, 3, 2]$. Sequence: $\langle P_1 \rangle$.
   * Check $P_3$: $\text{Need}(P_3) = [0, 1, 1] \le [5, 3, 2]$. **Satisfied!**
     * $\text{Work} = [5, 3, 2] + [2, 1, 1] = [7, 4, 3]$. Sequence: $\langle P_1, P_3 \rangle$.
   * Check $P_4$: $\text{Need}(P_4) = [4, 3, 1] \le [7, 4, 3]$. **Satisfied!**
     * $\text{Work} = [7, 4, 3] + [0, 0, 2] = [7, 4, 5]$. Sequence: $\langle P_1, P_3, P_4 \rangle$.
   * Check $P_0$: $\text{Need}(P_0) = [7, 4, 3] \le [7, 4, 5]$. **Satisfied!**
     * $\text{Work} = [7, 4, 5] + [0, 1, 0] = [7, 5, 5]$. Sequence: $\langle P_1, P_3, P_4, P_0 \rangle$.
   * Check $P_2$: $\text{Need}(P_2) = [6, 0, 0] \le [7, 5, 5]$. **Satisfied!**
     * $\text{Work} = [7, 5, 5] + [3, 0, 2] = [10, 5, 7]$. Sequence: $\langle P_1, P_3, P_4, P_0, P_2 \rangle$.

**Conclusion**: The system is in a **Safe State** with Safe Sequence:
$$\mathbf{\langle P_1, P_3, P_4, P_0, P_2 \rangle}$$
