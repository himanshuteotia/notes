### 📊 **MongoDB Aggregation Framework — Complete Guide to Operators & Keywords**

The **Aggregation Framework** in MongoDB is a powerful tool for **data analysis**, **transformation**, and **summarization**. It processes data through a **pipeline** of stages, where each stage performs specific operations on documents.

---

## ⚡ **1. What is the Aggregation Pipeline?**

- An **Aggregation Pipeline** processes documents in **stages**, transforming the data at each step.
- Think of it as a **conveyor belt** where data flows through a series of **filters**, **grouping**, and **modifications**.

### 💡 **Example Pipeline:**
```javascript
db.orders.aggregate([
  { $match: { status: "shipped" } },           // Filter
  { $group: { _id: "$customer_id", total: { $sum: "$amount" } } },  // Group
  { $sort: { total: -1 } }                     // Sort
])
```

---

## 📋 **2. Core Aggregation Pipeline Stages**

| **Operator**      | **Purpose**                                     | **Example Use Case**                |
|------------------|-------------------------------------------------|-------------------------------------|
| `$match`         | Filters documents (like `find()`)               | Select documents with specific criteria |
| `$group`         | Groups documents & performs aggregation (SUM, AVG) | Calculate total sales per user   |
| `$sort`          | Sorts documents                                 | Sort by total revenue              |
| `$project`       | Includes/excludes fields, creates computed fields | Select only specific fields       |
| `$limit`         | Limits the number of documents                  | Return top 5 results               |
| `$skip`          | Skips a number of documents                     | Pagination                         |
| `$count`         | Counts documents                                | Count total orders                 |
| `$unwind`        | Deconstructs arrays into multiple documents     | Flatten arrays                     |
| `$lookup`        | Joins collections (similar to SQL JOIN)         | Join orders with customer details  |
| `$facet`         | Runs multiple aggregations in parallel          | Combine multiple queries           |
| `$addFields`     | Adds new fields to documents                    | Add computed fields                |
| `$replaceRoot`   | Replaces the document root with a subdocument   | Restructure documents              |
| `$out`           | Writes results to a new collection              | Store aggregation results          |
| `$merge`         | Merges results into an existing collection      | Update or create data              |

---

## 🏆 **3. Detailed Breakdown of Aggregation Operators**

### ✅ **1. `$match` — Filter Documents**
- Works like `find()` but inside the aggregation pipeline.

```javascript
db.orders.aggregate([
  { $match: { status: "completed" } }
])
```

---

### ✅ **2. `$group` — Group & Aggregate Data**
- Similar to SQL’s `GROUP BY`.

```javascript
db.orders.aggregate([
  { $group: { _id: "$customer_id", total: { $sum: "$amount" } } }
])
```
- `_id`: The field to group by  
- `$sum`: Accumulator to sum `amount` fields

**Other Accumulators:**
| **Accumulator**  | **Purpose**                |
|------------------|----------------------------|
| `$sum`           | Total sum of values        |
| `$avg`           | Average value              |
| `$min`           | Minimum value              |
| `$max`           | Maximum value              |
| `$first`         | First document in group    |
| `$last`          | Last document in group     |
| `$push`          | Create array of values     |
| `$addToSet`      | Array of unique values     |

---

### ✅ **3. `$sort` — Sort Documents**
- Sorts the result set.

```javascript
db.orders.aggregate([
  { $sort: { amount: -1 } } // -1 for descending, 1 for ascending
])
```

---

### ✅ **4. `$project` — Reshape Documents**
- Select specific fields or create computed fields.

```javascript
db.orders.aggregate([
  { $project: { customer_id: 1, total_price: { $multiply: ["$quantity", "$price"] } } }
])
```

---

### ✅ **5. `$limit` & `$skip` — Pagination**
```javascript
db.orders.aggregate([
  { $sort: { order_date: -1 } },
  { $skip: 10 }, // Skip first 10 docs
  { $limit: 5 }  // Limit to next 5 docs
])
```

---

### ✅ **6. `$unwind` — Flatten Arrays**
- Breaks array fields into multiple documents.

**Example:**
```javascript
db.orders.aggregate([
  { $unwind: "$items" }
])
```

**Before:**
```json
{
  "order_id": 1,
  "items": ["pen", "notebook"]
}
```

**After Unwind:**
```json
{ "order_id": 1, "items": "pen" }
{ "order_id": 1, "items": "notebook" }
```

---

### ✅ **7. `$lookup` — Perform Joins**
- Joins documents from another collection.

```javascript
db.orders.aggregate([
  { $lookup: {
      from: "customers",
      localField: "customer_id",
      foreignField: "_id",
      as: "customer_details"
  }}
])
```

---

### ✅ **8. `$facet` — Multi-Faceted Aggregation**
- Run multiple pipelines in parallel.

```javascript
db.orders.aggregate([
  { $facet: {
      "total_sales": [{ $group: { _id: null, total: { $sum: "$amount" } } }],
      "top_customers": [{ $sort: { amount: -1 } }, { $limit: 5 }]
  }}
])
```

---

### ✅ **9. `$addFields` — Add New Fields**
```javascript
db.orders.aggregate([
  { $addFields: { total_price: { $multiply: ["$quantity", "$price"] } } }
])
```

---

### ✅ **10. `$replaceRoot` — Restructure Documents**
- Replace the current document with a subdocument.

```javascript
db.users.aggregate([
  { $replaceRoot: { newRoot: "$profile" } }
])
```

---

### ✅ **11. `$out` & `$merge` — Store Aggregation Results**

- **`$out`** writes results to a **new collection**:
```javascript
db.orders.aggregate([
  { $group: { _id: "$customer_id", total: { $sum: "$amount" } } },
  { $out: "customer_totals" }
])
```

- **`$merge`** merges results into an **existing collection**:
```javascript
db.orders.aggregate([
  { $group: { _id: "$customer_id", total: { $sum: "$amount" } } },
  { $merge: { into: "customer_totals", whenMatched: "merge" } }
])
```

---

## 📡 **4. Expression Operators (For Computations)**

| **Operator**     | **Purpose**                      |
|------------------|----------------------------------|
| `$sum`           | Sums values                      |
| `$avg`           | Averages values                  |
| `$min`           | Minimum value                    |
| `$max`           | Maximum value                    |
| `$concat`        | Concatenates strings             |
| `$substr`        | Extracts substring               |
| `$toUpper`       | Converts to uppercase            |
| `$toLower`       | Converts to lowercase            |
| `$dateToString`  | Converts date to string format   |
| `$cond`          | If-Else conditional logic        |
| `$ifNull`        | Returns value if not null        |
| `$regexMatch`    | Regular expression matching      |

---

## 🏆 **5. Common Aggregation Use Cases**

| **Use Case**                      | **Aggregation Operators Used**           |
|-----------------------------------|-----------------------------------------|
| Sales report per region           | `$match`, `$group`, `$sort`             |
| Flatten nested arrays             | `$unwind`                               |
| Join users with their orders      | `$lookup`                               |
| Calculate average ratings         | `$group`, `$avg`                        |
| Paginate sorted results           | `$sort`, `$skip`, `$limit`              |
| Count documents per category      | `$group`, `$count`                      |
| Full-text search                  | `$match`, `$text`                       |

---

💥 **Pro Tip:**  
- Use `.explain("executionStats")` to analyze aggregation performance.
- Combine `$match` and `$project` early in the pipeline to reduce data size for later stages.