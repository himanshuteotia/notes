### **What is Ingress in Kubernetes?**
**Ingress** is an API object in Kubernetes that **manages external access** to services within a cluster. It acts as a **reverse proxy** and **load balancer**, routing external HTTP/S traffic to the appropriate services based on defined rules.

### **Why Use Ingress?**
By default, Kubernetes services can be exposed using:
- **NodePort** (exposes service on a static port on all nodes)
- **LoadBalancer** (provisioned by cloud providers)
- **ClusterIP** (internal-only access)

However, these methods have limitations. **Ingress** provides:
✅ **Path-based Routing** – Route requests to different services based on URL paths.  
✅ **Domain-based Routing** – Use custom hostnames (e.g., `app.example.com`).  
✅ **SSL Termination** – Manage HTTPS with TLS certificates.  
✅ **Load Balancing** – Distribute traffic efficiently.  
✅ **Authentication & Security** – Enforce rules using middleware like OAuth2.  

---

### **How Ingress Works**
1. **Ingress Resource (YAML)**  
   Defines rules for routing external requests.
2. **Ingress Controller**  
   The actual implementation that processes the Ingress rules (e.g., **NGINX, Traefik, HAProxy**).

---

### **Basic Ingress Example**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```
🔹 This routes `http://myapp.example.com/` to `my-service` on port `80`.

---

### **Ingress Controllers**
To use Ingress, you must install an **Ingress Controller** in the cluster, like:
- **NGINX Ingress Controller** (most common)
- **Traefik**
- **HAProxy**
- **Istio Gateway** (for service mesh)

To install the **NGINX Ingress Controller**:
```sh
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

---

### **When to Use Ingress?**
- If you need to expose multiple services under **one domain**.
- To enable **path-based** and **host-based routing**.
- For **SSL termination** and **TLS security**.
- To **simplify service exposure** compared to using multiple LoadBalancers.

Would you like help setting up Ingress on **Minikube**? 🚀

### **Setting Up Ingress on Minikube 🚀**
Minikube does **not** come with an Ingress controller by default, so we need to **enable it** and configure it properly.

---

## **🔹 Step 1: Start Minikube**
If you haven't started Minikube yet, run:
```sh
minikube start
```
If it's already running, ensure it's updated:
```sh
minikube update
```

---

## **🔹 Step 2: Enable Ingress Addon**
Minikube has a built-in NGINX Ingress Controller that we can enable with:
```sh
minikube addons enable ingress
```
Verify it's enabled:
```sh
kubectl get pods -n kube-system
```
Look for a pod named something like `ingress-nginx-controller` in **Running** status.

---

## **🔹 Step 3: Deploy a Sample Application**
Let's create a simple deployment and service.

### **Create a Deployment**
```sh
kubectl create deployment demo-app --image=k8s.gcr.io/echoserver:1.4
```

### **Expose the Deployment as a Service**
```sh
kubectl expose deployment demo-app --port=80 --target-port=8080
```
Check if the service is running:
```sh
kubectl get svc
```

---

## **🔹 Step 4: Create an Ingress Resource**
Create a file **`ingress.yaml`**:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: demo-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: demo.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: demo-app
            port:
              number: 80
```
Apply the Ingress configuration:
```sh
kubectl apply -f ingress.yaml
```
Verify Ingress:
```sh
kubectl get ingress
```
You should see something like:
```
NAME           HOSTS       ADDRESS       PORTS   AGE
demo-ingress   demo.local  192.168.49.2  80      2m
```

---

## **🔹 Step 5: Test the Ingress**
Minikube uses a local IP, so you need to map `demo.local` to Minikube’s IP.

1️⃣ Get Minikube’s IP:
```sh
minikube ip
```
Example output:
```
192.168.49.2
```

2️⃣ Add an entry to your `/etc/hosts` file (**For Linux/macOS**):
```sh
echo "192.168.49.2 demo.local" | sudo tee -a /etc/hosts
```
For **Windows**, open `C:\Windows\System32\drivers\etc\hosts` in Notepad (as Admin) and add:
```
192.168.49.2 demo.local
```

3️⃣ Test in your browser or using `curl`:
```sh
curl http://demo.local
```
You should see an **echo server response**, confirming that Ingress is routing traffic correctly.

---

## **🎯 Done!**
Now your Ingress setup is complete! 🚀 You can:
- Add **more paths and services** to the `ingress.yaml`.
- Enable **TLS/HTTPS** using `cert-manager`.
- Deploy **multiple applications** under different hostnames.

Let me know if you need help customizing your setup! 😃

Great! Let's **customize your Ingress setup** by adding **multiple paths**, enabling **TLS/HTTPS** for secure communication, and ensuring your applications work smoothly.

---

## **🔹 Step 1: Add Multiple Paths for Different Services**
If you want to route different paths to different services, update your `ingress.yaml` file.

### **Example: Multi-Path Routing**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: multi-app-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: demo.local
    http:
      paths:
      - path: /app1
        pathType: Prefix
        backend:
          service:
            name: app1-service
            port:
              number: 80
      - path: /app2
        pathType: Prefix
        backend:
          service:
            name: app2-service
            port:
              number: 80
```
### **Steps to Apply:**
1. **Create Deployments & Services for App1 and App2**
```sh
kubectl create deployment app1 --image=k8s.gcr.io/echoserver:1.4
kubectl create deployment app2 --image=k8s.gcr.io/echoserver:1.4

