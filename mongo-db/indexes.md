### 🧩 **Indexes in MongoDB — Complete Guide**

**Indexes** in **MongoDB** are special data structures that **improve the speed and efficiency** of query operations. Without indexes, MongoDB must perform a **collection scan** — examining every document to select those that match the query — which is inefficient for large datasets.

Think of indexes as the **"table of contents"** in a book. Instead of flipping through every page to find a topic, you can go straight to the correct page using the table of contents.

---

## 📊 **1. What is an Index in MongoDB?**

- An **index** stores a small portion of the collection’s data in an **easily searchable form**.
- It allows MongoDB to quickly locate and retrieve data **without scanning the entire collection**.

**💡 Example:**
If you frequently query by `user_id`, creating an index on `user_id` will make those queries faster.

```javascript
db.users.createIndex({ user_id: 1 })
```

- `1` → **Ascending** order  
- `-1` → **Descending** order

---

## ⚡ **2. Why Use Indexes?**

| **Benefit**                  | **Impact**                             |
|------------------------------|----------------------------------------|
| ✅ **Faster Queries**        | Reduces query response time           |
| ✅ **Efficient Sorting**     | Sort operations utilize indexes       |
| ✅ **Optimized Aggregations** | Stages like `$match` and `$sort` benefit |
| ✅ **Improved Range Queries** | Efficient retrieval of data ranges    |
| ✅ **Reduces I/O Load**       | Less data read from disk              |

---

## 🏆 **3. Types of Indexes in MongoDB**

### ✅ **1. Single Field Index**
- Index on a single field in a document.

**Example:**
```javascript
db.users.createIndex({ user_id: 1 })
```
**Query:**
```javascript
db.users.find({ user_id: 12345 })
```

---

### ✅ **2. Compound Index**
- Index on **multiple fields** in a single index.

**Example:**
```javascript
db.orders.createIndex({ user_id: 1, order_date: -1 })
```
- Supports queries on:
  - `user_id`
  - `user_id` + `order_date`
- **⚠️ Order matters** in compound indexes.

---

### ✅ **3. Multikey Index**
- Indexes fields that store arrays.

**Example:**
```javascript
db.products.createIndex({ tags: 1 })
```
```javascript
db.products.insertOne({ name: "Laptop", tags: ["electronics", "computers"] })
```
- MongoDB creates an index entry for **each element** in the array.

---

### ✅ **4. Text Index**
- Supports **full-text search** on string fields.

**Example:**
```javascript
db.articles.createIndex({ content: "text" })
```
**Query:**
```javascript
db.articles.find({ $text: { $search: "MongoDB indexing" } })
```

**💡 Pro Tip:**  
- Can only create **one text index** per collection.
- Supports **stemming** and **stop words**.

---

### ✅ **5. Geospatial Index**
- Supports queries based on **geolocation data**.

**Example:**
```javascript
db.locations.createIndex({ coordinates: "2dsphere" })
```
**Query:**
```javascript
db.locations.find({
  coordinates: {
    $near: {
      $geometry: { type: "Point", coordinates: [40.730610, -73.935242] },
      $maxDistance: 5000
    }
  }
})
```

---

### ✅ **6. Hashed Index**
- Supports **shard keys** in **Hash-Based Sharding**.

**Example:**
```javascript
db.users.createIndex({ user_id: "hashed" })
```
- Provides **uniform distribution** across shards.

---

### ✅ **7. TTL (Time-To-Live) Index**
- Automatically deletes documents after a specified period.

**Example:**
```javascript
db.sessions.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 })
```
- Documents older than **1 hour** will be deleted.

---

## 📋 **4. How Indexes Work in Queries**

| **Query Type**                      | **Uses Index?** |
|-------------------------------------|-----------------|
| Exact Match (`{ user_id: 123 }`)    | ✅ Yes          |
| Range Query (`{ age: { $gt: 30 } }`)| ✅ Yes          |
| Sorting (`.sort({ age: 1 })`)       | ✅ Yes (if index exists) |
| Aggregations with `$match`          | ✅ Yes          |
| Full Collection Scan                | ❌ No           |

**💡 Check Index Usage:**
```javascript
db.users.find({ user_id: 123 }).explain("executionStats")
```

- If `IXSCAN` is used → ✅ Index was applied  
- If `COLLSCAN` is used → ❌ No index applied

---

## ⚖️ **5. Index Performance Trade-offs**

| **Advantage**                       | **Disadvantage**                         |
|-------------------------------------|------------------------------------------|
| ⚡ Faster Read Operations            | 🐢 Slower Write Operations (due to index updates) |
| 🔍 Optimized Search & Sorting       | 📁 Increased Storage Usage               |
| 🏆 Improved Query Performance       | ⚠️ Complex Maintenance with Many Indexes |

---

## 💥 **6. Indexing Best Practices**

1. **Use Indexes on Frequently Queried Fields:**  
   - Fields used in **`find()`**, **`sort()`**, or **`aggregate()`**.

2. **Limit the Number of Indexes:**  
   - Each index consumes disk space and slows down write operations.

3. **Use Compound Indexes for Multi-Field Queries:**  
   - But ensure the order aligns with query patterns.

4. **Avoid Indexing Fields with Low Cardinality:**  
   - Fields like `gender` or `boolean` are poor index choices.

5. **Monitor with `explain()`:**  
   - Regularly check query plans and index utilization.

6. **Use TTL Indexes for Expiring Data:**  
   - Session tokens, logs, and temporary data.

---

## 📊 **7. Analyzing and Optimizing Indexes**

### ✅ **List All Indexes in a Collection:**
```javascript
db.users.getIndexes()
```

---

### ✅ **Remove an Unused Index:**
```javascript
db.users.dropIndex("user_id_1")
```

---

### ✅ **Analyze Index Efficiency:**
```javascript
db.orders.find({ user_id: 123 }).explain("executionStats")
```

- **Metrics to Watch:**
  - **`nReturned`** → Number of documents returned.
  - **`totalDocsExamined`** → Should be minimized.
  - **`executionTimeMillis`** → Lower is better.

---

## 💡 **8. Quick Recap of Index Types**

| **Index Type**           | **Use Case**                               |
|--------------------------|--------------------------------------------|
| **Single Field Index**    | Simple exact-match queries                |
| **Compound Index**        | Queries involving multiple fields         |
| **Multikey Index**        | Fields storing arrays                     |
| **Text Index**            | Full-text search                          |
| **Geospatial Index**      | Geo-based queries (`$near`, `$geoWithin`)  |
| **Hashed Index**          | Shard keys in hashed sharding             |
| **TTL Index**             | Auto-delete expiring data                 |

---

💥 **Pro Tip:**  
- Over-indexing can slow down **write operations**.  
- Regularly monitor **read vs. write workloads** to maintain balance.
