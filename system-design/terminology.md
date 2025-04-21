**System design terminologies** along with their **definitions**:

---

### ✅ **1. Scalability**
**Definition:**  
The ability of a system to handle increased load by adding resources.  
- **Horizontal scaling:** Adding more machines.  
- **Vertical scaling:** Adding more power (CPU/RAM) to an existing machine.

---

### ✅ **2. Latency**
**Definition:**  
The time it takes for a request to travel from source to destination and back (round-trip time).

---

### ✅ **3. Throughput**
**Definition:**  
The number of requests a system can handle per unit time (e.g., 1000 requests/sec).

---

### ✅ **4. Load Balancer**
**Definition:**  
A component that distributes incoming traffic across multiple servers to ensure no single server gets overwhelmed.

---

### ✅ **5. Caching**
**Definition:**  
Storing copies of frequently accessed data in a faster storage layer to reduce access time.

---

### ✅ **6. CDN (Content Delivery Network)**
**Definition:**  
A network of geographically distributed servers that deliver content closer to the user's location to reduce latency.

---

### ✅ **7. Sharding**
**Definition:**  
Splitting a database into smaller chunks (shards) to distribute the load and data across multiple machines.

---

### ✅ **8. Replication**
**Definition:**  
Copying data across multiple machines to increase availability and fault tolerance.

---

### ✅ **9. Consistency**
**Definition:**  
All nodes see the same data at the same time.  
(In CAP theorem, this is one of the trade-offs.)

---

### ✅ **10. Availability**
**Definition:**  
The system remains operational and responsive, even during failures.

---

