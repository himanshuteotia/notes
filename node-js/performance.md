**production-ready Node.js performance checklist** — practical and to the point.

---

## ✅ Node.js Performance Checklist

### 🔁 Event Loop

➭ Avoid blocking code (`sync` versions of `fs`, `crypto`, `zlib`, etc)
➭ Offload CPU-heavy work to `worker_threads` or a queue service
➭ Monitor event loop lag (`perf_hooks`, `blocked-at`, etc)

---

### 🧠 Async & Non-blocking Code

➭ Use `async/await` or Promises for all I/O
➭ Don’t mix sync and async logic (e.g. `fs.readFileSync` in a request handler)
➭ Use batching for repetitive DB/API calls

---

### ⚙️ Process & Clustering

➭ Use `cluster` or `worker_threads` to utilize all CPU cores
➭ Run behind a process manager (PM2, systemd, Docker, etc)
➭ Gracefully handle crashes and restarts

---

### 🔥 Caching

➭ In-memory cache (e.g. `node-cache`, LRU cache) for hot data
➭ Use Redis for shared/distributed caching
➭ Cache expensive DB queries or computations

---

### 🌐 HTTP & Networking

➭ Enable **Keep-Alive** on HTTP agents
➭ Use `gzip` or `brotli` compression (`compression` middleware)
➭ Avoid N+1 API/DB calls
➭ Use CDN for static assets
➭ Limit payload size with body parsers (`express.json({ limit: '1mb' })`)

---

### 🚀 Efficient Code & Dependencies

➭ Don’t import entire libraries (`import { debounce } from 'lodash'`)
➭ Remove unused npm packages
➭ Prefer native or faster libraries (`fast-json-stringify`, `undici`, etc)
➭ Avoid `eval`, `with`, and dynamic property access

---

### 🔁 Streaming

➭ Stream large files and responses using `fs.createReadStream()`
➭ Pipe streams instead of loading full content in memory

---

### 📦 Database Optimization

➭ Use connection pooling
➭ Use indexes smartly
➭ Avoid repeated joins or unfiltered full scans
➭ Paginate large results
➭ Don't query on every request if result doesn't change (cache it)

---

### 🧠 V8 Optimization

➭ Avoid changing object shapes dynamically
➭ Pre-define all keys when creating objects
➭ Don’t delete properties — set to `null` instead
➭ Avoid deeply nested closures in hot paths

---

### 🧪 Monitoring & Profiling

➭ Add health checks (`/healthz`)
➭ Use `node --inspect` for debugging
➭ Use `clinic.js`, `0x`, `autocannon` for profiling & benchmarking
➭ Log memory usage (`process.memoryUsage()`) and event loop lag
➭ Use APM tools (New Relic, Datadog, Elastic, Sentry)

---

### 🧼 Memory Management

➭ Watch for memory leaks (use `heapdump`, Chrome DevTools snapshots)
➭ Use `--max-old-space-size` to control heap size
➭ Restart process if heap grows consistently (PM2 max\_memory\_restart)

---

### 🧯 Error Handling

➭ Catch and log all async errors
➭ Use `process.on('uncaughtException')` and `unhandledRejection` to log & exit
➭ Return consistent error responses (avoid internal leaks in messages)
