Great! Let’s start learning **RabbitMQ**, step-by-step in a very simple and practical way with JavaScript examples.

---

## 🐰 What is RabbitMQ?

**RabbitMQ** is a **message broker**. It lets applications send and receive messages between services — like a post office for your microservices.

### Why Use It?
- Decouples services (they don’t need to know about each other)
- Adds **asynchronous communication** (like queues in restaurants)
- Helps with **load balancing** and **retry logic**

---

## ⚙️ Core Concepts

| Concept       | Meaning                                                                 |
|---------------|-------------------------------------------------------------------------|
| **Producer**  | Sends messages                                                           |
| **Queue**     | Stores messages temporarily                                              |
| **Consumer**  | Listens and processes messages                                           |
| **Exchange**  | Receives messages from producers and routes to queues                   |
| **Routing Key**| Used to match messages to queues (like tags or labels)                  |
| **Binding**   | Connects queue to exchange using routing key                            |

---

## 🛠 How It Works (Simple View)

```txt
Producer --> Exchange --> Queue --> Consumer
```

---

## 🚀 Installation (Node.js)

First, install RabbitMQ and Node packages:

### 1. Start RabbitMQ (locally using Docker)
```bash
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```
> Access management UI at: http://localhost:15672  
> Username: `guest` | Password: `guest`

### 2. Install `amqplib`
```bash
npm install amqplib
```

---

## 📤 Producer Example (send message)

```js
const amqp = require('amqplib');

async function sendMessage() {
  const connection = await amqp.connect('amqp://localhost');
  const channel = await connection.createChannel();

  const queue = 'hello';
  const message = 'Hello from Himanshu!';

  await channel.assertQueue(queue);
  channel.sendToQueue(queue, Buffer.from(message));

  console.log('Sent:', message);
  await channel.close();
  await connection.close();
}

sendMessage();
```

---

## 📥 Consumer Example (receive message)

```js
const amqp = require('amqplib');

async function receiveMessage() {
  const connection = await amqp.connect('amqp://localhost');
  const channel = await connection.createChannel();

  const queue = 'hello';

  await channel.assertQueue(queue);
  console.log('Waiting for messages in', queue);

  channel.consume(queue, (msg) => {
    if (msg !== null) {
      console.log('Received:', msg.content.toString());
      channel.ack(msg);
    }
  });
}

receiveMessage();
```

---

## 🧠 Real-Life Use Cases

- Order processing in e-commerce
- Email/SMS notification systems
- Log processing and auditing
- Background task runners


Awesome! Let’s now move to the **next level of RabbitMQ**.

---

## 🧭 Overview of Exchange Types

RabbitMQ has different **exchange types** that define **how messages are routed** to queues:

| Exchange Type | Use Case                                                   | Example                                                 |
|---------------|-------------------------------------------------------------|----------------------------------------------------------|
| `direct`      | Route to queue with exact **routing key match**            | Send logs of level `error` to a specific queue           |
| `fanout`      | **Broadcast** to all queues                                 | Send notification to all services (SMS, Email, App)      |
| `topic`       | **Pattern-based routing** with `.` as separator             | Route messages like `user.created` or `user.deleted`     |
| `headers`     | Match based on message headers (rarely used)                | Match on metadata like `department: HR`, `type: urgent`  |

We’ll now see **examples** in JS for all 3 common types.

---

## 1️⃣ `Direct Exchange` Example

💡 **Send message to a queue if routing key matches exactly**

### Producer

```js
const amqp = require('amqplib');

async function directProducer() {
  const connection = await amqp.connect('amqp://localhost');
  const channel = await connection.createChannel();

  const exchange = 'logs_direct';
  const routingKey = 'error';
  const message = 'This is an error log!';

  await channel.assertExchange(exchange, 'direct', { durable: false });
  channel.publish(exchange, routingKey, Buffer.from(message));

  console.log(`Sent message with key "${routingKey}":`, message);
  await channel.close();
  await connection.close();
}

directProducer();
```

### Consumer

```js
const amqp = require('amqplib');

async function directConsumer() {
  const connection = await amqp.connect('amqp://localhost');
  const channel = await connection.createChannel();

  const exchange = 'logs_direct';
  const routingKey = 'error';

  await channel.assertExchange(exchange, 'direct', { durable: false });
  const q = await channel.assertQueue('', { exclusive: true }); // Temporary queue

  await channel.bindQueue(q.queue, exchange, routingKey);

  console.log('Waiting for messages...');
  channel.consume(q.queue, (msg) => {
    if (msg.content) {
      console.log(`Received [${msg.fields.routingKey}]:`, msg.content.toString());
    }
  });
}

directConsumer();
```

