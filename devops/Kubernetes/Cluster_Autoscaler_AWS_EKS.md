# **Cluster Autoscaler for AWS EKS**
The **Kubernetes Cluster Autoscaler** automatically scales the number of worker nodes in an **AWS EKS cluster** based on demand. It ensures that:
✅ New nodes are **added** when pods are pending due to resource shortages.  
✅ Unused nodes are **removed** when pods no longer need them.  

---

## **1️⃣ How Cluster Autoscaler Works**
- When **pods cannot be scheduled** due to insufficient CPU/memory, Cluster Autoscaler **adds new nodes**.
- When **nodes are underutilized** (idle for a certain period), Cluster Autoscaler **removes** them.
- It works **independently from HPA (Horizontal Pod Autoscaler)**, which only scales pods.

📌 **Example:**  
- **Without Cluster Autoscaler:** HPA scales from 2 → 5 pods, but the nodes are full, so new pods remain **pending**.  
- **With Cluster Autoscaler:** It **adds a new node** so the new pods can run.  

---

## **2️⃣ Prerequisites**
Before setting up **Cluster Autoscaler**, ensure:
✅ You have an **EKS cluster running** with `eksctl`.  
✅ `kubectl` is connected to the EKS cluster (`kubectl get nodes` should work).  
✅ You have `IAM permissions` to modify Auto Scaling Groups.

---

## **3️⃣ Step 1: Enable Auto Scaling for Worker Nodes**
Before installing Cluster Autoscaler, **enable auto-scaling** for your worker nodes.

Run this command to **edit the Auto Scaling Group (ASG) limits**:
```sh
eksctl scale nodegroup --cluster=my-k8s-cluster --name=standard-workers --nodes-min=2 --nodes-max=5
```
- **`--nodes-min=2`** → Minimum 2 worker nodes.
- **`--nodes-max=5`** → Maximum 5 worker nodes.

✅ **Verify Scaling Configurations**:
```sh
kubectl get nodes
```
You should see 2 worker nodes initially.

---

## **4️⃣ Step 2: Deploy the Cluster Autoscaler**
### **Apply the Cluster Autoscaler Deployment**
Run:
```sh
kubectl apply -f https://github.com/kubernetes/autoscaler/releases/latest/download/cluster-autoscaler-autodiscover.yaml
```
This deploys **Cluster Autoscaler** into the `kube-system` namespace.

✅ **Verify that it is running**:
```sh
kubectl get pods -n kube-system | grep cluster-autoscaler
```

---

## **5️⃣ Step 3: Set IAM Permissions for Autoscaler**
Cluster Autoscaler needs **IAM permissions** to modify Auto Scaling Groups.

1️⃣ **Create an IAM Policy for Autoscaler**:
```sh
aws iam create-policy --policy-name EKSClusterAutoscalerPolicy --policy-document file://cluster-autoscaler-policy.json
```
Create a file `cluster-autoscaler-policy.json` with this content:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeAutoScalingInstances",
                "autoscaling:SetDesiredCapacity",
                "autoscaling:DescribeTags",
                "autoscaling:DescribeLaunchConfigurations",
                "autoscaling:UpdateAutoScalingGroup",
                "autoscaling:TerminateInstanceInAutoScalingGroup"
            ],
            "Resource": "*"
        }
    ]
}
```
2️⃣ **Attach this policy to the EKS worker node role**:
```sh
aws iam attach-role-policy --role-name <your-node-role> --policy-arn arn:aws:iam::<your-account-id>:policy/EKSClusterAutoscalerPolicy
```
- Replace `<your-node-role>` with your EKS **worker node IAM role** (`eksctl get iamidentitymapping`).
- Replace `<your-account-id>` with your AWS **account ID**.

✅ **Confirm IAM permissions**:
```sh
aws iam list-attached-role-policies --role-name <your-node-role>
```
You should see **EKSClusterAutoscalerPolicy** attached.

---

## **6️⃣ Step 4: Configure Cluster Autoscaler**
Now, we edit the **Cluster Autoscaler Deployment** to use **Auto Discovery**.

### **Edit the Deployment**
Run:
```sh
kubectl edit deployment cluster-autoscaler -n kube-system
```
Find this line:
```yaml
command:
  - ./cluster-autoscaler
```
Modify it to:
```yaml
command:
  - ./cluster-autoscaler
  - --cloud-provider=aws
  - --namespace=kube-system
  - --scale-down-enabled=true
  - --nodes=2:5:standard-workers
```
✅ **What These Flags Do?**
- `--scale-down-enabled=true` → Allows scaling down when nodes are idle.
- `--nodes=2:5:standard-workers` → Minimum **2 nodes**, maximum **5 nodes**.

Save and exit.

✅ **Restart Cluster Autoscaler**:
```sh
kubectl delete pod -n kube-system -l app.kubernetes.io/name=cluster-autoscaler
```

---

## **7️⃣ Step 5: Test Autoscaling**
To test **Cluster Autoscaler**, create a Deployment with **10 replicas**.

### **Create a High-Demand Deployment**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: load-test
spec:
  replicas: 10
  selector:
    matchLabels:
      app: load-test
  template:
    metadata:
      labels:
        app: load-test
    spec:
      containers:
        - name: busybox
          image: busybox
          command: ["sh", "-c", "while true; do echo hello; sleep 10; done"]
          resources:
            requests:
              cpu: "250m"
              memory: "256Mi"
```
**Apply the Deployment**:
```sh
kubectl apply -f load-test.yaml
```

