**System Design Cheat Sheet**

---

### 🔹 Basics
- **Scalability:** Ability to handle increased load
- **Latency:** Time taken for a request to travel
- **Throughput:** Requests handled per unit time
- **Availability:** System's ability to remain operational
- **Consistency:** All nodes see the same data
- **CAP Theorem:** Pick two of: Consistency, Availability, Partition Tolerance

---

### 🚀 Load Handling
- **Load Balancer:** Distributes requests across servers
- **Rate Limiting:** Controls request frequency
- **Load Shedding:** Drops excessive/low-priority traffic
- **Backpressure:** Slows intake to prevent overload

---

### 🛋️ Data Management
- **Caching:** Store frequently accessed data (e.g., Redis)
- **TTL:** Expiry time for cached data
- **Replication:** Copies of data across nodes
- **Sharding:** Splitting data across DBs
- **Write-Ahead Log (WAL):** Logs writes before DB commit
- **Event Sourcing:** Persisting state changes as a series of events
- **CQRS:** Separating reads from writes

---

### 🤖 Architecture Patterns
- **Monolith:** All-in-one system
- **Microservices:** Small, independent services
- **BFF (Backend for Frontend):** API tailored per frontend
- **Sidecar Pattern:** Helper container for main app (K8s)
- **Event-Driven:** Services communicate via events/messages
- **Saga Pattern:** Manages distributed transactions

---

### 📝 Communication
- **Message Queue:** Async communication (e.g., Kafka, RabbitMQ)
- **Webhook:** Event-triggered callback
- **gRPC:** High-performance RPC protocol
- **Fan-Out / Fan-In:** Parallel tasking / aggregation

---

### 📚 Reliability
- **Circuit Breaker:** Stops calling a failing service
- **Retry Mechanism:** Retry failed ops with backoff
- **DLQ:** Stores unprocessable messages
- **Heartbeat:** Health check signal
- **Leader Election:** One node leads others (Raft, Paxos)
- **Idempotency:** Safe to retry an operation

---

### 📚 Deployment & Scaling
- **Blue-Green Deployment:** Two prod environments
- **Canary Deployment:** Gradual rollout to users
- **Immutable Infrastructure:** No post-deployment changes
- **Pre-Warming:** Avoiding cold starts
- **Horizontal Scaling:** Adding instances
- **Vertical Scaling:** Adding resources to same node
- **Autoscaler (HPA):** Dynamic pod scaling in Kubernetes

---

### ✨ Optimization & Routing
- **Sticky Sessions:** Route user to same instance
- **GSLB:** Distributes across global regions
- **CDN:** Edge caching static content
- **Service Discovery:** Services auto-discover peers
- **Consistent Hashing:** Load balancing with minimal reshuffle

---

### 📊 Observability
- **Logs:** Text-based tracking of events
- **Metrics:** Numeric representation of performance
- **Tracing:** Follows request path across services
- **SLI/SLO/SLA:** Indicators, objectives, agreements
- **Chaos Engineering:** Test system resilience via failures

---

### ✨ Advanced Topics
- **Bloom Filter:** Space-efficient set membership test
- **CRDTs:** Auto-mergeable data structures
- **Gossip Protocol:** Peer-to-peer update sharing
- **Shadow Traffic:** Silent traffic testing
- **Edge Computing:** Processing closer to end users
- **Cold/Warm/Hot Paths:** Batch vs real-time pipelines