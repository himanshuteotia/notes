Deploying your **Node.js application on AWS EKS (Elastic Kubernetes Service)** requires multiple steps. Follow this **step-by-step guide** to set up **AWS EKS, deploy your Node.js app, and expose it using a Load Balancer**.

---

## **Prerequisites**
Make sure you have the following installed:

1. **AWS CLI** – To interact with AWS  
   ```sh
   aws configure
   ```
   (Set up AWS credentials with **Access Key ID & Secret**)

2. **kubectl** – Kubernetes CLI for managing the cluster  
   ```sh
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   chmod +x kubectl
   sudo mv kubectl /usr/local/bin/
   ```

3. **eksctl** – CLI tool to create an EKS cluster  
   ```sh
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv /tmp/eksctl /usr/local/bin
   ```

4. **Docker** – To containerize your Node.js app  
   ```sh
   docker --version
   ```

---

## **Step 1: Create an AWS EKS Cluster**
Use `eksctl` to create an EKS cluster with 2 nodes:

```sh
eksctl create cluster \
  --name my-k8s-cluster \
  --region us-east-1 \
  --nodegroup-name standard-workers \
  --node-type t3.medium \
  --nodes 2
```
🔹 This creates:
- A **managed Kubernetes cluster** (`my-k8s-cluster`)
- **2 worker nodes** (`t3.medium`)
- Default **VPC, subnets, and security groups**

✅ **Verify the cluster is running**:
```sh
kubectl get nodes
```
(You should see two nodes with `Ready` status.)

---

## **Step 2: Create a Simple Node.js App**
If you haven’t already, create a directory and add the `server.js` file:

```javascript
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello from AWS EKS!");
});

app.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

---

## **Step 3: Create a Docker Image and Push to Amazon Elastic Container Registry (ECR)**
AWS **ECR (Elastic Container Registry)** is a private Docker registry.

### **Create an ECR Repository**
```sh
aws ecr create-repository --repository-name k8s-node-app
```

### **Authenticate Docker with AWS ECR**
```sh
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <aws-account-id>.dkr.ecr.us-east-1.amazonaws.com
```

### **Build & Push the Image to ECR**
```sh
docker build -t k8s-node-app .
docker tag k8s-node-app:latest <aws-account-id>.dkr.ecr.us-east-1.amazonaws.com/k8s-node-app:latest
docker push <aws-account-id>.dkr.ecr.us-east-1.amazonaws.com/k8s-node-app:latest
```

---

## **Step 4: Create Kubernetes Deployment and Service**
Now, create **YAML files** for deployment and service.

### **Deployment YAML (`deployment.yaml`)**
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
```

### **Service YAML (`service.yaml`)**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nodejs-service
spec:
  selector:
    app: nodejs-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer
```
🔹 **Type: LoadBalancer** – AWS will create an **Elastic Load Balancer (ELB)** to expose the service.

---

## **Step 5: Deploy to AWS EKS**
Apply the configurations:
```sh
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

✅ **Check if Pods are Running**:
```sh
kubectl get pods
```

✅ **Check the Service and Get the Load Balancer URL**:
```sh
kubectl get services
```
You will see output like this:
```
NAME             TYPE           CLUSTER-IP     EXTERNAL-IP        PORT(S)        AGE
nodejs-service   LoadBalancer   10.100.200.1   <your-elb-url>     80:3000/TCP    2m
```
**Copy the `EXTERNAL-IP` (your Load Balancer URL)** and open it in a browser:
```
http://<your-elb-url>
```
You should see:
```
Hello from AWS EKS!
```

---

## **Step 6: Configure a Custom Domain with Ingress (Optional)**
Instead of accessing via **ELB IP**, configure a **domain** with an **Ingress Controller**.

### **Ingress YAML (`ingress.yaml`)**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nodejs-ingress
spec:
  rules:
    - host: myapp.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: nodejs-service
                port:
                  number: 80
```

Apply the ingress:
```sh
kubectl apply -f ingress.yaml
```
Then, update your **DNS (Route 53)** to point to the Load Balancer.

---

## **Step 7: Clean Up (When You're Done)**
To **delete** all resources:
```sh
eksctl delete cluster --name my-k8s-cluster
```

---

## **Summary of Steps**
✅ **Step 1:** Create an AWS EKS cluster with `eksctl`.  
✅ **Step 2:** Build a simple Node.js app.  
✅ **Step 3:** Push the Docker image to **Amazon ECR**.  
✅ **Step 4:** Deploy the app using **Kubernetes Deployment & Service**.  
✅ **Step 5:** Expose the app via **AWS LoadBalancer**.  
✅ **Step 6 (Optional):** Use **Ingress & Custom Domain**.  
✅ **Step 7:** Clean up resources after testing.  

---

## **Next Steps**
🔹 **Set up Autoscaling (HPA) for Kubernetes**  
🔹 **Monitor Logs using AWS CloudWatch**  
🔹 **Secure with AWS IAM Roles for Service Accounts**  
