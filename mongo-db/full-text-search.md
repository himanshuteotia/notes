**Full-text search** is a technique used in databases and search engines to search for and retrieve documents or records that contain specific words or phrases. Unlike basic keyword matching, full-text search understands the structure of language and indexes the content in a way that allows for more advanced searching.

### 🔍 Key Features of Full-Text Search:

* **Word-based matching:** Finds documents containing one or more words, regardless of their position.
* **Relevance ranking:** Returns results sorted by how closely they match the search query.
* **Stemming and lemmatization (optional):** Matches different forms of the same word (e.g., "run", "running").
* **Stop words exclusion:** Common words like "the", "is", etc., can be ignored during indexing.
* **Boolean operators:** Supports `AND`, `OR`, `NOT` in queries.
* **Phrase search:** You can search for exact phrases using quotes, e.g., `"open source"`.

### 🛠 Example Use Cases:

* Searching blog posts or articles on a website
* Product search in e-commerce
* Message search in a chat application

### 💡 Example:

Imagine you have this text in your database:

> "Node.js is a JavaScript runtime built on Chrome's V8 engine."

If someone searches for "JavaScript engine", full-text search can identify and return this record even if the exact phrase doesn’t appear together.

### 🔧 In Tools:

* **MongoDB**: Uses `$text` operator with text indexes.
* **MySQL**: Has `FULLTEXT` index for full-text search in `MyISAM` and `InnoDB`.
* **Elasticsearch / Typesense**: Designed specifically for full-text search, offering advanced features and high performance.

Here’s a simple **JavaScript + MongoDB full-text search example** using the MongoDB Node.js driver.

---

### 📦 1. Sample Data Insertion

```js
const { MongoClient } = require("mongodb");

async function insertSampleData() {
  const client = new MongoClient("mongodb://localhost:27017");
  await client.connect();
  const db = client.db("searchDemo");
  const collection = db.collection("articles");

  // Create a text index on the 'title' and 'content' fields
  await collection.createIndex({ title: "text", content: "text" });

  // Insert sample documents
  await collection.insertMany([
    { title: "Node.js Guide", content: "Learn how Node.js works with the V8 engine." },
    { title: "React Tutorial", content: "A beginner's guide to building apps with React." },
    { title: "JavaScript Tips", content: "Improve your JS skills with these tips and tricks." }
  ]);

  console.log("Sample data inserted.");
  await client.close();
}
insertSampleData();
```

---

### 🔍 2. Full-Text Search Query

```js
async function searchArticles(query) {
  const client = new MongoClient("mongodb://localhost:27017");
  await client.connect();
  const db = client.db("searchDemo");
  const collection = db.collection("articles");

  const results = await collection.find({ $text: { $search: query } }).toArray();

  console.log("Search results:");
  results.forEach((doc) => console.log(doc.title));
  await client.close();
}

searchArticles("Node.js engine");
```

---

### 🧠 How it works:

* **Text index** is created on `title` and `content`.
* `$text: { $search: "Node.js engine" }` looks for documents containing both words.
* MongoDB internally ranks results by relevance.

Great! To **sort by relevance score** in MongoDB full-text search, you can use the special `$meta: "textScore"` projection and sorting.

---

### ✅ Updated Search with Relevance Score

```js
async function searchArticlesWithScore(query) {
  const { MongoClient } = require("mongodb");
  const client = new MongoClient("mongodb://localhost:27017");

  await client.connect();
  const db = client.db("searchDemo");
  const collection = db.collection("articles");

  const results = await collection
    .find(
      { $text: { $search: query } },
      { projection: { score: { $meta: "textScore" }, title: 1, content: 1 } }
    )
    .sort({ score: { $meta: "textScore" } })
    .toArray();

  console.log("Search results (sorted by relevance):");
  results.forEach((doc) => {
    console.log(`📄 ${doc.title} (score: ${doc.score.toFixed(2)})`);
  });

  await client.close();
}

searchArticlesWithScore("JavaScript engine");
```

---

### 🧾 Output Example:

```
📄 Node.js Guide (score: 1.50)
📄 JavaScript Tips (score: 1.00)
```

---

### 🔍 Notes:

* `projection: { score: { $meta: "textScore" } }`: Adds the relevance score in results.
* `.sort({ score: { $meta: "textScore" } })`: Sorts by the score (higher = more relevant).