---

## 2️⃣ `Fanout Exchange` Example

💡 **Send message to all queues, routing key is ignored**

### Producer

```js
const amqp = require('amqplib');

async function fanoutProducer() {
  const connection = await amqp.connect('amqp://localhost');
  const channel = await connection.createChannel();

  const exchange = 'logs_fanout';
  const message = 'Broadcast: System will go down for maintenance!';

  await channel.assertExchange(exchange, 'fanout', { durable: false });
  channel.publish(exchange, '', Buffer.from(message));

  console.log('Broadcasted:', message);
  await channel.close();
  await connection.close();
}

fanoutProducer();
```

### Consumer

```js
const amqp = require('amqplib');

async function fanoutConsumer() {
  const connection = await amqp.connect('amqp://localhost');
  const channel = await connection.createChannel();

  const exchange = 'logs_fanout';

  await channel.assertExchange(exchange, 'fanout', { durable: false });
  const q = await channel.assertQueue('', { exclusive: true });

  await channel.bindQueue(q.queue, exchange, '');

  console.log('Waiting for broadcast messages...');
  channel.consume(q.queue, (msg) => {
    if (msg.content) {
      console.log('Received:', msg.content.toString());
    }
  });
}

fanoutConsumer();
```

---

## 3️⃣ `Topic Exchange` Example

💡 **Routing based on pattern** (e.g. `user.*`, `user.created`)

### Producer

```js
const amqp = require('amqplib');

async function topicProducer() {
  const connection = await amqp.connect('amqp://localhost');
  const channel = await connection.createChannel();

  const exchange = 'logs_topic';
  const routingKey = 'user.created';
  const message = 'New user created!';

  await channel.assertExchange(exchange, 'topic', { durable: false });
  channel.publish(exchange, routingKey, Buffer.from(message));

  console.log(`Sent message with key "${routingKey}":`, message);
  await channel.close();
  await connection.close();
}

topicProducer();
```

### Consumer (for all `user.*` messages)

```js
const amqp = require('amqplib');

async function topicConsumer() {
  const connection = await amqp.connect('amqp://localhost');
  const channel = await connection.createChannel();

  const exchange = 'logs_topic';
  const pattern = 'user.*'; // Match all user events

  await channel.assertExchange(exchange, 'topic', { durable: false });
  const q = await channel.assertQueue('', { exclusive: true });

  await channel.bindQueue(q.queue, exchange, pattern);

  console.log('Waiting for pattern-based messages...');
  channel.consume(q.queue, (msg) => {
    if (msg.content) {
      console.log(`Received [${msg.fields.routingKey}]:`, msg.content.toString());
    }
  });
}

topicConsumer();
```

---

## 📌 Summary

| Type     | Key Feature                   | Routing Key Example    |
|----------|-------------------------------|-------------------------|
| direct   | Exact match                   | `'error'`, `'info'`     |
| fanout   | Send to all queues            | `''` (ignored)          |
| topic    | Wildcard-based routing        | `'user.*'`, `'order.#'` |


Awesome! Let’s now add a **REST API** using **Express.js** to trigger order creation 🚀

---

## 🧾 What We’ll Do

- Add an Express server to `order-service`
- Create an API: `POST /orders`
- On hitting the API, publish an order to RabbitMQ

---

## 📦 Install Express

Go to your root and run:

```bash
cd order-system
npm install express
```

---

## 📁 Final Structure for `order-service`

```
order-service/
├── index.js         ← Starts the server
├── publisher.js     ← Publishes order
└── routes.js        ← Express route
```

---

### 🧠 `order-service/publisher.js`

No changes. Just reuse the `publishOrder()` function:

```js
const { createChannel } = require('../rabbitmq/connection');

async function publishOrder(order) {
  const exchange = 'order_events';
  const channel = await createChannel(exchange);

  channel.publish(exchange, '', Buffer.from(JSON.stringify(order)));
  console.log('[Order Service] Published order:', order);
}

module.exports = publishOrder;
```

---

### 🚏 `order-service/routes.js`

