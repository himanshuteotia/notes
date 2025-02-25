# 🧠 **Caching Patterns in Distributed Systems**

**Caching** is a technique used to **store data temporarily** to reduce the **load on databases**, improve **performance**, and **decrease latency**. In distributed systems, caching plays a critical role in scaling applications and ensuring high availability.

---

## ⚡ **Why Use Caching?**
- 🚀 **Improves Performance:** Faster data retrieval.
- 💰 **Reduces Costs:** Minimizes database calls.
- 🧮 **Increases Scalability:** Handles more user requests.
- ⚡ **Reduces Latency:** Especially for read-heavy applications.

---

## 📊 **Types of Caches:**
1. **In-Memory Cache** (e.g., Redis, Memcached) → Fastest but volatile.
2. **Distributed Cache** (e.g., Redis Cluster, Hazelcast) → Shared across multiple instances.
3. **Local Cache** (e.g., LRU Cache in Node.js) → Resides within the application.

---

## 🎯 **Common Caching Patterns:**

### ✅ **1. Cache-Aside (Lazy Loading)**
**Most common caching strategy.**

- The application checks the cache before hitting the database.
- If the data is not in the cache (**cache miss**), fetch from DB and update the cache.

**📊 Workflow:**
```
Client → Cache (miss) → Database → Cache (store) → Client
```

**💡 Use Case:** Ideal for read-heavy applications with occasional writes.

**Example (Node.js + Redis):**
```javascript
const redis = require('redis');
const client = redis.createClient();
const { promisify } = require('util');
const getAsync = promisify(client.get).bind(client);
const setAsync = promisify(client.set).bind(client);

async function getUser(userId) {
  const cachedData = await getAsync(userId);

  if (cachedData) {
    return JSON.parse(cachedData); // Cache hit
  }

  // Cache miss, fetch from DB
  const userData = await db.query(`SELECT * FROM users WHERE id = ${userId}`);
  await setAsync(userId, JSON.stringify(userData), 'EX', 60); // Expires in 60s
  return userData;
}
```

---

### ✅ **2. Read-Through Cache**
The application interacts directly with the cache. If the cache misses, it loads data from the database **and** updates the cache automatically.

**📊 Workflow:**
```
Client → Cache → (miss) → Database → Cache → Client
```

**💡 Use Case:** Simplifies application code but can introduce latency during cache misses.

---

### ✅ **3. Write-Through Cache**
Every write operation goes through the cache **first**. The cache writes the data to the database **immediately**.

**📊 Workflow:**
```
Client → Cache → Database
```

**💡 Use Case:** Ensures cache consistency with DB but increases write latency.

---

### ✅ **4. Write-Behind (Write-Back) Cache**
Similar to **Write-Through**, but the cache writes data to the database **asynchronously** after a short delay.

**📊 Workflow:**
```
Client → Cache (ack) → Database (delayed write)
```

**💡 Use Case:** Optimizes write performance but risks data loss if cache fails before writing to the DB.

---

### ✅ **5. Cache Invalidation Patterns**
Managing data freshness is critical in caching. These patterns ensure the cache doesn’t serve stale data.

#### 🔸 **Time-To-Live (TTL):**
- Set an expiry time for cached data.

```javascript
await setAsync('key', JSON.stringify(data), 'EX', 60); // 60 sec TTL
```

#### 🔸 **Explicit Invalidation:**
- Invalidate cache entries when data is updated.

```javascript
await client.del('key'); // Remove from cache after DB update
```

#### 🔸 **Write-Through with Invalidation:**
- Invalidate cache on **writes** and **re-populate** on next read.

---

### ✅ **6. Cache Busting (Versioning)**
When a large set of cached data needs to be invalidated, **cache versioning** is used.

**Example:**
```javascript
const version = 'v2';
const cacheKey = `${version}:user:${userId}`;
```
- Changing `v2` to `v3` invalidates all old cache entries.

---

### ✅ **7. Content Delivery Network (CDN) Caching**
For static assets (images, CSS, JS), CDNs cache data at edge servers close to users.

**💡 Use Case:** Optimizes performance for static content delivery.

---

### ✅ **8. Two-Tier Caching**
Combines **local cache** and **distributed cache**.

1. App checks local cache (fastest).
2. If a **miss**, it checks the distributed cache.
3. If another **miss**, it fetches from the DB and updates both caches.

**💡 Use Case:** Reduces network calls and improves response time.

---

### ✅ **9. Cache Stampede Prevention**
Occurs when many clients request the same data at once, causing a surge of DB hits on cache expiration.

**Prevention Techniques:**
- **Locking:** Only one process can fetch and update the cache.
- **Stale-While-Revalidate:** Serve stale data while fetching fresh data in the background.
- **Randomized TTL:** Add randomness to expiry times to avoid simultaneous cache misses.

---

### ✅ **10. Sharded Caching**
Divide the cache across multiple nodes to scale horizontally.

**💡 Use Case:** Used in large-scale systems like **Twitter** or **Facebook** to handle massive traffic.

---

## ⚖️ **Comparison of Caching Patterns:**

| **Pattern**          | **Read Performance** | **Write Performance** | **Consistency** | **Use Case**              |
|----------------------|----------------------|-----------------------|-----------------|---------------------------|
| **Cache-Aside**       | ⭐⭐⭐⭐                 | ⭐⭐                    | Weak            | Read-heavy systems        |
| **Read-Through**      | ⭐⭐⭐⭐                 | ⭐⭐                    | Strong          | Simple caching logic      |
| **Write-Through**     | ⭐⭐⭐                  | ⭐⭐                    | Strong          | Strong consistency needs  |
| **Write-Behind**      | ⭐⭐⭐⭐                 | ⭐⭐⭐⭐                  | Eventual        | High write performance    |
| **Two-Tier Caching**   | ⭐⭐⭐⭐⭐                | ⭐⭐                    | Strong/Weak     | Low-latency requirements  |
| **Sharded Caching**    | ⭐⭐⭐⭐⭐                | ⭐⭐⭐⭐                  | Strong/Weak     | High scalability systems  |

---

## 💥 **Best Practices:**

1. ✅ **Use TTLs** to avoid stale data.
2. ✅ **Monitor cache hit/miss ratios**.
3. ✅ **Design for cache failures** — always have a fallback to the database.
4. ✅ **Evict rarely-used data** using strategies like **LRU (Least Recently Used)** or **LFU (Least Frequently Used)**.
5. ✅ **Ensure idempotency** in write-behind strategies.

---

## 💡 **When to Avoid Caching:**
- 🔥 In real-time systems that require strong consistency.
- 🔥 If the data is highly dynamic and changes frequently.
- 🔥 For security-sensitive data (unless encrypted).

---

## 🏆 **Final Thoughts:**
- Choose the right caching pattern based on **read/write ratios**, **consistency needs**, and **failure tolerance**.
- A well-designed caching strategy can **improve performance by 10x** while reducing infrastructure costs.