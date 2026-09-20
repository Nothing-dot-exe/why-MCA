# VTU MCA 2022/2024 Scheme - Operating Systems (22MCA12)
## Full 100-Mark Solved Examination Paper with Step-by-Step Solutions
Time: 3 Hours | Max Marks: 100

================================================================================
MODULE 1: OPERATING SYSTEM STRUCTURES & PROCESS MANAGEMENT
================================================================================

Q.1 (a) Explain Operating System Services and Dual-Mode Operation (User Mode vs Kernel Mode). [10 Marks]
Answer:
Operating System Services:
1. Program Execution: Loads program into memory, runs it, and handles normal/abnormal termination.
2. I/O Operations: Provides an abstraction layer so applications do not interact with raw hardware drivers directly.
3. File System Manipulation: Reading, writing, creating, deleting, and managing directory permissions.
4. Communications: Inter-process communication (IPC) via shared memory or message passing.
5. Error Detection & Handling: Handling CPU/memory errors, division by zero, network drops.

Dual-Mode Operation:
To prevent rogue user applications from crashing the machine, hardware provides a Mode Bit:
- Mode Bit = 1: User Mode (restricted CPU instruction set; cannot access raw memory or I/O).
- Mode Bit = 0: Kernel Mode / Privileged Mode (full CPU instruction set allowed).
Transition: When a user app requests a service via System Call (e.g. read(), fork()), hardware raises a software interrupt (trap), switching mode bit from 1 to 0. OS executes the system call in kernel mode, resets mode bit to 1, and returns control to the user app.

--------------------------------------------------------------------------------
Q.1 (b) Explain Process State Diagram and Process Control Block (PCB) structure. [10 Marks]
Answer:
Process States:
1. New: Process is being created.
2. Ready: Process is in main memory waiting to be assigned to CPU by scheduler.
3. Running: Instructions are being executed on the CPU core.
4. Waiting / Blocked: Process waiting for I/O event or signal completion.
5. Terminated: Process finished execution and OS reclaims its resources.

Process Control Block (PCB) Fields:
- Process ID (PID)
- Process State (Ready, Running, Waiting)
- Program Counter (PC): Address of next instruction to execute
- CPU Registers: Accumulator, index registers, stack pointer
- CPU Scheduling Info: Priority, quantum timer
- Memory Management Info: Page tables, base/limit registers
- I/O Status Info: List of allocated devices and open files

================================================================================
MODULE 2: CPU SCHEDULING & PROCESS SYNCHRONIZATION
================================================================================

Q.3 (a) Calculate Average Waiting Time and Turnaround Time for the following processes using:
(i) FCFS (ii) SJF (Non-preemptive) (iii) Round Robin (Quantum = 2ms)
Process | Burst Time (ms)
P1      | 6
P2      | 8
P3      | 7
P4      | 3
Arrival Times: All arrive at t = 0. [10 Marks]
Answer:
(i) First-Come First-Served (FCFS):
Gantt Chart: | P1 (0-6) | P2 (6-14) | P3 (14-21) | P4 (21-24) |
Waiting Times:
P1 = 0 ms
P2 = 6 ms
P3 = 14 ms
P4 = 21 ms
Average Waiting Time = (0 + 6 + 14 + 21) / 4 = 41 / 4 = 10.25 ms.
Turnaround Times (TAT = WT + Burst):
P1 = 6 ms, P2 = 14 ms, P3 = 21 ms, P4 = 24 ms.
Average Turnaround Time = (6 + 14 + 21 + 24) / 4 = 65 / 4 = 16.25 ms.

(ii) Shortest Job First (SJF Non-preemptive):
Order of execution by burst time: P4(3), P1(6), P3(7), P2(8).
Gantt Chart: | P4 (0-3) | P1 (3-9) | P3 (9-16) | P2 (16-24) |
Waiting Times:
P4 = 0 ms
P1 = 3 ms
P3 = 9 ms
P2 = 16 ms
Average Waiting Time = (0 + 3 + 9 + 16) / 4 = 28 / 4 = 7.00 ms.
Turnaround Times:
P4 = 3, P1 = 9, P3 = 16, P2 = 24.
Average Turnaround Time = (3 + 9 + 16 + 24) / 4 = 52 / 4 = 13.00 ms.

(iii) Round Robin (Quantum = 2 ms):
Ready Queue Order: P1, P2, P3, P4
Execution intervals:
P1 (0-2), P2 (2-4), P3 (4-6), P4 (6-8),
P1 (8-10), P2 (10-12), P3 (12-14), P4 (14-15 -> finishes),
P1 (15-17 -> finishes), P2 (17-19), P3 (19-21),
P2 (21-23), P3 (23-24 -> finishes), P2 (24-25 -> finishes).

Completion Times: P4 = 15, P1 = 17, P3 = 24, P2 = 25.
Waiting Times (TAT - Burst):
P1 = 17 - 6 = 11 ms
P2 = 25 - 8 = 17 ms
P3 = 24 - 7 = 17 ms
P4 = 15 - 3 = 12 ms
Average Waiting Time = (11 + 17 + 17 + 12) / 4 = 57 / 4 = 14.25 ms.

================================================================================
MODULE 3: DEADLOCKS & MEMORY MANAGEMENT
================================================================================

Q.5 (a) State the Four Necessary Conditions for Deadlock. Explain Banker's Algorithm with a safety check example. [10 Marks]
Answer:
Four Necessary Conditions (Coffman Conditions):
1. Mutual Exclusion: At least one resource must be held in a non-shareable mode.
2. Hold and Wait: A process holds at least one resource and waits for additional resources held by others.
3. No Preemption: Resources cannot be forcibly confiscated from a process; only released voluntarily.
4. Circular Wait: A closed chain of processes P0, P1, ..., Pn exists such that P0 waits for P1, and Pn waits for P0.

Banker's Algorithm:
Given 3 resource types: A(10), B(5), C(7).
Available Vector: Available = Total - sum(Allocation).
Need Matrix = Max - Allocation.
A process Pi can be executed if Need_i <= Available. Once executed, Available = Available + Allocation_i.
If all processes complete, the system is in a SAFE STATE.