```js
const express = require('express');
const publishOrder = require('./publisher');

const router = express.Router();

router.post('/orders', async (req, res) => {
  try {
    const order = {
      orderId: Math.floor(Math.random() * 100000),
      userId: req.body.userId,
      items: req.body.items
    };

    await publishOrder(order);
    res.status(201).json({ message: 'Order placed!', order });
  } catch (err) {
    console.error('Error publishing order:', err);
    res.status(500).json({ error: 'Failed to place order' });
  }
});

module.exports = router;
```

---

### 🚀 `order-service/index.js`

```js
const express = require('express');
const bodyParser = require('body-parser');
const orderRoutes = require('./routes');

const app = express();
app.use(bodyParser.json());
app.use(orderRoutes);

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Order service running on http://localhost:${PORT}`);
});
```

---

## ✅ Test the API

### Start the server:

```bash
node order-service/index.js
```

### Make a POST request:

You can use Postman, or run:

```bash
curl -X POST http://localhost:3000/orders \
-H "Content-Type: application/json" \
-d '{"userId": 101, "items": ["book", "pen"]}'
```

---

### 📤 You’ll See Logs Like:

**In terminal running order-service:**
```txt
[Order Service] Published order: { orderId: 83472, userId: 101, items: [ 'book', 'pen' ] }
```

**In inventory-service terminal:**
```txt
[Inventory Service] Reducing stock for: 83472
```

**In billing-service terminal:**
```txt
[Billing Service] Generating invoice for: 83472
```

Perfect! Let’s now integrate **MongoDB** into the `order-service` so that each order is saved in a database 📦

---

## 🔧 What We’ll Do

1. Use **MongoDB** and **Mongoose**
2. Save each order before publishing to RabbitMQ
3. Keep the flow:
   - Receive order via REST
   - Save to DB
   - Publish to RabbitMQ

---

## 🧱 Step 1: Install Mongoose

From your `order-system` root:

```bash
npm install mongoose
```

---

## 📁 Updated Folder Structure for `order-service`

```
order-service/
├── db.js                 ← MongoDB connection
├── order.model.js        ← Order schema/model
├── index.js              ← Starts server
├── publisher.js          ← Publishes message
└── routes.js             ← Express routes
```

---

## 🔌 `order-service/db.js` – MongoDB connection

```js
const mongoose = require('mongoose');

async function connectDB() {
  try {
    await mongoose.connect('mongodb://localhost:27017/orderdb', {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });
    console.log('[MongoDB] Connected');
  } catch (err) {
    console.error('MongoDB connection failed:', err);
    process.exit(1);
  }
}

module.exports = connectDB;
```

---

## 📄 `order-service/order.model.js` – Mongoose Schema

```js
const mongoose = require('mongoose');

const orderSchema = new mongoose.Schema({
  orderId: Number,
  userId: Number,
  items: [String],
  createdAt: {
    type: Date,
    default: Date.now,
  },
});

const Order = mongoose.model('Order', orderSchema);
module.exports = Order;
```

---

## 🛣️ `order-service/routes.js` – Update to Save Order First

```js
const express = require('express');
const publishOrder = require('./publisher');
const Order = require('./order.model');

const router = express.Router();

router.post('/orders', async (req, res) => {
  try {
    const order = new Order({
      orderId: Math.floor(Math.random() * 100000),
      userId: req.body.userId,
      items: req.body.items,
    });

    await order.save();
    await publishOrder(order);

    res.status(201).json({ message: 'Order saved and published!', order });
  } catch (err) {
    console.error('Error placing order:', err);
    res.status(500).json({ error: 'Something went wrong' });
  }
});

module.exports = router;
```

---

## 🚀 `order-service/index.js` – Connect DB before starting server

```js
const express = require('express');
const bodyParser = require('body-parser');
const orderRoutes = require('./routes');
const connectDB = require('./db');

const app = express();
app.use(bodyParser.json());
app.use(orderRoutes);

const PORT = 3000;

