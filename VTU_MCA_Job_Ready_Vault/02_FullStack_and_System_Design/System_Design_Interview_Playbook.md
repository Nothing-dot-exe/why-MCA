# System Design Interview Playbook: High-Level & Low-Level Design
**Objective**: Crack System Design interviews for SDE-1 / SDE-2 and Cloud Engineering roles.

---

## 1. The 4-Step System Design Interview Framework

In a 45-minute system design interview, NEVER jump straight into drawing boxes. Follow this exact 4-step framework:

```
Step 1: Requirements Clarification (5 mins)
  ├─► Functional Requirements (What features must it have?)
  ├─► Non-Functional Requirements (Latency, High Availability, Consistency)
  └─► Capacity Estimation (DAU, QPS, Storage per year, Bandwidth)
       ▼
Step 2: High-Level Design (10 mins)
  ├─► Core API Signatures (POST /api/v1/shorten, GET /{id})
  ├─► High-Level Block Diagram (Client -> Load Balancer -> Web Servers -> Cache -> DB)
  └─► Database Schema (Tables, Primary Keys, Foreign Keys)
       ▼
Step 3: Deep Dive into Core Components (20 mins)
  ├─► Data partitioning / Sharding strategy
  ├─► Caching Layer & Cache Invalidation (Cache-Aside, Write-Through)
  ├─► Asynchronous processing with Message Queues (Kafka / RabbitMQ)
  └─► Handling single points of failure (SPOF)
       ▼
Step 4: Bottlenecks & Operational Resiliency (10 mins)
  ├─► Rate Limiting (Token Bucket / Leaky Bucket)
  ├─► Circuit Breaker pattern (Resilience4j / Envoy)
  └─► Telemetry: Metrics, Distributed Tracing (Jaeger), Centralized Logs (ELK)
```

---

## 2. Core System Design Building Blocks

### A. Load Balancing Algorithms
1. **Round Robin**: Distributes requests sequentially across servers.
2. **Least Connections**: Forwards requests to the server with the fewest active sessions. Ideal for long-lived WebSocket connections.
3. **Consistent Hashing**:
   - Maps both servers and keys to a logical hash ring ($0$ to $2^{32} - 1$).
   - When a server is added or removed, only $K/N$ keys need to be remapped (where $K$ is keys, $N$ is servers), avoiding catastrophic cache stampedes.
   - Virtual nodes are used to balance traffic evenly.

### B. Caching Strategies (Redis / Memcached)
* **Cache-Aside (Lazy Loading)**:
  - App checks cache. If hit, return data. If miss, fetch from DB, populate cache, return data.
  - *Best for*: Read-heavy workloads.
* **Write-Through**:
  - App writes data to cache, and cache synchronously updates DB.
  - *Best for*: No data loss risk, slightly higher write latency.
* **Write-Behind (Write-Back)**:
  - App writes to cache immediately; cache asynchronously batches writes to DB.
  - *Best for*: High-throughput write workloads (e.g., website page-view counters).
* **Eviction Policies**: LRU (Least Recently Used), LFU (Least Frequently Used), FIFO, TTL (Time to Live).

### C. Message Queues & Event-Driven Architecture (Apache Kafka)
* **Decoupling**: The producer does not need to know who the consumer is.
* **Buffering & Rate Smoothing**: If payment gateway drops to 50 TPS during a flash sale of 5,000 TPS, Kafka queues the requests safely without dropping orders.
* **Topics, Partitions, and Consumer Groups**:
  - Each topic is split into partitions. Ordering is guaranteed *within a single partition*.
  - A consumer group allows multiple worker instances to process partitions in parallel.

---

## 3. Classic System Design Blueprint: URL Shortener (TinyURL)

### 1. Requirements
* **Functional**: Given a long URL, generate a unique 7-character short URL. Accessing the short URL redirects with HTTP 301/302.
* **Scale**: 100 Million new URLs created/month, 10:1 Read-to-Write ratio.
  - Write QPS: $100\text{M} / (30 \times 86400) \approx 40\text{ writes/sec}$.
  - Read QPS: $400\text{ reads/sec}$.
  - Storage: 100M $\times$ 500 bytes $\approx$ 50 GB/month $\rightarrow$ 3 TB over 5 years.

### 2. Short URL Key Generation
* Using Base62 encoding (`[a-z, A-Z, 0-9]`):
  - $62^6 \approx 56.8 \text{ Billion}$ unique combinations.
  - $62^7 \approx 3.5 \text{ Trillion}$ combinations. A 7-character string is optimal.
* **Key Generation Service (KGS)**:
  - Pre-generate random 7-character Base62 keys in advance and store them in a Key-DB.
  - Mark keys as `used` when requested. This eliminates runtime hash collision retries entirely.

### 3. Redirection Status Code: 301 vs 302
* **301 Permanent Redirect**: Browser caches the redirection locally. Subsequent clicks bypass the TinyURL server completely (reduces server load, but breaks analytics tracking).
* **302 Temporary Redirect**: Browser always queries TinyURL first. Crucial if you want to track analytics (click count, geographical origin, referrer header).
