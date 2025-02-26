### **Question:**  
I have two API routes—one is CPU-intensive, while the other simply returns a string. When tested separately using **autocannon**, the CPU-bound route handles **50 requests in 10 seconds**, whereas the lightweight route processes **14,000 requests in 10 seconds**.  

However, when I ran the CPU-intensive route continuously for **60 seconds** and simultaneously tested the lightweight route for **10 seconds**, the lightweight route's request rate dropped to just **10 requests per 10 seconds**.  

How can I improve the availability of the lightweight route in this scenario?

---

### **Solution:**  
The issue occurs because the CPU-intensive route monopolizes the event loop, reducing the responsiveness of the lightweight route. Here’s how you can improve availability:

#### **1. Offload CPU-Intensive Work to Worker Threads**
Instead of blocking the main event loop, use **Worker Threads** for the CPU-intensive task. This ensures that the main thread remains free to handle lightweight requests.

Example using Worker Threads:
```javascript
const { Worker, isMainThread, parentPort } = require('worker_threads');

if (isMainThread) {
    const express = require('express');
    const app = express();

    app.get('/cpu-intensive', (req, res) => {
        const worker = new Worker(__filename);
        worker.on('message', (result) => res.json({ result }));
    });

    app.get('/light-request', (req, res) => {
        res.send('Hello, world!');
    });

    app.listen(3000, () => console.log('Server running on port 3000'));
} else {
    // CPU-intensive task inside the worker thread
    function heavyComputation() {
        let sum = 0;
        for (let i = 0; i < 1e9; i++) sum += i;
        return sum;
    }

    parentPort.postMessage(heavyComputation());
}
```
👉 This ensures that CPU-intensive work runs in a separate thread, keeping the event loop responsive.

---

#### **2. Use `setImmediate()` for Cooperative Scheduling**  
If you can’t use worker threads, break the CPU-intensive task into smaller chunks and schedule them using `setImmediate()`, allowing the event loop to process other requests in between.

Example:
```javascript
app.get('/cpu-intensive', (req, res) => {
    let sum = 0;
    let i = 0;

    function compute() {
        if (i < 1e9) {
            let end = i + 1e6;
            while (i < end) sum += i++;
            setImmediate(compute); // Yield control back to the event loop
        } else {
            res.json({ result: sum });
        }
    }

    compute();
});
```
👉 This prevents event loop starvation, improving the availability of other routes.

---

#### **3. Run a Cluster of Processes (Node.js Cluster Mode)**
If your server has multiple CPU cores, you can leverage **clustering** to spawn multiple processes, each handling different requests independently.

Example using **Cluster**:
```javascript
const cluster = require('cluster');
const os = require('os');
const express = require('express');

if (cluster.isMaster) {
    const numCPUs = os.cpus().length;
    for (let i = 0; i < numCPUs; i++) cluster.fork();
} else {
    const app = express();

    app.get('/cpu-intensive', (req, res) => {
        let sum = 0;
        for (let i = 0; i < 1e9; i++) sum += i;
        res.json({ result: sum });
    });

    app.get('/light-request', (req, res) => {
        res.send('Hello, world!');
    });

    app.listen(3000, () => console.log(`Worker ${process.pid} started`));
}
```
👉 With clustering, the lightweight request can still be handled by other processes, even if one is busy with a CPU-intensive task.

---

### **Best Approach for Your Case**
- **If your server has multiple cores:** Use **Clustering** to distribute CPU-bound work across multiple processes.  
- **If you're running on a single core:** Use **Worker Threads** to offload intensive tasks.  
- **If Worker Threads aren’t an option:** Use **setImmediate()** for cooperative scheduling.  

This will significantly **improve availability** and allow both routes to perform efficiently. 🚀