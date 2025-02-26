## 🔹 **Difference Between Buffer and Stream in Node.js**

Both **Buffer** and **Stream** are used in Node.js to handle data efficiently, especially when working with **files, network requests, or large data processing**. However, they have **different approaches** to handling data.

---

## 🚀 **1. Buffer**
- A **Buffer** is a **temporary memory** storage that holds binary data.
- It loads the **entire** data into memory **before** processing it.
- Useful when working with **small files** or **binary data** like images, videos, and encryption.

### ✅ **Example of Buffer (Reading a file)**
```javascript
const fs = require('fs');

fs.readFile('large-file.txt', (err, data) => {
    if (err) throw err;
    console.log(data.toString()); // Converts buffer to string
});
```
### 🔴 **Disadvantage** of Buffer:
- **Consumes a lot of memory** if the file is large.
- **Blocks the event loop** while loading data into memory.

---

## 🚀 **2. Stream**
- A **Stream** processes data **piece by piece (chunks)** rather than loading everything at once.
- **Efficient for large files** because it reduces memory usage.
- Commonly used for **reading/writing files, network communication, and HTTP requests**.

### ✅ **Example of Stream (Reading a file)**
```javascript
const fs = require('fs');

const readableStream = fs.createReadStream('large-file.txt');

readableStream.on('data', (chunk) => {
    console.log('Received Chunk:', chunk.toString());
});
```
### ✅ **Example of Stream (Piping Data)**
You can directly **pipe** a readable stream into a writable stream.
```javascript
const fs = require('fs');

const readable = fs.createReadStream('input.txt');
const writable = fs.createWriteStream('output.txt');

readable.pipe(writable);
```
### 🟢 **Advantage** of Stream:
- **Does not load the entire data into memory**, making it efficient.
- **Non-blocking**, so it does not slow down the event loop.

---

## 🔥 **Key Differences: Buffer vs. Stream**
| Feature  | Buffer | Stream |
|----------|--------|--------|
| **Data Processing** | Loads entire data at once | Processes data in chunks |
| **Memory Usage** | High (depends on file size) | Low (handles large files efficiently) |
| **Performance** | Slow for large files | Fast for large files |
| **Blocking vs Non-blocking** | Blocks execution while loading | Non-blocking, efficient |

---

## 🎯 **When to Use What?**
| Use Case | Use **Buffer** | Use **Stream** |
|----------|--------------|---------------|
| Small files (config, logs) | ✅ Yes | ❌ No |
| Large files (videos, big data) | ❌ No | ✅ Yes |
| Network requests (HTTP) | ❌ No | ✅ Yes |
| Data transformation (e.g., compression) | ❌ No | ✅ Yes |

---

### **Conclusion**
- **Use Buffers** when working with **small amounts of data** or when you need **instant access** to the whole data at once.
- **Use Streams** when working with **large data sets**, **files**, or **network requests** to improve performance and memory efficiency.