✅ **Monitor Scaling Activity**:
```sh
kubectl get pods
kubectl get nodes
```
After a few minutes, **Cluster Autoscaler will add more nodes** to accommodate the pods.

---

## **8️⃣ Step 6: Scale Down and Verify**
To test **scaling down**, delete the Deployment:
```sh
kubectl delete deployment load-test
```
After **5-10 minutes**, Cluster Autoscaler should **remove extra nodes**.

✅ **Monitor Scaling Down**:
```sh
kubectl get nodes
```
You should see the number of nodes **decreasing**.

---

## **9️⃣ Summary**
✅ **Step 1:** Enable Auto Scaling in the Node Group  
✅ **Step 2:** Deploy Cluster Autoscaler in AWS EKS  
✅ **Step 3:** Set IAM Permissions for Cluster Autoscaler  
✅ **Step 4:** Configure Cluster Autoscaler with Auto Discovery  
✅ **Step 5:** Test Scaling Up by Creating High Load  
✅ **Step 6:** Test Scaling Down by Deleting Workloads  

---

## **🔹 Next Steps**
🔹 **Integrate Cluster Autoscaler with HPA** (Horizontal Pod Autoscaler)  
🔹 **Monitor Scaling Events using CloudWatch**  
🔹 **Use Prometheus & Grafana to track pod & node utilization**  

# **Integrate Cluster Autoscaler with HPA in AWS EKS**
To achieve **dynamic scaling**, we need to integrate **Horizontal Pod Autoscaler (HPA)** with **Cluster Autoscaler (CA)** in AWS **EKS**.

### **How They Work Together**
1️⃣ **HPA scales pods** based on CPU/memory usage.  
2️⃣ **If there are not enough nodes**, **Cluster Autoscaler scales up the worker nodes**.  
3️⃣ When load decreases, **HPA scales down pods**, and **Cluster Autoscaler removes unused nodes**.  

---

## **1️⃣ Prerequisites**
Before starting, ensure that:
✅ **Cluster Autoscaler** is set up (Check `kubectl get pods -n kube-system | grep cluster-autoscaler`).  
✅ **HPA is installed** and **Metrics Server is running** (`kubectl get deployment -n kube-system metrics-server`).  
✅ Your node group has **Auto Scaling enabled** (`eksctl scale nodegroup` used in previous steps).  

---

## **2️⃣ Step 1: Deploy a Sample Node.js Application**
Create a **Node.js Deployment** with CPU & Memory resource limits.

### **Create `deployment.yaml`**
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
              memory: "512Mi"
```

**Apply the Deployment**:
```sh
kubectl apply -f deployment.yaml
```

✅ **Verify running pods**:
```sh
kubectl get pods
```

---

## **3️⃣ Step 2: Create HPA for CPU-Based Autoscaling**
Now, create an **HPA that scales pods based on CPU utilization**.

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
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50  # Scale when CPU > 50%
```

**Apply the HPA configuration**:
```sh
kubectl apply -f hpa.yaml
```

✅ **Verify HPA is working**:
```sh
kubectl get hpa
```
You should see:
```
NAME         REFERENCE         TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
nodejs-hpa   Deployment/nodejs-app   30%/50%   2         10        2          1m
```

---

## **4️⃣ Step 3: Configure Cluster Autoscaler for Node Scaling**
To ensure nodes **scale up when pods require more resources**, modify **Cluster Autoscaler settings**.

### **Edit Cluster Autoscaler Deployment**
Run:
```sh
kubectl edit deployment cluster-autoscaler -n kube-system
```

Find this section:
```yaml
command:
  - ./cluster-autoscaler
```

Modify it to include node scaling settings:
```yaml
command:
  - ./cluster-autoscaler
  - --cloud-provider=aws
  - --namespace=kube-system
  - --scale-down-enabled=true
  - --nodes=2:5:standard-workers  # Min 2 nodes, Max 5 nodes
```
✅ **Restart Cluster Autoscaler**:
```sh
kubectl delete pod -n kube-system -l app.kubernetes.io/name=cluster-autoscaler
```

---

## **5️⃣ Step 4: Test Auto Scaling**
To trigger **autoscaling**, create a load generator.

### **Create Load Generator**
Run:
```sh
kubectl run -i --tty load-generator --image=busybox -- /bin/sh
```
Inside the pod, generate high CPU usage:
```sh
while true; do wget -q -O- http://nodejs-service.default.svc.cluster.local/; done
```

✅ **Monitor Scaling**:
```sh
kubectl get hpa
kubectl get pods
kubectl get nodes
```
- If **HPA scales to 10 pods**, Cluster Autoscaler should **add more nodes**.
- Check if **new nodes** appear.

---

## **6️⃣ Step 5: Scale Down When Load Decreases**
Once testing is done, stop the load generator:
```sh
kubectl delete pod load-generator
```

✅ **Monitor Scaling Down**:
```sh
kubectl get hpa
kubectl get nodes
```
- **HPA should reduce pods** when CPU load decreases.  
- **Cluster Autoscaler should remove unused nodes**.

---

## **7️⃣ Summary**
✅ **Step 1:** Deploy Node.js app with CPU limits  
✅ **Step 2:** Set up **HPA** for CPU-based autoscaling  
✅ **Step 3:** Configure **Cluster Autoscaler** to scale worker nodes  
✅ **Step 4:** **Simulate high load** to trigger autoscaling  
✅ **Step 5:** **Test scale-down** when the load decreases  

---

## **🔹 Next Steps**
🔹 **Monitor Autoscaling with AWS CloudWatch**  
🔹 **Use Prometheus & Grafana for better visualization**  
🔹 **Implement memory-based scaling in HPA**  