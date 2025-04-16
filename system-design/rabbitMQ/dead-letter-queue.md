Let’s now dive into **Retry Mechanism, DLQ (Dead Letter Queue), Delayed Messages, and how RabbitMQ fits into microservices** — all with simple JavaScript examples. 💥

---

## 🔁 **Retry Mechanism & DLQ (Dead Letter Queue)**

### 💡 What is DLQ?
A **Dead Letter Queue** stores **failed or rejected messages** so they don’t get lost. You can later check or reprocess them.

---

### ✅ Scenario:

1. You try processing a message.
2. It fails (e.g., API error).
3. Instead of losing it, RabbitMQ moves it to a **DLQ**.
4. You can **log, alert, or retry later**.

---

### 🛠 Step-by-Step Implementation

#### Step 1: Setup main queue with DLQ

```js
await channel.assertExchange('main-exchange', 'direct');

await channel.assertQueue('main-queue', {
  durable: true,
  deadLetterExchange: 'dlx-exchange',
  deadLetterRoutingKey: 'dlx-key'
});

await channel.bindQueue('main-queue', 'main-exchange', 'task');
```

#### Step 2: Setup DLQ

```js
await channel.assertExchange('dlx-exchange', 'direct');

await channel.assertQueue('dlq', { durable: true });
await channel.bindQueue('dlq', 'dlx-exchange', 'dlx-key');
```

#### Step 3: Consumer with manual reject

```js
channel.consume('main-queue', (msg) => {
  const content = msg.content.toString();
  console.log('Received:', content);

  const failed = true; // simulate failure

  if (failed) {
    console.log('Failed. Moving to DLQ...');
    channel.reject(msg, false); // false = don't requeue
  } else {
    channel.ack(msg);
  }
});
```

> ✨ If a message fails, it automatically goes to the `dlq` queue.

---

## 🕒 **Delayed Message (Retry after some time)**

RabbitMQ has no built-in delay, but we can use a **TTL (Time-To-Live) + DLX** trick.

### Step-by-Step

#### Step 1: Create a delay queue

```js
await channel.assertQueue('delay-queue', {
  durable: true,
  messageTtl: 5000, // 5 seconds delay
  deadLetterExchange: 'main-exchange',
  deadLetterRoutingKey: 'task'
});
```

#### Step 2: Send message to delay-queue

```js
channel.sendToQueue('delay-queue', Buffer.from('Retry after delay'));
```

> It waits in `delay-queue` for 5 seconds, then goes to `main-queue` via `main-exchange`.

---

## 🧱 RabbitMQ in Microservices

Let’s say you have these services:

- 🛒 Order Service
- 📦 Inventory Service
- 🧾 Billing Service

### Instead of:
Each calling each other via HTTP (tight coupling)

### Use RabbitMQ:
- Order Service emits event `order.created`
- Inventory + Billing subscribe to that event
- Each service works independently

This makes the system:
✅ **Scalable**  
✅ **Loose-coupled**  
✅ **Async & Resilient**

---

## 🧪 Real Use Example in Microservices

```js
// Order Service
channel.publish('events', 'order.created', Buffer.from(JSON.stringify({ orderId: 123 })));

// Inventory Service
channel.bindQueue('inventory-queue', 'events', 'order.created');
// consume and reduce stock

// Billing Service
channel.bindQueue('billing-queue', 'events', 'order.created');
// consume and generate invoice
```