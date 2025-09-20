### Serialization

Think of **serialization** as **turning a live object into a format that can be stored or sent over the network**.

* Example: You have a JavaScript object like

  ```js
  { id: 1, name: "Himanshu" }
  ```

  If you want to send it through Kafka, save it to disk, or push it over HTTP, you can’t just pass the object as-is.
* Serialization converts it into something like:

  * JSON → `{"id":1,"name":"Himanshu"}` (human-readable)
  * Avro/Protobuf → compact binary (not human-readable, but faster and smaller).

### Deserialization

This is the reverse process: **taking the serialized data and reconstructing the original object**.

* If you receive a Kafka message as Avro binary, deserialization uses the **Avro schema** to rebuild:

  ```js
  { id: 1, name: "Himanshu" }
  ```

### Why does this matter?

* Machines need a consistent format to communicate.
* Serialization ensures data is portable, compact, and understood by different services (written in different languages).
* Deserialization ensures the receiver interprets the data correctly, using the schema.

---

💡 Quick analogy:

* **Serialization** = Packing your clothes into a suitcase before a trip.
* **Deserialization** = Unpacking them at the destination.

Do you want me to show you **how serialization & deserialization works in Avro with a Node.js example**?
