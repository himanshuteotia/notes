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

let’s do this with **Avro + Node.js**.
We’ll use the popular npm package [`avsc`](https://www.npmjs.com/package/avsc) which handles Avro serialization and deserialization.

---

### Step 1: Install

```bash
npm install avsc
```

---

### Step 2: Define the Avro schema

```js
const avro = require('avsc');

// Define a schema
const type = avro.Type.forSchema({
  type: 'record',
  name: 'User',
  fields: [
    { name: 'id', type: 'int' },
    { name: 'name', type: 'string' },
    { name: 'email', type: ['null', 'string'], default: null }
  ]
});
```

---

### Step 3: Serialize (object → binary)

```js
// Original JS object
const user = { id: 1, name: 'Himanshu', email: 'himanshu@example.com' };

// Serialize it into a buffer (binary data)
const buffer = type.toBuffer(user);

console.log('Serialized (buffer):', buffer);
```

You’ll see something like:

```
<Buffer 02 0e 48 69 6d 61 6e 73 68 75 1c 68 69 6d 61 6e 73 68 75 40 65 78 61 6d 70 6c 65 2e 63 6f 6d>
```

That’s compact binary data — much smaller than JSON.

---

### Step 4: Deserialize (binary → object)

```js
// Deserialize the buffer back to JS object
const decodedUser = type.fromBuffer(buffer);

console.log('Deserialized (object):', decodedUser);
```

Output:

```js
{ id: 1, name: 'Himanshu', email: 'himanshu@example.com' }
```

---

✅ Now you’ve seen:

* **Serialization** = `object → buffer`
* **Deserialization** = `buffer → object`

---


Nice — let’s map this to a real Kafka flow so you can see how producer/consumer work with Avro in Node.js. I’ll show two approaches:

1. **Simple Avro (no schema registry)** — good for demos or trusted internal systems.
2. **Avro + Confluent Schema Registry** — production-ready, lets producers/consumers share schemas safely and evolve them.

I’ll use `kafkajs` for Kafka and `avsc` for Avro in the simple example. For schema-registry I’ll use `@kafkajs/confluent-schema-registry`. Run these locally or on your dev cluster.

---

# 1) Simple Avro with `avsc` (no registry)

Install:

```bash
npm i kafkajs avsc
```

Producer (`producer_simple.js`):

```js
// producer_simple.js
const { Kafka } = require('kafkajs');
const avro = require('avsc');

const kafka = new Kafka({ clientId: 'simple-producer', brokers: ['localhost:9092'] });
const producer = kafka.producer();

const userType = avro.Type.forSchema({
  type: 'record',
  name: 'User',
  fields: [
    { name: 'id', type: 'int' },
    { name: 'name', type: 'string' },
    { name: 'email', type: ['null', 'string'], default: null }
  ]
});

async function run() {
  await producer.connect();
  const user = { id: 1, name: 'Himanshu', email: 'himanshu@example.com' };
  const buffer = userType.toBuffer(user); // serialize to Buffer
  await producer.send({
    topic: 'users-avro-simple',
    messages: [{ value: buffer }]
  });
  console.log('sent');
  await producer.disconnect();
}

run().catch(console.error);
```

Consumer (`consumer_simple.js`):

```js
// consumer_simple.js
const { Kafka } = require('kafkajs');
const avro = require('avsc');

const kafka = new Kafka({ clientId: 'simple-consumer', brokers: ['localhost:9092'] });
const consumer = kafka.consumer({ groupId: 'simple-group' });

const userType = avro.Type.forSchema({
  type: 'record',
  name: 'User',
  fields: [
    { name: 'id', type: 'int' },
    { name: 'name', type: 'string' },
    { name: 'email', type: ['null', 'string'], default: null }
  ]
});

async function run() {
  await consumer.connect();
  await consumer.subscribe({ topic: 'users-avro-simple', fromBeginning: true });

  await consumer.run({
    eachMessage: async ({ message }) => {
      const buf = message.value; // Buffer
      const user = userType.fromBuffer(buf); // deserialize
      console.log('received', user);
    }
  });
}

run().catch(console.error);
```

Notes:

* Both sides must use the same schema object/version. If they differ, deserialization will fail or produce wrong data.
* No automatic schema management — you must coordinate schema changes manually.

---

# 2) Avro + Confluent Schema Registry (recommended for real systems)

Why use a schema registry? It stores schema versions, gives each schema an ID, and provides compatibility checks. Producers register/encode by schema id; consumers decode using the id encoded with the data. This makes schema evolution safe.

Install:

```bash
npm i kafkajs @kafkajs/confluent-schema-registry
```

Producer with registry (`producer_registry.js`):

```js
// producer_registry.js
const { Kafka } = require('kafkajs');
const { SchemaRegistry, COMPATIBILITY } = require('@kafkajs/confluent-schema-registry');

const kafka = new Kafka({ clientId: 'reg-producer', brokers: ['localhost:9092'] });
const producer = kafka.producer();

// Schema Registry client (confluent schema registry URL)
const registry = new SchemaRegistry({ host: 'http://localhost:8081' });

const userSchema = {
  type: 'record',
  name: 'User',
  namespace: 'com.example',
  fields: [
    { name: 'id', type: 'int' },
    { name: 'name', type: 'string' },
    { name: 'email', type: ['null', 'string'], default: null }
  ]
};

async function run() {
  await producer.connect();

  // Register schema (or retrieve existing id) under a subject name.
  // Subject name convention: <topic>-value (commonly)
  const subject = 'users-avro-registry-value';
  const { id } = await registry.register({ type: 'AVRO', schema: JSON.stringify(userSchema) }, { subject });

  const user = { id: 2, name: 'Himanshu', email: 'him@example.com' };

  // Encode automatically prepends magic byte + schema id per Confluent wire format
  const encoded = await registry.encode(id, user);

  await producer.send({
    topic: 'users-avro-registry',
    messages: [{ value: encoded }]
  });

  console.log('sent with schema id', id);
  await producer.disconnect();
}

run().catch(console.error);
```

Consumer with registry (`consumer_registry.js`):

```js
// consumer_registry.js
const { Kafka } = require('kafkajs');
const { SchemaRegistry } = require('@kafkajs/confluent-schema-registry');

const kafka = new Kafka({ clientId: 'reg-consumer', brokers: ['localhost:9092'] });
const consumer = kafka.consumer({ groupId: 'reg-group' });

const registry = new SchemaRegistry({ host: 'http://localhost:8081' });

async function run() {
  await consumer.connect();
  await consumer.subscribe({ topic: 'users-avro-registry', fromBeginning: true });

  await consumer.run({
    eachMessage: async ({ message }) => {
      const buf = message.value; // Buffer with Confluent wire format
      const decoded = await registry.decode(buf); // returns JS object
      console.log('received decoded', decoded);
    }
  });
}

run().catch(console.error);
```

Important details about Confluent wire format:

* The encoded buffer includes:

  * 1 magic byte (0)
  * 4-byte schema id (big-endian)
  * Avro-encoded payload
* The registry client handles the encode/decode and schema id plumbing for you.

---

# Extra tips and pitfalls

* **Compatibility rules**: The registry supports compatibility settings (BACKWARD, FORWARD, FULL). Set rules to control allowed schema changes (e.g., you can add a field with a default and remain backward-compatible).
* **Subject naming**: Common practice: `topic-name-value` and `topic-name-key` if you also serialize keys.
* **Keys**: Kafka message keys can also be Avro-encoded; treat them similarly.
* **Language interoperability**: Avro + registry works well across languages — producer in Java, consumer in Node.js works fine as long as they use the same schema id.
* **Performance**: Avro binary is smaller and faster than JSON; schema-registry adds a small overhead but gives safety and evolvability.
* **Local testing**: You can run a local Confluent Schema Registry and Kafka (e.g., Confluent Platform or docker-compose images).

