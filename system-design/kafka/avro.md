Avro schema is basically a **contract** that defines the structure of your data when you’re using **Apache Avro**, which is a data serialization system.

Here’s the idea:

* Avro lets you encode your data in a compact binary format.
* To make sure everyone who reads or writes that data knows what fields exist, what types they are, and whether they’re optional, you define an **Avro schema**.

### Key points about Avro schema:

1. **Written in JSON** – The schema itself is a JSON document.
2. **Defines types and structure** – It describes fields, their types (string, int, array, record, etc.), and default values.
3. **Used for serialization & deserialization** – The writer (producer) and reader (consumer) both use the schema to interpret the data.
4. **Supports schema evolution** – You can add new fields (with defaults), rename, or remove fields carefully without breaking older data.

### Example

Here’s a simple Avro schema for a "User" record:

```json
{
  "type": "record",
  "name": "User",
  "namespace": "example.avro",
  "fields": [
    { "name": "id", "type": "int" },
    { "name": "name", "type": "string" },
    { "name": "email", "type": ["null", "string"], "default": null }
  ]
}
```

* **type: record** → means this schema defines a record (like a table row or an object).
* **fields** → list of key-value definitions for each property.
* `"email": ["null", "string"]` → means the field can be null or a string (a union type).

---

👉 You usually see Avro schema used with **Kafka** (Kafka messages often use Avro), **Hadoop**, or other distributed systems, because it’s compact, fast, and schema-driven.
