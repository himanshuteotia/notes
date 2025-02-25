### 💼 **Common MongoDB Interview Questions & Answers**

Whether you're preparing for a **MongoDB developer** role or a **full-stack developer** position, these **MongoDB interview questions** cover a wide range of topics, from basic concepts to advanced features like sharding and aggregation.

---

## 📊 **1. MongoDB Basics**

### ❓ **Q1. What is MongoDB? Why use it over SQL databases?**  
**💡 Answer:**  
- MongoDB is a **NoSQL**, **document-oriented** database that stores data in **JSON-like** documents (`BSON`).  
- **Advantages over SQL databases:**
  - **Schema-less** (flexible data models)
  - **Horizontal scalability** (sharding)
  - **High performance** for read/write operations
  - **Document-based storage** (natural mapping to objects)

---

### ❓ **Q2. What is BSON in MongoDB?**  
**💡 Answer:**  
- BSON (**Binary JSON**) is the format MongoDB uses to store documents.
- It extends JSON with additional data types like **Date**, **Binary**, and **ObjectId** for optimized storage and querying.

---

### ❓ **Q3. What is a Collection and Document in MongoDB?**  
**💡 Answer:**  
- **Document:** A single record in MongoDB (stored in BSON format).
- **Collection:** A group of related documents (equivalent to a table in SQL).

**Example:**
```json
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "name": "Alice",
  "age": 28,
  "interests": ["music", "sports"]
}
```

---

### ❓ **Q4. What are the advantages of MongoDB?**  
**💡 Answer:**
- Schema flexibility
- High availability with replica sets
- Horizontal scalability using sharding
- Easy integration with Node.js, Python, and other modern stacks
- Rich querying and aggregation capabilities

---

## 📋 **2. CRUD Operations**

### ❓ **Q5. How do you perform CRUD operations in MongoDB?**  

**💡 Create (Insert):**
```javascript
db.users.insertOne({ name: "John", age: 30 })
```

**💡 Read (Find):**
```javascript
db.users.find({ age: { $gt: 25 } })
```

**💡 Update:**
```javascript
db.users.updateOne({ name: "John" }, { $set: { age: 31 } })
```

**💡 Delete:**
```javascript
db.users.deleteOne({ name: "John" })
```

---

### ❓ **Q6. How is `find()` different from `findOne()`?**  
**💡 Answer:**  
- `find()` returns a **cursor** to all matching documents.  
- `findOne()` returns the **first matching document**.

---

## ⚡ **3. Indexing & Performance**

### ❓ **Q7. What are indexes in MongoDB? Why are they important?**  
**💡 Answer:**  
- Indexes improve query performance by allowing the database to **quickly locate documents** without scanning the entire collection.
- Without indexes, MongoDB performs a **collection scan**.

**Example:**
```javascript
db.users.createIndex({ age: 1 }) // Ascending index on age
```

---

### ❓ **Q8. What types of indexes are available in MongoDB?**  
**💡 Answer:**
1. **Single Field Index**
2. **Compound Index** (multi-field)
3. **Multikey Index** (for arrays)
4. **Text Index** (full-text search)
5. **Geospatial Index** (`2dsphere`)
6. **Hashed Index** (for sharding)
7. **TTL Index** (for expiring documents)

---

### ❓ **Q9. How do you check if a query uses an index?**  
**💡 Answer:**
```javascript
db.users.find({ age: { $gt: 30 } }).explain("executionStats")
```
- Look for **`IXSCAN`** (index used) or **`COLLSCAN`** (full scan).

---

## 📡 **4. Aggregation Framework**

### ❓ **Q10. What is the Aggregation Framework in MongoDB?**  
**💡 Answer:**  
- A **data processing pipeline** that transforms documents using stages like `$match`, `$group`, `$sort`, etc.

**Example:**
```javascript
db.orders.aggregate([
  { $match: { status: "shipped" } },
  { $group: { _id: "$customer_id", total: { $sum: "$amount" } } },
  { $sort: { total: -1 } }
])
```

