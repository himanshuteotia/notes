### **Multi-Container Pod Example with Redis**
This example demonstrates a **multi-container Pod** where:
1. **A Node.js app** stores data in Redis.
2. **A Redis database** runs in the same Pod.
3. **They share a network** (containers communicate via `localhost`).

---

### **📌 Use Case:**
- The **Node.js app** connects to Redis and stores a sample key-value pair.
- The **Redis database** runs inside the same Pod.
- No need for an external Redis service—everything is contained within one Pod.

---

### **🚀 Pod YAML with Node.js and Redis**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis-app-pod
spec:
  containers:
    - name: redis
      image: redis:latest
      ports:
        - containerPort: 6379

    - name: node-app
      image: node:18
      command: ["sh", "-c"]
      args:
        - |
          npm install ioredis && \
          node -e "
          const Redis = require('ioredis');
          const redis = new Redis({ host: 'localhost', port: 6379 });
          redis.set('greeting', 'Hello from Node.js', () => {
            redis.get('greeting', (err, result) => {
              console.log('Stored value:', result);
            });
          });
          while (true) { sleep(10); }"
```

---

### **🔍 Explanation**
1. **Redis Container**:
   - Runs Redis on **port 6379**.
   - Other containers inside the Pod can access it using `localhost:6379`.

2. **Node.js App Container**:
   - Uses **`ioredis`** to connect to Redis.
   - **Stores a key-value pair** (`greeting: "Hello from Node.js"`).
   - Reads the value back and prints it.
   - Runs an infinite loop (`while (true) { sleep(10); }`) to keep the container alive.

---

### **📌 Deploy the Pod**
Apply the YAML:
```sh
kubectl apply -f redis-app-pod.yaml
```

Check if the Pod is running:
```sh
kubectl get pods
```

View logs of the Node.js app:
```sh
kubectl logs -f redis-app-pod -c node-app
```
✅ You should see:
```
Stored value: Hello from Node.js
```

---

### **💡 Why Use This Multi-Container Setup?**
✅ **Tightly coupled services**—No need for a separate Redis deployment.  
✅ **Local communication**—Node.js can access Redis via `localhost`, avoiding network overhead.  
✅ **Easy deployment**—Both app & Redis are packaged together.