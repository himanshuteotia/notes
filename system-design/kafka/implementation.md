**Basic Kafka implementation in Node.js** using the official Kafka client: **`kafkajs`**.

---

### ✅ Step 1: Install KafkaJS

```bash
npm install kafkajs
```

---

### ✅ Step 2: Kafka Producer (sends messages)

```js
// producer.js
const { Kafka } = require('kafkajs');

const kafka = new Kafka({
  clientId: 'my-app',
  brokers: ['localhost:9092'], // Replace with your Kafka broker address
});

const producer = kafka.producer();

const run = async () => {
  await producer.connect();
  await producer.send({
    topic: 'order-topic',
    messages: [
      { value: 'New order placed by customer' },
    ],
  });
  console.log('Message sent');
  await producer.disconnect();
};

run().catch(console.error);
```

---

### ✅ Step 3: Kafka Consumer (reads messages)

```js
// consumer.js
const { Kafka } = require('kafkajs');

const kafka = new Kafka({
  clientId: 'my-app',
  brokers: ['localhost:9092'],
});

const consumer = kafka.consumer({ groupId: 'order-group' });

const run = async () => {
  await consumer.connect();
  await consumer.subscribe({ topic: 'order-topic', fromBeginning: true });

  await consumer.run({
    eachMessage: async ({ topic, partition, message }) => {
      console.log(`Received: ${message.value.toString()}`);
    },
  });
};

run().catch(console.error);
```

---

### 🧪 How to Run

1. Make sure Kafka and Zookeeper are running (you can use Docker or install locally).
2. Run the consumer:

```bash
node consumer.js
```

3. Run the producer in another terminal:

```bash
node producer.js
```

---

### 📝 Notes

* Kafka should be running at `localhost:9092`. If you’re using Docker, adjust the broker IP accordingly.
* The topic (`order-topic`) should already exist or Kafka should be configured to auto-create it.
