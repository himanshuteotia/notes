## **Setting Up Autoscaling (HPA) on AWS EKS**
Kubernetes **Horizontal Pod Autoscaler (HPA)** automatically scales the number of pods **based on CPU or memory utilization**. This ensures that your application handles increased traffic efficiently.

---

## **Step 1: Install Metrics Server**
The **metrics server** is required for **HPA** to fetch CPU/memory usage from pods.

### **Deploy the Metrics Server**
```sh
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
**Verify that the Metrics Server is Running:**
```sh
kubectl get deployment -n kube-system metrics-server
```
If you see `Running`, it means the metrics server is active.

---

## **Step 2: Update Your Deployment with Resource Limits**
For **HPA to work**, pods must have **CPU requests and limits** defined in their Deployment.

### **Modify `deployment.yaml`**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nodejs-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nodejs-app
  template:
    metadata:
      labels:
        app: nodejs-app
    spec:
      containers:
        - name: nodejs-app
          image: <aws-account-id>.dkr.ecr.us-east-1.amazonaws.com/k8s-node-app:latest
          ports:
            - containerPort: 3000
          resources:
            requests:
              cpu: "100m"
              memory: "128Mi"
            limits:
              cpu: "500m"
              memory: "256Mi"
```
- **Requests:** Minimum CPU/memory required.
- **Limits:** Maximum CPU/memory allowed.

**Apply the changes:**
```sh
kubectl apply -f deployment.yaml
```

---

## **Step 3: Create the HPA (Horizontal Pod Autoscaler)**
The **HPA** monitors pod resource usage and scales the number of pods dynamically.

### **Create `hpa.yaml`**
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nodejs-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nodejs-app
  minReplicas: 2
  maxReplicas: 5
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```
- **`minReplicas: 2`** → Minimum number of pods.
- **`maxReplicas: 5`** → Maximum number of pods.
- **`averageUtilization: 50`** → If CPU usage **exceeds 50%**, new pods are added.

Apply the **HPA**:
```sh
kubectl apply -f hpa.yaml
```

---

## **Step 4: Verify HPA is Working**
Check the **HPA status**:
```sh
kubectl get hpa
```
You'll see output like this:
```
NAME         REFERENCE         TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
nodejs-hpa   Deployment/nodejs-app   30%/50%   2         5         2          5m
```
- **`30%/50%`** → Current CPU usage vs Target utilization.
- **`Replicas: 2`** → The number of running pods.

---

## **Step 5: Simulate High Load**
To test **autoscaling**, generate high CPU usage using `kubectl run`:

```sh
kubectl run -i --tty load-generator --image=busybox -- /bin/sh
```
Inside the container, run an infinite loop:
```sh
while true; do wget -q -O- http://nodejs-service.default.svc.cluster.local/; done
```
(Exit using **CTRL + C** after a few minutes.)

After a while, check if new pods were created:
```sh
kubectl get pods
```
You should see **more pods** than before due to increased CPU usage.

---

## **Step 6: Stop the Load and Watch Autoscaling Down**
Once the high CPU load stops, Kubernetes will **automatically reduce the number of pods**.

Monitor HPA:
```sh
kubectl get hpa
```
Once CPU usage is low, Kubernetes will **scale down the pods**.

---

## **Summary**
✅ **Step 1:** Install Metrics Server  
✅ **Step 2:** Define CPU/Memory Limits in Deployment  
✅ **Step 3:** Create an HPA YAML file  
✅ **Step 4:** Apply HPA and Monitor Scaling  
✅ **Step 5:** Simulate High Load to Trigger Scaling  
✅ **Step 6:** Stop Load and Watch Kubernetes Scale Down  

---

## **Next Steps**
🔹 Use **Memory-based autoscaling** instead of CPU  
🔹 Set up **Cluster Autoscaler** for dynamic node scaling  
🔹 Use **Prometheus & Grafana** to monitor metrics 

### **Setting Up Memory-Based Autoscaling (HPA) in AWS EKS**
By default, Kubernetes **Horizontal Pod Autoscaler (HPA)** scales pods **based on CPU usage**. However, you can also configure **memory-based autoscaling** by modifying the HPA configuration.

---

## **Step 1: Install the Metrics Server**
If you haven't installed the **Metrics Server**, install it first:
```sh
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

Verify that the Metrics Server is running:
```sh
kubectl get deployment -n kube-system metrics-server
```

---

## **Step 2: Define Memory Requests and Limits in Deployment**
For **HPA to scale based on memory usage**, you must specify **resource requests and limits** in your **Deployment**.

### **Modify `deployment.yaml`**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nodejs-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nodejs-app
  template:
    metadata:
      labels:
        app: nodejs-app
    spec:
      containers:
        - name: nodejs-app
          image: <aws-account-id>.dkr.ecr.us-east-1.amazonaws.com/k8s-node-app:latest
          ports:
            - containerPort: 3000
          resources:
            requests:
              cpu: "100m"
              memory: "128Mi"  # Minimum memory required
            limits:
              cpu: "500m"
              memory: "512Mi"  # Max memory before throttling
```
- **Requests** → Minimum resources the pod will always get.
- **Limits** → Maximum allowed resources before Kubernetes throttles the pod.

Apply the changes:
```sh
kubectl apply -f deployment.yaml
```

---

## **Step 3: Create Memory-Based HPA**
Now, create an **HPA rule that scales based on memory utilization**.

### **Create `hpa-memory.yaml`**
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: nodejs-hpa-memory
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nodejs-app
  minReplicas: 2
  maxReplicas: 5
  metrics:
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 75  # Scale if memory usage exceeds 75%
```

**Apply the HPA configuration:**
```sh
kubectl apply -f hpa-memory.yaml
```

Verify if the HPA is applied:
```sh
kubectl get hpa
```
Expected output:
```
NAME                REFERENCE           TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
nodejs-hpa-memory   Deployment/nodejs-app   60%/75%    2         5         2          2m
```
- **`60%/75%`** → Shows the current memory usage vs. threshold.
- **Pods will scale up when usage exceeds 75%**.

---

## **Step 4: Simulate High Memory Load**
To test **memory-based scaling**, create a container that generates **high memory usage**.

```sh
kubectl run -i --tty memory-generator --image=busybox -- /bin/sh
```

Inside the container, run this command to generate high memory load:
```sh
dd if=/dev/zero of=/dev/null bs=1M count=100000
```

---

## **Step 5: Monitor Autoscaling**
Check **HPA status**:
```sh
kubectl get hpa
```

Check if new pods are created:
```sh
kubectl get pods
```

You should see new pods running as **HPA scales up** due to high memory usage.

---

## **Step 6: Stop Load and Watch Autoscaling Down**
Once the high memory load stops, **HPA will scale down the pods** automatically.

---

## **Summary**
✅ **Step 1:** Install the Metrics Server  
✅ **Step 2:** Define memory limits in `deployment.yaml`  
✅ **Step 3:** Create an **HPA rule for memory utilization**  
✅ **Step 4:** Simulate high memory usage  
✅ **Step 5:** Monitor scaling behavior  

---

## **Next Steps**
🔹 **Use both CPU & Memory-based autoscaling together**  
🔹 **Monitor HPA with Prometheus & Grafana**  
🔹 **Set up Cluster Autoscaler to scale EC2 nodes dynamically**  