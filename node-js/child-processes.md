In Node.js, **child processes** are used to run separate instances of programs or scripts, allowing you to execute CPU-intensive or isolated tasks without blocking the main event loop. You can create child processes using the **`child_process`** module.

---

## ✅ **Types of Child Processes in Node.js**
Node.js provides four main ways to create child processes:

### 1️⃣ **`spawn()`** - Runs a command as a child process
- Best for running **long-running** processes and continuously receiving output.
- Streams data in chunks (stdout, stderr).

**Example: Running a shell command (`ls -l`)**
```javascript
const { spawn } = require('child_process');

const child = spawn('ls', ['-l']); // Runs "ls -l" in a Unix shell

child.stdout.on('data', (data) => {
    console.log(`Output: ${data}`);
});

child.stderr.on('data', (data) => {
    console.error(`Error: ${data}`);
});

child.on('close', (code) => {
    console.log(`Child process exited with code ${code}`);
});
```
✅ **Use `spawn()` when** you need **real-time output** (e.g., running a server, streaming logs).

---

### 2️⃣ **`exec()`** - Runs a command and buffers output
- Works like `spawn()`, but **buffers the output** instead of streaming it.
- Use this when you need **small amounts of output**.

**Example: Running a shell command and getting the output**
```javascript
const { exec } = require('child_process');

exec('ls -l', (error, stdout, stderr) => {
    if (error) {
        console.error(`Error: ${error.message}`);
        return;
    }
    if (stderr) {
        console.error(`Stderr: ${stderr}`);
        return;
    }
    console.log(`Output:\n${stdout}`);
});
```
✅ **Use `exec()` when** you need the **full output at once** rather than streaming.

---

### 3️⃣ **`execFile()`** - Runs an executable file directly
- Similar to `exec()`, but **runs an executable file directly** without a shell.
- Safer than `exec()` because it avoids shell command injection risks.

**Example: Running a script file**
```javascript
const { execFile } = require('child_process');

execFile('./script.sh', (error, stdout, stderr) => {
    if (error) {
        console.error(`Error: ${error.message}`);
        return;
    }
    console.log(`Output: ${stdout}`);
});
```
✅ **Use `execFile()` when** running **binary executables** like `python`, `ffmpeg`, or scripts.

---

### 4️⃣ **`fork()`** - Creates a new Node.js process
- Specially designed for running **Node.js scripts** as child processes.
- Enables **message passing** via `child.send()` and `process.on('message')`.

**Example: Creating a child Node.js process**
#### **`parent.js`**
```javascript
const { fork } = require('child_process');

const child = fork('./child.js');

child.send({ msg: 'Hello from parent' });

child.on('message', (message) => {
    console.log('Received from child:', message);
});
```
#### **`child.js`**
```javascript
process.on('message', (message) => {
    console.log('Received from parent:', message);
    process.send({ msg: 'Hello from child' });
});
```
✅ **Use `fork()` when** you need to **spawn another Node.js script** and communicate via messages.

---

## 🚀 **Comparison of Child Process Methods**
| Method | Use Case | Streams Data? | Runs External Commands? | Supports IPC (Messaging)? |
|--------|---------|-------------|------------------------|--------------------------|
| **spawn()** | Long-running processes, real-time output | ✅ Yes | ✅ Yes | ❌ No |
| **exec()** | Short commands, buffered output | ❌ No (Buffers) | ✅ Yes | ❌ No |
| **execFile()** | Running executable files | ❌ No (Buffers) | ✅ Yes | ❌ No |
| **fork()** | Running Node.js child processes | ✅ Yes | ❌ No | ✅ Yes |

---

## 🎯 **When to Use Which?**
- **Use `spawn()`** when you need to **stream** large amounts of data (e.g., real-time logs).
- **Use `exec()`** when you need the **entire output at once**.
- **Use `execFile()`** when running **binaries** or scripts **directly**.
- **Use `fork()`** when you need **a Node.js child process with message passing**.