### ✅ **11. Partition Tolerance**
**Definition:**  
The system continues to function even when network partitions occur (some nodes can't talk to others).

---

### ✅ **12. CAP Theorem**
**Definition:**  
States that in any distributed system, you can only guarantee **2 out of 3**:  
- Consistency  
- Availability  
- Partition Tolerance

---

### ✅ **13. Load Shedding**
**Definition:**  
Dropping or delaying lower-priority traffic when the system is overloaded.

---

### ✅ **14. Rate Limiting**
**Definition:**  
Restricting the number of requests a user/client can make in a given timeframe to prevent abuse.

---

### ✅ **15. Queue (Message Queue)**
**Definition:**  
A buffer that temporarily holds messages or tasks to be processed asynchronously by workers (e.g., RabbitMQ, Kafka).

---

### ✅ **16. Eventual Consistency**
**Definition:**  
In distributed systems, updates propagate over time and all nodes eventually reach the same state.

---

### ✅ **17. Microservices**
**Definition:**  
A system architecture where services are small, independent, and communicate over a network.

---

### ✅ **18. Monolith**
**Definition:**  
A single, unified application where all functions are tightly coupled and deployed together.

---

### ✅ **19. Fault Tolerance**
**Definition:**  
The ability of a system to continue operating properly in the event of a failure.

---

### ✅ **20. High Availability**
**Definition:**  
Ensuring a system is always available with minimal downtime using redundancy, failovers, etc.

---

### ✅ **21. Idempotency**
**Definition:**  
An operation that produces the same result even if it is performed multiple times.

---

### ✅ **22. Backpressure**
**Definition:**  
A mechanism to slow down or pause the input when the system can’t keep up with the incoming load.

---

### ✅ **23. Heartbeat**
**Definition:**  
A periodic signal sent to check if a service or component is alive.

---

### ✅ **24. Leader Election**
**Definition:**  
A mechanism to select a single leader node among a group of nodes in a distributed system (e.g., Raft, ZooKeeper).

---

### ✅ **25. Proxy**
**Definition:**  
An intermediary server that sits between the client and the backend service to handle routing, caching, or security.

Absolutely! Here are **more advanced system design terminologies** along with their **definitions**:

---

### ✅ **26. API Gateway**
**Definition:**  
A server that acts as a single entry point into a system, routing requests to appropriate microservices, handling rate limiting, authentication, and logging.

---

### ✅ **27. Service Discovery**
**Definition:**  
A mechanism by which services automatically detect each other in a dynamic environment (e.g., using Consul, Eureka, or Kubernetes DNS).

---

### ✅ **28. Circuit Breaker**
**Definition:**  
A pattern that prevents a system from making calls to a failing service and allows it to recover gracefully (e.g., Netflix’s Hystrix).

---

### ✅ **29. Retry Mechanism**
**Definition:**  
Automatically re-attempting failed operations to handle transient errors, often used with exponential backoff.

---

### ✅ **30. Dead Letter Queue (DLQ)**
**Definition:**  
A queue where messages that could not be processed successfully are stored for later inspection or reprocessing.

---

### ✅ **31. Observability**
**Definition:**  
The ability to understand the internal state of a system based on external outputs such as logs, metrics, and traces.

---

### ✅ **32. Rate Limiter Token Bucket / Leaky Bucket**
**Definition:**  
Algorithms used to implement rate limiting.  
- **Token bucket:** allows bursts.  
- **Leaky bucket:** smoothens out bursts.

---

### ✅ **33. Webhook**
**Definition:**  
A reverse API call — the server sends a real-time HTTP callback to another system when an event occurs.

---

### ✅ **34. Throttling**
**Definition:**  
Deliberately limiting the rate of requests to a service to avoid overload or abuse.

---

### ✅ **35. Redundancy**
**Definition:**  
The duplication of components or services to improve reliability and availability.

---

### ✅ **36. Blue-Green Deployment**
**Definition:**  
A deployment strategy that reduces downtime and risk by running two identical environments (blue and green), and switching traffic when the new version is stable.

---

### ✅ **37. Canary Deployment**
**Definition:**  
Gradually rolling out a new version to a small subset of users to monitor and catch issues before full deployment.

---

### ✅ **38. Sticky Sessions**
**Definition:**  
Routing a user’s requests to the same server during a session, often used when sessions are stored in memory.

---

### ✅ **39. Consistent Hashing**
**Definition:**  
A hashing technique that helps distribute load across nodes in a way that minimizes reshuffling when nodes are added or removed.

---

### ✅ **40. TTL (Time to Live)**
**Definition:**  
The amount of time or number of hops that a piece of data (like a cache entry or DNS record) remains valid.

---

### ✅ **41. Gossip Protocol**
**Definition:**  
A method for nodes in a distributed system to share information and keep each other updated through random peer-to-peer communication.

---

### ✅ **42. Immutable Infrastructure**
**Definition:**  
Infrastructure that is not modified after deployment — if changes are needed, a new instance is created and the old one is discarded.

---

### ✅ **43. Sidecar Pattern**
**Definition:**  
A design pattern where a helper container runs alongside a main application container in a pod (common in Kubernetes), typically used for logging, proxying, or security.

---

### ✅ **44. Event Sourcing**
**Definition:**  
Instead of saving the current state, all changes (events) are stored, and the current state is rebuilt by replaying events.

---

### ✅ **45. CQRS (Command Query Responsibility Segregation)**
**Definition:**  
Separating read and write operations into different models to optimize performance, scalability, and security.

---

### ✅ **46. BFF (Backend for Frontend)**
**Definition:**  
An API layer built specifically for each frontend (web, mobile), tailoring the data and logic to its needs.

---

### ✅ **47. Federation (in GraphQL)**
**Definition:**  
Combining multiple GraphQL services into a single unified graph, allowing microservices to expose part of the overall schema.

---

### ✅ **48. Horizontal vs Vertical Partitioning**
**Definition:**  
- **Horizontal:** dividing rows (sharding).  
- **Vertical:** dividing columns (splitting features/data into different services or tables).

---

### ✅ **49. Data Lake**
**Definition:**  
A centralized repository that stores raw data in its native format (structured and unstructured), often used in big data architectures.

---

### ✅ **50. SLA, SLO, SLI**
**Definition:**  
- **SLA (Service Level Agreement):** Contractual agreement with customers.  
- **SLO (Service Level Objective):** Internal target for reliability.  
- **SLI (Service Level Indicator):** Actual measured performance.


### ✅ **51. Idempotent Operation**
**Definition:**  
An operation that has the **same effect** no matter how many times it is performed (e.g., charging a user once despite retries).

---

### ✅ **52. Fan-Out / Fan-In**
**Definition:**  
- **Fan-out:** One process triggers multiple parallel tasks.  
- **Fan-in:** Aggregating results from multiple tasks into one.

---

### ✅ **53. Singleton Pattern (in distributed systems)**
**Definition:**  
Ensuring a service or process has only **one active instance** at any given time (e.g., one cron job leader, master election).

---

### ✅ **54. Write-Ahead Log (WAL)**
**Definition:**  
A durability technique where write operations are logged before being applied, allowing recovery after a crash.

---

### ✅ **55. Paxos / Raft**
**Definition:**  
Consensus algorithms used to ensure **leader election** and **data consistency** across distributed nodes.

---

### ✅ **56. CRDT (Conflict-free Replicated Data Type)**
**Definition:**  
A data structure used in eventually consistent systems that can automatically **merge conflicting updates**.

---

### ✅ **57. Bloom Filter**
**Definition:**  
A space-efficient probabilistic data structure to test whether an element is **possibly in a set** (false positives allowed, but not false negatives).

---

### ✅ **58. GSLB (Global Server Load Balancing)**
**Definition:**  
Distributes traffic across multiple data centers or regions to improve global **latency** and **failover**.

---

### ✅ **59. Hot vs Cold vs Warm Path**
**Definition:**  
Used in real-time data pipelines:  
- **Hot path:** real-time processing  
- **Warm path:** near real-time  
- **Cold path:** batch processing (high latency)

---

### ✅ **60. LRU / LFU Cache Eviction**
**Definition:**  
- **LRU (Least Recently Used):** Evict the data that hasn’t been accessed in the longest time.  
- **LFU (Least Frequently Used):** Evict the least-used data over time.

---

### ✅ **61. Shadow Traffic**
**Definition:**  
Sending a copy of live production traffic to a new system version to test behavior without affecting real users.

---

### ✅ **62. Sticky Load Balancing**
**Definition:**  
Routing users to the **same server** during a session, often for performance or session consistency.

---

### ✅ **63. Edge Computing**
**Definition:**  
Running computation **closer to the user** (on edge devices or edge servers) to reduce latency.

---

### ✅ **64. Immutable Data**
**Definition:**  
Data that is never changed after creation—any changes produce a new version. Important for **audit logs** and **event sourcing**.

---

### ✅ **65. Saga Pattern**
**Definition:**  
A sequence of local transactions with compensating actions in case of failure — often used in **distributed transactions**.

---

### ✅ **66. Side Effect**
**Definition:**  
Any change outside the function scope (e.g., network call, DB update) — needs careful handling in retries and idempotent design.

---

### ✅ **67. Retry Storm**
**Definition:**  
A cascading failure caused by multiple clients **retrying failed requests simultaneously**, overwhelming the system.

---

### ✅ **68. Chaos Engineering**
**Definition:**  
Intentionally injecting failures into a system to test its **resilience and recovery mechanisms** (e.g., Netflix's Chaos Monkey).

---

### ✅ **69. Batching**
**Definition:**  
Grouping multiple operations together to **optimize throughput** and reduce system overhead (e.g., batch DB writes).

---

### ✅ **70. Pre-Warming**
**Definition:**  
Starting up or preparing servers/resources **in advance** to avoid cold-start latency (common in serverless systems).

---

### ✅ **71. API Rate Quota**
**Definition:**  
Total **limit over time** (e.g., 10,000 requests/day), while rate limiting is per second or minute.

---

### ✅ **72. Horizontal Pod Autoscaler (HPA)**
**Definition:**  
In Kubernetes, automatically scales pods based on CPU, memory, or custom metrics.

---

### ✅ **73. Node Affinity / Taints & Tolerations**
**Definition:**  
Kubernetes mechanisms to **control where pods run**, useful for isolating workloads or optimizing resource usage.

---

### ✅ **74. Graceful Shutdown**
**Definition:**  
Allowing a service to finish in-flight requests before shutting down, preventing **data loss or half-done operations**.

---

### ✅ **75. Vertical Slice Architecture**
**Definition:**  
Organizing code by feature instead of layers (e.g., instead of `controllers/`, `services/`, you have `user/`, `orders/` etc.).