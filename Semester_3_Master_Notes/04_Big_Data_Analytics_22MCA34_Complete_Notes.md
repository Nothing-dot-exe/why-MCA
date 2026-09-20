# Master Study Notes: Big Data Analytics & Distributed Systems
## Course Code: 22MCA34 / MMC304 | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: Big Data Paradigm & Infrastructure (The 5 V's of Big Data, Structured vs Semi-Structured vs Unstructured Data, Distributed Storage vs Relational RDBMS, Scaling Up vs Scaling Out).
* **Module 2**: Hadoop Distributed File System - HDFS (Architecture: NameNode, DataNode, Secondary NameNode, Block Size & Replication Policy, Rack Awareness, Read/Write Data Flow Pipeline).
* **Module 3**: MapReduce Computation & YARN Architecture (Map Phase, Shuffle and Sort, Reduce Phase, Combiners, Partitioners, YARN Resource Manager, NodeManager, ApplicationMaster, Container Lifecycle).
* **Module 4**: Apache Spark & In-Memory Distributed Analytics (Spark Architecture, Resilient Distributed Datasets - RDDs, Lineage Graphs & Fault Tolerance, Narrow vs Wide Dependencies, Transformations vs Actions, Spark SQL & DataFrames).
* **Module 5**: NoSQL Databases & Real-time Streaming (CAP Theorem, BASE Properties, Columnar Storage - Apache HBase, Document Store - MongoDB, Key-Value, Distributed Streaming with Apache Kafka).

---

# MODULE 1: BIG DATA PARADIGM & SCALING

## 1.1 The 5 V's of Big Data
1. **Volume**: Massive scale of data generated (Petabytes, Exabytes from logs, sensors, social networks).
2. **Velocity**: Rate at which data is ingested and processed in real time (e.g., IoT telemetry, stock trades).
3. **Variety**: Diversity of data types:
   * *Structured*: Relational SQL tables with fixed schema.
   * *Semi-Structured*: JSON, XML, YAML documents with flexible key-value tags.
   * *Unstructured*: Raw audio, video, satellite imagery, free-form text.
4. **Veracity**: Trustworthiness, noise ratio, and quality of incoming data feeds.
5. **Value**: Business insights extracted to enable data-driven decision making.

## 1.2 Scaling Up (Vertical) vs. Scaling Out (Horizontal)
| Dimension | Scaling Up (Vertical) | Scaling Out (Horizontal / Big Data) |
| :--- | :--- | :--- |
| **Approach** | Adding more RAM/CPUs to a single high-end server | Adding commodity PC nodes to a distributed cluster |
| **Cost Curve** | Exponential (Super-servers are prohibitively expensive) | Linear (Affordable off-the-shelf commodity hardware) |
| **Hardware Limit**| Hard physical ceiling on RAM bus and CPU sockets | Virtually infinite (thousands of networked nodes) |
| **Fault Tolerance**| Single Point of Failure (SPOF) if motherboard dies | Built-in software redundancy (automatic block replication) |

---

# MODULE 2: HADOOP DISTRIBUTED FILE SYSTEM (HDFS)

## 2.1 HDFS Architecture
```
                         +-------------------+
                         |     NameNode      |  <--- Metadata Master (RAM)
                         | (FsImage + EditLog)|       Stores namespace, block maps
                         +-------------------+
                                   |
         +-------------------------+-------------------------+
         |                                                   |
+-----------------+                                 +-----------------+
|  Rack 1         |                                 |  Rack 2         |
|  +------------+ |                                 |  +------------+ |
|  |  DataNode  | |                                 |  |  DataNode  | |
|  | [B1][B2]   | |                                 |  | [B1][B3]   | |
|  +------------+ |                                 |  +------------+ |
|  +------------+ |                                 |  +------------+ |
|  |  DataNode  | |                                 |  |  DataNode  | |
|  | [B2][B3]   | |                                 |  | [B1][B2]   | |
|  +------------+ |                                 |  +------------+ |
+-----------------+                                 +-----------------+
```
* **NameNode (Master)**:
  * Manages filesystem namespace, directory tree, file permissions, and mapping of blocks to DataNodes.
  * Keeps metadata in memory for ultra-fast lookup; persists state using `FsImage` (checkpoint snapshot) and `EditLog` (transaction journal).
