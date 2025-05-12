# Elasticsearch Interview Questions and Answers

## 📘 Basics

**1. What is Elasticsearch and what are its key features?**
Elasticsearch is a distributed, RESTful search and analytics engine capable of storing, searching, and analyzing large volumes of data in near real time. It is built on top of Apache Lucene.

**Key Features:**

* Full-text search
* Real-time data retrieval
* Scalability and high availability
* Document-oriented (stores JSON)
* Built-in distributed system

**2. What is an index in Elasticsearch?**
An index is like a database in a relational DB. It stores related documents with similar characteristics.

**3. How is data stored in Elasticsearch?**
Data is stored as JSON documents in indices. Each document is a unit of searchable data.

**4. What is an inverted index and how does it help in searching?**
An inverted index maps terms to the documents that contain them, enabling fast full-text searches.

**5. What are nodes and clusters in Elasticsearch?**

* **Node**: A single Elasticsearch instance.
* **Cluster**: A collection of nodes working together, identified by a shared cluster name.

## 🔎 Search & Querying

**6. What is the difference between `match` and `term` queries?**

* `match`: Performs full-text search, analyzes the input.
* `term`: Exact match, no analysis (used for keyword fields).

**7. How does full-text search work in Elasticsearch?**
It uses analyzers to tokenize and normalize text into searchable terms using an inverted index.

**8. How does Elasticsearch calculate the relevance score of a document?**
It uses the TF-IDF (term frequency-inverse document frequency) and BM25 algorithm to rank documents.

**9. What is a bool query?**
A compound query that combines multiple queries using `must`, `should`, `must_not`, and `filter` clauses.

**10. What is fuzziness and how does it work?**
Fuzziness enables typo-tolerant search by allowing character edits (Levenshtein distance).

## 🧰 Indexing & Analysis

**11. What is an analyzer in Elasticsearch?**
An analyzer processes text during indexing and querying using a tokenizer and filters.

**12. What are tokenizer and filters in analyzers?**

* **Tokenizer**: Breaks text into tokens (words).
* **Filters**: Modify tokens (lowercasing, stemming, etc.).

**13. How would you define a custom analyzer for product search?**
Define it in index settings with a custom tokenizer and filters to handle specific text formats (e.g., `t-shirt` to `tshirt`).

**14. What is a mapping?**
Defines how fields in a document are indexed and stored (data types, analyzers, etc.).

**15. How do you handle dynamic vs strict mappings?**

* **Dynamic**: Fields added automatically.
* **Strict**: Only predefined fields are allowed.

## 🏃‍♂️ Performance & Optimization

**16. How do you scale Elasticsearch to handle millions of products?**
Use multiple shards, replicas, and horizontal scaling with multiple nodes.

**17. What are shards and replicas?**

* **Shard**: A subset of the index.
* **Replica**: A copy of a shard for fault tolerance.

**18. What is the impact of number of shards on performance?**
Too many shards can increase overhead. Balanced sharding improves indexing/search speed.

**19. How do you optimize a slow query?**

* Use filters (cached)
* Avoid wildcard queries
* Reduce index size
* Use pagination

**20. What is the difference between `doc_values`, `stored fields`, and `_source`?**

* `doc_values`: Optimized for aggregations/sorting.
* `stored`: Separate copy for retrieval.
* `_source`: Original JSON stored.

## 🏢 E-commerce Use Case

**21. How would you implement autocomplete or "search-as-you-type"?**
Use edge n-gram tokenizer or `search_as_you_type` field type.

**22. How would you implement synonym support?**
Define a custom analyzer with a synonym filter in the index settings.

**23. How can you boost results based on fields?**
Use `boost` parameter in queries or function\_score queries to prioritize certain fields (e.g., `name` > `description`).

**24. How would you implement faceted search (filters)?**
Use aggregations (terms, range, etc.) on fields like category or price.

**25. How do you handle multilingual search?**
Use language-specific analyzers or create separate fields per language with respective analyzers.

## 🪖 Tools & Ecosystem

**26. What is Kibana and how does it relate to Elasticsearch?**
Kibana is a visualization tool for Elasticsearch data—used for dashboards, logs, and query testing.

**27. What is the ELK stack?**
ELK = Elasticsearch + Logstash + Kibana (now often referred to as Elastic Stack).

**28. How do you monitor the health of an Elasticsearch cluster?**
Use `_cluster/health`, Kibana, or tools like Elastic Monitoring.

**29. How would you back up Elasticsearch data?**
Use the snapshot and restore API with a shared filesystem or cloud storage.

**30. How do you secure Elasticsearch?**
Use API keys, TLS encryption, user authentication, and IP filtering.

## 🧱 Advanced

**31. What is the difference between `keyword` and `text` fields?**

* `keyword`: Exact match, not analyzed.
* `text`: Analyzed, used for full-text search.

**32. How would you handle nested documents and queries?**
Use `nested` field type and `nested` query to properly query objects with inner structure.

**33. What is the difference between `nested` and `object` field types?**

* `object`: Flat structure (not suitable for inner-level filtering).
* `nested`: Maintains relation between nested fields.

**34. How do you use aggregations (e.g., for analytics)?**
Use `aggs` to compute metrics (avg, sum) or group data (`terms`, `range`, etc.).

**35. What are runtime fields?**
Fields computed at query time using a script without storing them in the index.