kubectl expose deployment app1 --port=80 --target-port=8080 --name=app1-service
kubectl expose deployment app2 --port=80 --target-port=8080 --name=app2-service
```

2. **Apply the Updated Ingress**
```sh
kubectl apply -f ingress.yaml
```

3. **Test Multi-Path Routing**
```sh
curl http://demo.local/app1
curl http://demo.local/app2
```
You should see different responses from `app1` and `app2`!

---

## **🔹 Step 2: Enable TLS/HTTPS using Self-Signed Certificates**
To secure traffic, you need **TLS certificates**. You can create **self-signed certificates** or use **Let’s Encrypt**.

### **Option 1: Self-Signed TLS Certificate**
1. Generate TLS certificate and key:
```sh
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=demo.local/O=demo"
```

2. Create a Kubernetes **Secret**:
```sh
kubectl create secret tls demo-tls --key tls.key --cert tls.crt
```

3. **Update `ingress.yaml` to use TLS**:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: secure-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo.local
    secretName: demo-tls
  rules:
  - host: demo.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: demo-app
            port:
              number: 80
```

4. **Apply Secure Ingress**
```sh
kubectl apply -f ingress.yaml
```

5. **Test HTTPS Access**
```sh
curl -k https://demo.local
```
The `-k` flag bypasses SSL warnings for self-signed certificates.

---

## **🔹 Step 3: Use Let’s Encrypt for Real HTTPS**
To get a **trusted SSL certificate**, use `cert-manager`, which automates SSL certificate management.

1. **Install cert-manager**
```sh
kubectl apply -f https://github.com/jetstack/cert-manager/releases/latest/download/cert-manager.yaml
```
2. **Create a ClusterIssuer for Let’s Encrypt**
```yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: your-email@example.com
    privateKeySecretRef:
      name: letsencrypt-private-key
    solvers:
    - http01:
        ingress:
          class: nginx
```
Apply it:
```sh
kubectl apply -f cluster-issuer.yaml
```

3. **Modify Ingress to Use Let’s Encrypt**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: secure-ingress
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
  - hosts:
    - demo.local
    secretName: demo-tls
  rules:
  - host: demo.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: demo-app
            port:
              number: 80
```
Apply the changes:
```sh
kubectl apply -f ingress.yaml
```

Now, Kubernetes will request an **SSL certificate from Let’s Encrypt**, and your site will run on **HTTPS**.

---

## **✅ Final Checklist**
✔ Minikube running: `minikube status`  
✔ Ingress enabled: `kubectl get pods -n kube-system | grep ingress`  
✔ Services created: `kubectl get svc`  
✔ Ingress applied: `kubectl get ingress`  

---

Now, your Ingress is fully set up with **multiple paths and HTTPS support!** 🎉  

### **What are Annotations in Kubernetes?**
Annotations in Kubernetes are **key-value metadata** attached to objects like Pods, Services, Deployments, and Ingress. They store **non-identifying, descriptive, or configuration data** that Kubernetes components and external tools can use.

Unlike labels, **annotations are not used for selection or filtering** but for adding extra information.

---

### **Why Use Annotations?**
✅ Add **metadata** without affecting object identity.  
✅ Store **tool-specific information** (e.g., monitoring, logging, or ingress settings).  
✅ Configure **Ingress controllers, service meshes, or cloud integrations**.  
✅ Store **build/version details** for debugging.  

---

### **Example of Annotations**
Here’s how you can add annotations to a Pod:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  annotations:
    description: "This is a sample pod"
    monitoring: "enabled"
spec:
  containers:
  - name: nginx
    image: nginx
```
- The annotation **"description"** provides a human-readable note.
- The annotation **"monitoring"** can be used by a monitoring tool.

---

## **Annotations in Ingress**
Annotations are widely used in **Ingress** to configure behavior in **NGINX, Traefik, or HAProxy**.

### **Example: NGINX Ingress Annotations**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: demo-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  rules:
  - host: demo.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: demo-app
            port:
              number: 80
```
### **Common Ingress Annotations**
| Annotation | Description |
|------------|-------------|
| `nginx.ingress.kubernetes.io/rewrite-target: /` | Rewrites the path for backend services |
| `nginx.ingress.kubernetes.io/ssl-redirect: "true"` | Redirects HTTP to HTTPS |
| `nginx.ingress.kubernetes.io/enable-cors: "true"` | Enables CORS support |
| `nginx.ingress.kubernetes.io/proxy-body-size: "10m"` | Increases allowed request body size |

---

### **How to View Annotations of an Object**
To check annotations on a resource:
```sh
kubectl get ingress demo-ingress -o yaml
```
or
```sh
kubectl describe ingress demo-ingress
```

---

## **Key Differences: Labels vs. Annotations**
| Feature | Labels | Annotations |
|---------|--------|------------|
| Used for selection | ✅ Yes | ❌ No |
| Can be queried | ✅ Yes | ❌ No |
| Stores metadata | ✅ Limited | ✅ Extensive |
| Used in selectors (e.g., `kubectl get pods -l env=dev`) | ✅ Yes | ❌ No |
| Used by external tools | ❌ Rarely | ✅ Often |

---

## **When to Use Annotations?**
- When you need **extra metadata** for objects (e.g., documentation, version info).
- For **configuring Ingress behavior** (e.g., SSL redirects, CORS).
- For **storing tool-specific configurations** (e.g., logging, monitoring).
- When you **don’t need filtering or selection** (use labels for that).