* **DataNode (Worker/Slave)**:
  * Stores actual physical blocks (default block size: **128 MB** in Hadoop 2.x/3.x).
  * Periodically transmits **Heartbeats** (every 3 seconds) and **Block Reports** to the NameNode.
* **Secondary NameNode**:
  * Not a hot-standby! Performs periodic "checkpointing" by downloading `FsImage` and `EditLog` from the active NameNode, merging them into an updated `FsImage`, and sending it back to prevent the EditLog from expanding uncontrollably.

## 2.2 HDFS Data Replication & Rack Awareness
* **Default Replication Factor**: 3.
* **Rack Awareness Policy**:
  * 1st Replica: Placed on a local DataNode in the same rack as the client writer.
  * 2nd Replica: Placed on a different, remote rack (protects against total rack switch failure).
  * 3rd Replica: Placed on a different node within the same remote rack (balances cross-switch network traffic).

---

# MODULE 3: MAPREDUCE & YARN ARCHITECTURE

## 3.1 MapReduce Execution Flow: WordCount
```
Input Data: ["Deer Bear River", "Car Car River", "Deer Car Bear"]
      |
[Splitting] -> Chunk 1: "Deer Bear River" | Chunk 2: "Car Car River"
      |
[Mapping]   -> (Deer, 1), (Bear, 1), (River, 1) | (Car, 1), (Car, 1), (River, 1)
      |
[Shuffle &  -> Group by key across network:
  Sorting]     Bear: [1, 1], Car: [1, 1, 1], Deer: [1, 1], River: [1, 1]
      |
[Reducing]  -> Sum values:
               (Bear, 2), (Car, 3), (Deer, 2), (River, 2)
      |
[Output]    -> Final part-r-00000 written back to HDFS.
```

## 3.2 YARN (Yet Another Resource Negotiator)
* **ResourceManager (RM)**: Cluster-wide scheduler; allocates compute resources across all competing tenant jobs via pluggable schedulers (FIFO, Capacity, Fair).
* **NodeManager (NM)**: Per-machine agent; monitors physical CPU/RAM consumption and manages resource **Containers**.
* **ApplicationMaster (AM)**: One instance per running job; negotiates containers from the RM and coordinates execution with NodeManagers.

---

# MODULE 4: APACHE SPARK & RDD IN-MEMORY ANALYTICS

## 4.1 Why Spark is 100x Faster than MapReduce
* MapReduce writes intermediate state to local disk between every Map and Reduce phase, generating massive disk I/O and network serialization overhead.
* Apache Spark processes intermediate data **in-memory (RAM)** using **Resilient Distributed Datasets (RDDs)**.

## 4.2 RDD Operations: Transformations vs Actions
* **Transformations (Lazy Evaluation)**: Creates a new RDD from an existing one. Execution is postponed until an action is invoked (e.g., `map()`, `filter()`, `flatMap()`, `groupByKey()`).
* **Actions (Eager Execution)**: Triggers the computation pipeline and returns a non-RDD value to the driver program or saves to disk (e.g., `count()`, `collect()`, `saveAsTextFile()`, `reduce()`).
* **Fault Tolerance via Lineage Graph (DAG)**: If a worker node crashes, Spark recalculates only the missing partition from its ancestral parent transformation without re-running the entire dataset.

---

# MODULE 5: NOSQL DATABASES & CAP THEOREM