---

### ❓ **Q11. What are `$match`, `$group`, and `$project` in aggregation?**  
**💡 Answer:**
- **`$match`** → Filters documents (similar to `find`)  
- **`$group`** → Groups documents and applies aggregation (e.g., `$sum`, `$avg`)  
- **`$project`** → Selects fields and creates computed fields  

---

## 🏗️ **5. Data Modeling & Schema Design**

### ❓ **Q12. How do you model relationships in MongoDB?**  
**💡 Answer:**  
1. **Embedding:** Include related data in the same document (for one-to-few relationships).  
2. **Referencing:** Use ObjectIds to link documents (for one-to-many or many-to-many).

**Example (Referencing):**
```json
{
  "_id": 1,
  "user_id": ObjectId("507f1f77bcf86cd799439011"),
  "order_total": 150
}
```

---

### ❓ **Q13. When should you embed vs reference documents?**  
**💡 Embed if:**
- Data is frequently read together.
- Document size stays within BSON limits (16 MB).

**💡 Reference if:**
- There are many related documents.
- Relationships change frequently.

---

## 🏆 **6. Advanced Concepts**

### ❓ **Q14. What is Sharding in MongoDB?**  
**💡 Answer:**  
- **Sharding** is MongoDB’s method for **horizontal scaling** by partitioning data across multiple servers (**shards**).
- Uses a **shard key** to determine how data is split.

---

### ❓ **Q15. What is a Replica Set?**  
**💡 Answer:**  
- A **Replica Set** is a group of MongoDB servers that maintain the same data for **high availability**.
- Consists of:
  - **Primary:** Handles reads/writes.
  - **Secondary:** Syncs from the primary (can be used for read scaling).
  - **Arbiter (optional):** Helps in election but doesn’t store data.

---

### ❓ **Q16. What is CAP Theorem? How does MongoDB fit into it?**  
**💡 Answer:**  
- **CAP Theorem:** In distributed databases, you can only guarantee **2 of 3**:
  1. **Consistency**
  2. **Availability**
  3. **Partition Tolerance**

- **MongoDB is CP** by default (Consistency + Partition Tolerance)  
- Can be configured for **AP** using **read preferences**.

---

## ⚖️ **7. Data Integrity & Security**

### ❓ **Q17. How does MongoDB ensure data consistency?**  
**💡 Answer:**  
- **Write Concerns:** Control the level of acknowledgment requested from MongoDB for write operations.  
- **Read Concerns:** Define the level of isolation for read operations.

---

### ❓ **Q18. How do you secure MongoDB?**  
**💡 Answer:**
1. Enable **authentication** and use **strong user roles**.
2. Use **TLS/SSL** for encrypted communication.
3. Apply **firewall rules** to limit access.
4. Use **role-based access control (RBAC)**.

---

## 📊 **8. Performance Tuning**

### ❓ **Q19. How do you improve MongoDB query performance?**  
**💡 Answer:**
- Use **indexes** for frequently queried fields.
- Avoid **large `$in` or `$or` queries**.
- Use **projection** to return only necessary fields.
- Apply **aggregation pipeline optimizations**.
- Monitor with **`db.currentOp()`** and **`explain()`**.

---

### ❓ **Q20. What are some common anti-patterns in MongoDB?**  
**💡 Answer:**
- Overusing **embedding** in large documents (hits BSON limit).
- Using **low-cardinality fields** as shard keys.
- Storing large blobs (use GridFS instead).
- Creating too many indexes (affects write performance).

---

## 💡 **Bonus: Scenario-Based Questions**

1. **How would you design a real-time chat application in MongoDB?**
2. **What would you do if a shard becomes unbalanced?**
3. **How would you handle high write loads in a distributed system?**
4. **What is the best way to migrate data between collections?**

---

💥 **Pro Tip:**  
- In interviews, focus on **real-world use cases** (e.g., how you’ve optimized queries, scaled systems, or solved performance bottlenecks).