connectDB().then(() => {
  app.listen(PORT, () => {
    console.log(`Order service running on http://localhost:${PORT}`);
  });
});
```

---

## ✅ MongoDB Setup (If you don’t have it already)

You can install MongoDB locally, or use Docker:

```bash
docker run -d -p 27017:27017 --name mongodb mongo
```

---

## 📬 Test Everything

```bash
curl -X POST http://localhost:3000/orders \
-H "Content-Type: application/json" \
-d '{"userId": 101, "items": ["headphones", "charger"]}'
```

Check `MongoDB`:

```bash
docker exec -it mongodb mongosh
use orderdb
db.orders.find().pretty()
```

---

## 🧠 Summary

✔ MongoDB saves the order  
✔ RabbitMQ handles the messaging  
✔ Other services act on the event

Awesome! Let’s now add **Retry Logic with DLQ (Dead Letter Queue)** in your system. This ensures **failed messages** are **not lost** and can be retried or inspected later. 🔁

---

## ✅ Goal:

If **Inventory Service** or **Billing Service** fails to process a message:
- it will be **moved to a DLQ**
- you can inspect, log, or even **retry it later manually or via scheduled jobs**

---

## 🛠 Step-by-Step Plan (we'll apply to `billing-service` as example):

1. 🔁 Create a retry queue with **TTL** (delay)
2. 🪦 Create a DLQ (dead-letter queue)
3. ⚙ Setup exchange bindings
4. 🔥 Simulate a failure in Billing Service
5. 📥 Failed message → retry → finally to DLQ

---

## 🔁 How Retry via DLX + TTL Works

```txt
           [main-queue]
               |
         (fails & reject)
               ↓
         [retry-queue] -- TTL 5s →
               ↓
        [main-exchange] (retries)
               |
         (fails again)
               ↓
            [DLQ]
```

---

## 📦 Setup in `billing-service/consumer.js`

```js
const { createChannel } = require('../rabbitmq/connection');

async function startBillingService() {
  const exchange = 'order_events';
  const retryExchange = 'billing_retry_exchange';
  const dlqExchange = 'billing_dlq_exchange';

  const channel = await createChannel(exchange);
  await channel.assertExchange(retryExchange, 'direct', { durable: true });
  await channel.assertExchange(dlqExchange, 'direct', { durable: true });

  // Step 1: Main queue with DLX set to retry exchange
  await channel.assertQueue('billing_main_queue', {
    durable: true,
    deadLetterExchange: retryExchange,
    deadLetterRoutingKey: 'retry',
  });
  await channel.bindQueue('billing_main_queue', exchange, '');

  // Step 2: Retry queue with TTL + DLX back to main queue
  await channel.assertQueue('billing_retry_queue', {
    durable: true,
    messageTtl: 5000, // Retry after 5 seconds
    deadLetterExchange: exchange,
    deadLetterRoutingKey: '',
  });
  await channel.bindQueue('billing_retry_queue', retryExchange, 'retry');

  // Step 3: DLQ (final resting place)
  await channel.assertQueue('billing_dlq_queue', { durable: true });
  await channel.bindQueue('billing_dlq_queue', dlqExchange, 'dlq');

  // Consumer
  channel.consume('billing_main_queue', async (msg) => {
    try {
      const order = JSON.parse(msg.content.toString());

      // 🧪 Simulate a failure
      if (order.userId === 500) throw new Error('Billing failed!');

      console.log('[Billing Service] Generating invoice for:', order.orderId);
      channel.ack(msg);
    } catch (err) {
      console.error('[Billing Service] Failed. Sending to retry...', err.message);
      // Reject and trigger DLX (goes to retry queue)
      channel.reject(msg, false);
    }
  });
}
module.exports = startBillingService;
```

---

## 🔁 What Happens?

- If order `userId === 500`, it simulates a failure.
- Message → goes to `billing_retry_queue` (waits 5s)
- → comes back to `billing_main_queue`
- Fails again → goes to `billing_dlq_queue`

---

## 🧪 Test It

In Postman or curl, send:

```bash
curl -X POST http://localhost:3000/orders \
-H "Content-Type: application/json" \
-d '{"userId": 500, "items": ["phone", "charger"]}'
```

You’ll see:

```txt
[Billing Service] Failed. Sending to retry... Billing failed!
(wait 5s)
[Billing Service] Failed. Sending to retry... Billing failed!
```

Eventually it will end up in DLQ.

You can check it via RabbitMQ UI at:  
[http://localhost:15672](http://localhost:15672) → Queues → `billing_dlq_queue`

---

## ✅ Summary

| Queue               | Purpose                         |
|---------------------|---------------------------------|
| `billing_main_queue` | Where billing listens normally |
| `billing_retry_queue`| Temporarily holds failed msgs  |
| `billing_dlq_queue`  | Permanent store for failed msgs|

---

## Next Steps?

You can now:
- ✅ Retry only 1–2 times (track `x-death` header)
- 🕓 Schedule periodic retry from DLQ
- 📊 Log all DLQ messages to MongoDB