## 5.1 The CAP Theorem (Brewer's Theorem)
A distributed data store can simultaneously provide at most **two out of the three** guarantees:
```
                       [ Consistency (C) ]
                             /     \
                            /   C   \
                           /  A P    \
        [ Availability (A) ] -------- [ Partition Tolerance (P) ]
```
1. **Consistency (C)**: Every read receives the most recent write or an error.
2. **Availability (A)**: Every non-failing node returns a non-error response, without guarantee that it contains the most recent write.
3. **Partition Tolerance (P)**: The system continues to operate despite arbitrary message loss or network partitions.
* *Note*: Because network partitions are inevitable in real-world distributed networks, distributed databases must choose between **CP** (e.g., MongoDB, HBase) or **AP** (e.g., Cassandra, DynamoDB).

---

# 🎯 High-Yield VTU Exam Solved Questions (10-Mark Model Answers)

### Question 1: HDFS Read & Write Workflow (VTU Dec 2023 / Jan 2024 - 10 Marks)
* Explain with a neat architectural diagram the step-by-step procedure of writing a file and reading a file in HDFS.

**Model Answer:**
1. **HDFS Write Operation**:
   * **Step 1 (Create Request)**: Client invokes `create()` on `DistributedFileSystem`.
   * **Step 2 (RPC to NameNode)**: `DistributedFileSystem` issues an RPC request to the NameNode to create a new file entry in the filesystem namespace. NameNode verifies permissions and that the file does not already exist.
   * **Step 3 (DataStreamer Pipeline)**: Client writes data to an internal buffer. `FSDataOutputStream` splits data into packets (64 KB) and requests the NameNode for a pipeline of DataNodes (e.g., DN1, DN2, DN3 based on replication policy).
   * **Step 4 (Streaming Packets)**: Client streams packets to DN1; DN1 flushes packet to local disk and forwards directly to DN2; DN2 forwards to DN3 (Pipelined write).
   * **Step 5 (Ack Queue)**: Acknowledgments flow in reverse direction (DN3 -> DN2 -> DN1 -> Client).
   * **Step 6 (Close)**: Once all blocks are written, client calls `close()`, and NameNode commits the file.

2. **HDFS Read Operation**:
   * **Step 1**: Client opens file by calling `open()` on `DistributedFileSystem`.
   * **Step 2**: RPC call to NameNode to retrieve block locations for the first few blocks of the file. NameNode returns addresses of DataNodes sorted by topological network proximity to the client.
   * **Step 3**: `FSDataInputStream` connects directly to the closest DataNode storing Block 1 and streams data to the client.
   * **Step 4**: When the block ends, stream closes connection to DN1 and seamlessly opens a connection to the nearest DataNode for Block 2.

---

### Question 2: MapReduce WordCount Numerical Execution Trace (10 Marks)
**Input Document contains 2 lines:**
* Line 1: `"big data big"`
* Line 2: `"data science"`

Trace the state of Key-Value pairs through: (i) Input Splitting, (ii) Map Phase, (iii) Combiner, (iv) Shuffle & Sort, (v) Reduce Phase.

**Step-by-Step Trace:**
1. **Input Split 1**: `"big data big"` | **Input Split 2**: `"data science"`
2. **Map Phase Output**:
   * Mapper 1: `("big", 1)`, `("data", 1)`, `("big", 1)`
   * Mapper 2: `("data", 1)`, `("science", 1)`
3. **Local Combiner (Mini-reducer on mapper nodes)**:
   * Combiner 1: `("big", 2)`, `("data", 1)`
   * Combiner 2: `("data", 1)`, `("science", 1)`
4. **Shuffle & Sort (Network partitioning & grouping by key)**:
   * Key `"big"`: `["big", [2]]`
   * Key `"data"`: `["data", [1, 1]]`
   * Key `"science"`: `["science", [1]]`
5. **Reduce Phase (Aggregator)**:
   * `reduce("big", [2])` $\to$ `("big", 2)`
   * `reduce("data", [1, 1])` $\to$ `("data", 2)`
   * `reduce("science", [1])` $\to$ `("science", 1)`
* **Final Output in HDFS**:
  ```
  big     2
  data    2
  science 1
  ```
