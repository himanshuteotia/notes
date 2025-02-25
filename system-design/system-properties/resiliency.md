### ✅ **Resiliency** — *Building Systems That Recover from Failures*

**Resiliency** in system design refers to the **ability of a system to absorb, adapt to, and recover from failures or unexpected disruptions** while maintaining core functionalities. A resilient system doesn't just survive failures — it **recovers gracefully** and **continues to operate**.

Think of resiliency as the **"bounce-back"** ability of your system. Even when parts of it fail, it should **degrade gracefully** and recover without significant impact on users.

---

## 📊 **1. Key Characteristics of a Resilient System**

| **Aspect**              | **Description**                                             |
|-------------------------|-------------------------------------------------------------|
| **Fault Tolerance**      | Continues functioning despite component failures.          |
| **Graceful Degradation** | Partial functionality remains available during failures.   |
| **Self-Healing**         | Detects and auto-recovers from failures.                   |
| **Redundancy**           | Uses backup components to avoid single points of failure.   |
| **Load Shedding**        | Drops non-critical requests to maintain stability.         |
| **Disaster Recovery**    | Ability to recover from large-scale outages or disasters.  |

---

## 🏆 **2. Resiliency vs. Availability vs. Reliability**

| **Concept**     | **Definition**                                      | **Example**                                     |
|-----------------|----------------------------------------------------|-------------------------------------------------|
| **Resiliency**   | System recovers quickly from failures.             | Netflix uses Chaos Engineering to test failures. |
| **Availability** | System is accessible when needed.                  | A website is online 24/7.                        |
| **Reliability**  | System consistently performs as expected over time.| Bank transactions process without errors.        |

💡 **Quick Insight:**  
- **Availability** is about minimizing downtime.  
- **Reliability** is about consistent and correct operation.  
- **Resiliency** is about **recovering from disruptions**.

---

## 🏗️ **3. How Does Resiliency Work in Systems?**

### **Example Resilient System Architecture:**
```
           +------------------+
Client --> |  Load Balancer   | --> Healthy Service A
           +------------------+     |
                                    |--> Fails --> Rerouted to Backup
                                    |
                             Healthy Service B
```

- **Load Balancer** redirects traffic if one service fails.
- **Health Checks** detect failures in real time.
- **Auto-scaling** and **failover strategies** kick in to maintain operations.

---

## 💡 **4. Resiliency Patterns & Strategies**

### ⚙️ **1. Circuit Breaker Pattern**
- Prevents repeated calls to a failing service.
- Opens the circuit when failures cross a threshold, allowing time to recover.

```javascript
if (failureCount > threshold) {
  openCircuit();
} else {
  callService();
}
```

### 💾 **2. Bulkhead Pattern**
- Isolates critical components so that a failure in one part doesn’t cascade to others.
- **Example:** Separate service pools for different types of requests.

### 🔁 **3. Retry & Exponential Backoff**
- Automatically retries failed requests after increasing time intervals.
- Helps with transient errors (e.g., temporary network issues).

### 🚦 **4. Rate Limiting & Load Shedding**
- Protects the system during heavy load by rejecting low-priority requests.
- Ensures critical services remain available.

### 📊 **5. Chaos Engineering**
- Intentionally introduces failures to test the system's resiliency.
- **Netflix's Chaos Monkey** is a well-known example.

---

## 📡 **5. Real-World Examples of Resiliency**

| **Company**      | **Resiliency Strategy Used**                            |
|------------------|--------------------------------------------------------|
| **Netflix**      | Chaos Engineering, auto-scaling, global load balancing |
| **Amazon AWS**   | Multi-region failover, distributed databases           |
| **Google Cloud** | Redundant networking and global replication            |
| **Slack**        | Circuit breakers, retry strategies, and fallbacks      |

---

## ⚖️ **6. Trade-Offs in Resiliency**

| **Trade-Off**                  | **Explanation**                                 |
|--------------------------------|-------------------------------------------------|
| **Cost vs. Resiliency**        | More redundancy = higher costs.                 |
| **Latency vs. Resiliency**     | Fallback strategies can introduce extra delays. |
| **Consistency vs. Resiliency** | In distributed systems, achieving both is hard. |

---

## 🧮 **7. Building a Resilient System: Best Practices**

1. **Implement Health Checks** for all critical services.
2. **Use Load Balancers** for traffic rerouting during failures.
3. **Add Redundancy** at every layer (databases, servers, regions).
4. **Design for Graceful Degradation** — ensure partial functionality.
5. **Use Circuit Breakers** and **Retry Policies** in microservices.
6. **Set Up Monitoring & Alerting** using tools like **Prometheus** and **Grafana**.
7. **Test with Chaos Engineering** to validate your system’s resiliency.

---

## 🎯 **8. Resiliency in Distributed Systems (CAP Theorem Impact)**

In distributed systems, you can optimize for only two out of the following three:

| **Property**         | **Description**                                           |
|---------------------|-----------------------------------------------------------|
| **Consistency**      | Every read returns the most recent write.                 |
| **Availability**     | Every request receives a response (even if outdated).    |
| **Partition Tolerance** | System remains operational during network failures.    |

**Resilient systems** often favor **Availability** and **Partition Tolerance** while sometimes compromising on immediate consistency (eventual consistency).

---

### 💥 **Key Takeaway:**  
**Resiliency is about how well your system can withstand and recover from failures.**  
It’s not about avoiding failures — it’s about **surviving them** and **recovering quickly**.