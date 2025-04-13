**SSE (Server-Sent Events)** is a transport mechanism that allows a **server to push real-time updates to the browser over a single long-lived HTTP connection**. It's part of the HTML5 standard and is especially useful for **live notifications, stock tickers, or real-time dashboards**.

---

### 🔧 How SSE (Server-Sent Events) Transport Works:

1. **Client opens a connection** using JavaScript:
   ```javascript
   const eventSource = new EventSource('/events');
   eventSource.onmessage = (event) => {
     console.log('New message:', event.data);
   };
   ```

2. **Server keeps the HTTP connection open** and pushes updates in plain text:
   ```
   data: Hello from server\n\n
   data: Another message\n\n
   ```

3. The browser automatically **reconnects if the connection drops**.

---

### ✅ Pros:
- Simple and lightweight (uses HTTP, no need for WebSocket protocol).
- Built-in **reconnect** and **event ID** support.
- **Great for unidirectional communication** (server ➝ client).

### ❌ Cons:
- **Unidirectional only** (client can't send messages back).
- Works only over **HTTP/1.1** (not HTTP/2 natively).
- Not supported in **some older browsers** (mainly IE).

---

### 🔁 Comparison with WebSocket:
| Feature          | SSE                     | WebSocket              |
|------------------|-------------------------|-------------------------|
| Direction        | Server ➝ Client only     | Full-duplex (2-way)     |
| Protocol         | HTTP                    | WS (WebSocket protocol) |
| Browser support  | Most modern browsers     | Most modern browsers    |
| Use case         | Notifications, feeds    | Chat, games, live apps  |

Great! Here's a **simple Node.js example** of using **SSE (Server-Sent Events)** for real-time updates:


## 🔧 Node.js SSE Example

### 1. **Backend (Express server)**

```javascript
const express = require('express');
const app = express();
const PORT = 3000;

app.get('/events', (req, res) => {
  // Set headers for SSE
  res.setHeader('Content-Type', 'text/event-stream');
  res.setHeader('Cache-Control', 'no-cache');
  res.setHeader('Connection', 'keep-alive');

  // Send a message every 5 seconds
  const intervalId = setInterval(() => {
    const data = `data: ${new Date().toLocaleTimeString()}\n\n`;
    res.write(data);
  }, 5000);

  // Handle connection close
  req.on('close', () => {
    clearInterval(intervalId);
    res.end();
  });
});

app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
```

---

### 2. **Frontend (HTML + JS)**

```html
<!DOCTYPE html>
<html>
  <body>
    <h1>Server Time</h1>
    <div id="output"></div>

    <script>
      const eventSource = new EventSource('http://localhost:3000/events');
      eventSource.onmessage = (event) => {
        document.getElementById('output').innerHTML = `Time: ${event.data}`;
      };
    </script>
  </body>
</html>
```

---

## ✅ What This Does:
- Backend sends the current time every 5 seconds.
- Frontend receives it and displays it live.
- All over a **single persistent HTTP connection**.

Perfect! Here's how you can **broadcast SSE messages to multiple connected clients** using Node.js and Express.

---

## ✅ Goal:
Multiple clients connect → Server stores their connections → Server broadcasts the same message to all clients

---

## 🔧 Updated Node.js Code: Broadcast to All Connected Clients

```javascript
const express = require('express');
const app = express();
const PORT = 3000;

// Store all client connections
let clients = [];

app.get('/events', (req, res) => {
  // Set SSE headers
  res.setHeader('Content-Type', 'text/event-stream');
  res.setHeader('Cache-Control', 'no-cache');
  res.setHeader('Connection', 'keep-alive');

  // Push this client into the array
  clients.push(res);

  // Remove client on disconnect
  req.on('close', () => {
    clients = clients.filter(client => client !== res);
    res.end();
  });
});

// Broadcast to all clients every 5 seconds
setInterval(() => {
  const message = `data: Broadcast at ${new Date().toLocaleTimeString()}\n\n`;
  clients.forEach(client => client.write(message));
}, 5000);

app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
```

---

## 🧪 Try it:
- Open `http://localhost:3000/events` in **multiple browser tabs**.
- All tabs will receive the **same broadcast message every 5 seconds**.



Awesome! SSE follows a **specific text-based format** to send events over the open HTTP connection. Let's break it down:

---

## ✅ **SSE Message Format from Server**

Each message should end with **two newlines (`\n\n`)**.

### 🔹 Basic format:

```plaintext
data: Hello, world!\n\n
```

This sends a simple message:  
➡️ `Hello, world!`

---

### 🔹 Multiple lines in one message:

```plaintext
data: Line 1
data: Line 2
data: Line 3

```

The browser receives:
```
Line 1
Line 2
Line 3
```

---

### 🔹 Custom Event Names (Optional)

```plaintext
event: update
data: Updated stock price\n\n
```

JavaScript can listen to this custom event like:

```javascript
eventSource.addEventListener('update', (event) => {
  console.log('Got update:', event.data);
});
```

---

### 🔹 Message ID (Optional)

```plaintext
id: 1234
data: Something important\n\n
```

- The browser keeps track of `id` and sends it back when reconnecting (for resuming missed events).

---

### 🔹 Retry Delay (Optional)

```plaintext
retry: 10000
```

- Tells browser: if connection drops, wait 10 seconds before reconnecting.

---

## 💡 Final Example

```plaintext
id: 42
event: message
data: Hello from server!

```

---

## 🧪 In Node.js:

```javascript
res.write(`data: Hello, world!\n\n`);
res.write(`event: update\ndata: Price changed!\n\n`);
res.write(`id: 123\ndata: Track me\n\n`);
```

You **can send JSON over SSE**, but since SSE is just **plain text**, you need to **stringify the JSON** manually before sending it.

---

## ✅ How to Send JSON in SSE

### 🔹 Server Side (Node.js)

```javascript
const data = { price: 102.5, symbol: "AAPL" };
res.write(`data: ${JSON.stringify(data)}\n\n`);
```

👉 This sends:
```plaintext
data: {"price":102.5,"symbol":"AAPL"}

```

---

## 🖥️ Frontend (JavaScript)

```javascript
const eventSource = new EventSource('/events');

eventSource.onmessage = (event) => {
  const parsedData = JSON.parse(event.data);
  console.log('Stock:', parsedData.symbol, 'Price:', parsedData.price);
};
```

---

## ⚠️ Important Notes:
- Always end the message with **`\n\n`**.
- Only the `data:` part is stringified. You cannot send raw objects.
- Make sure to escape special characters in your JSON if needed (usually taken care of by `JSON.stringify`).

Here's how to send **JSON with custom event types** in SSE and how to trigger it via a **POST request** (e.g., admin pushing a message to all connected clients).

---

## 🔧 Full Example: SSE with JSON + Custom Event + Manual Trigger via POST

---

### 🧠 1. Server (Node.js - Express)

```javascript
const express = require('express');
const app = express();
app.use(express.json());

const PORT = 3000;
let clients = [];

// SSE endpoint
app.get('/events', (req, res) => {
  res.setHeader('Content-Type', 'text/event-stream');
  res.setHeader('Cache-Control', 'no-cache');
  res.setHeader('Connection', 'keep-alive');

  clients.push(res);

  req.on('close', () => {
    clients = clients.filter(client => client !== res);
    res.end();
  });
});

// POST endpoint to broadcast a message
app.post('/broadcast', (req, res) => {
  const { event, payload } = req.body;

  const message = [
    `event: ${event}`,
    `data: ${JSON.stringify(payload)}`,
    '', // extra line means \n\n
  ].join('\n');

  clients.forEach(client => client.write(message + '\n'));
  res.json({ success: true });
});

app.listen(PORT, () => {
  console.log(`Listening on http://localhost:${PORT}`);
});
```

---

### 💻 2. Frontend (HTML + JavaScript)

```html
<!DOCTYPE html>
<html>
  <body>
    <h2>Live Updates</h2>
    <div id="updates"></div>

    <script>
      const eventSource = new EventSource('http://localhost:3000/events');

      eventSource.addEventListener('stockUpdate', (event) => {
        const data = JSON.parse(event.data);
        document.getElementById('updates').innerHTML +=
          `<p>${data.symbol}: ₹${data.price}</p>`;
      });
    </script>
  </body>
</html>
```

---

### 📤 3. Test POST Request (using `curl` or Postman)

#### Sample `curl`:

```bash
curl -X POST http://localhost:3000/broadcast \
  -H "Content-Type: application/json" \
  -d '{"event": "stockUpdate", "payload": { "symbol": "RELIANCE", "price": 2750 }}'
```

✅ All clients listening on `/events` will receive this as a **custom `stockUpdate` event** with JSON payload.