# **Set Up Prometheus & Grafana for Kubernetes Monitoring on AWS EKS**
Monitoring **EKS (Elastic Kubernetes Service)** with **Prometheus** and **Grafana** allows real-time observability of **pod performance, cluster metrics, and autoscaling behavior**.

✅ **Step 1:** Install **Prometheus** on EKS  
✅ **Step 2:** Install **Grafana** for Dashboards  
✅ **Step 3:** Configure Grafana with Prometheus Data Source  
✅ **Step 4:** Import Kubernetes Monitoring Dashboards  
✅ **Step 5:** Set Up Alerts in Grafana  

---

## **1️⃣ Step 1: Deploy Prometheus in AWS EKS**
Prometheus collects **metrics from Kubernetes nodes, pods, and services**.

### **Create a Namespace for Monitoring**
```sh
kubectl create namespace monitoring
```

### **Deploy Prometheus**
Apply a Prometheus **ConfigMap**:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: prometheus-config
  namespace: monitoring
data:
  prometheus.yml: |
    global:
      scrape_interval: 15s
    scrape_configs:
      - job_name: 'kubernetes-nodes'
        kubernetes_sd_configs:
          - role: node
      - job_name: 'kubernetes-pods'
        kubernetes_sd_configs:
          - role: pod
```
Apply the ConfigMap:
```sh
kubectl apply -f prometheus-config.yaml
```

Deploy **Prometheus Deployment & Service**:
```sh
kubectl apply -f https://raw.githubusercontent.com/prometheus-operator/kube-prometheus/main/manifests/setup/prometheus-deployment.yaml
kubectl apply -f https://raw.githubusercontent.com/prometheus-operator/kube-prometheus/main/manifests/setup/prometheus-service.yaml
```

✅ **Verify Prometheus is Running**:
```sh
kubectl get pods -n monitoring
```
You should see:
```
prometheus-deployment-xyz  1/1 Running
```

---

## **2️⃣ Step 2: Deploy Grafana for Dashboards**
Grafana provides **visualizations for Kubernetes metrics collected by Prometheus**.

### **Deploy Grafana**
```sh
kubectl apply -f https://raw.githubusercontent.com/prometheus-operator/kube-prometheus/main/manifests/setup/grafana-deployment.yaml
kubectl apply -f https://raw.githubusercontent.com/prometheus-operator/kube-prometheus/main/manifests/setup/grafana-service.yaml
```

✅ **Expose Grafana on Port 3000**
```sh
kubectl port-forward svc/grafana -n monitoring 3000:3000
```
Now, open **http://localhost:3000** in your browser.

---

## **3️⃣ Step 3: Configure Prometheus as Grafana Data Source**
1️⃣ Log into **Grafana** at **http://localhost:3000**  
2️⃣ Username: `admin`, Password: `admin` (or set via environment variables)  
3️⃣ Click **Configuration → Data Sources → Add New Data Source**  
4️⃣ Select **Prometheus**  
5️⃣ Set **URL:** `http://prometheus-service.monitoring.svc.cluster.local:9090`  
6️⃣ Click **Save & Test**  

✅ **Grafana is now connected to Prometheus!** 🎯

---

## **4️⃣ Step 4: Import Kubernetes Monitoring Dashboards**
1️⃣ Click **Dashboards → Import**  
2️⃣ Enter the **Dashboard ID from Grafana.com**: `3119` (Kubernetes Cluster Monitoring)  
3️⃣ Click **Load**  
4️⃣ Select **Prometheus** as the Data Source  
5️⃣ Click **Import**  

📌 Now, you can monitor:
- **CPU & Memory Usage per Pod**
- **Cluster Node Utilization**
- **Network & Storage Metrics**
- **Horizontal Pod Autoscaler (HPA) Events**
- **Kubernetes Cluster Autoscaler Logs**

---

## **5️⃣ Step 5: Set Up Alerts in Grafana**
1️⃣ Go to **Alerts → Create New Alert**  
2️⃣ Select **Metric:** `container_memory_usage_bytes`  
3️⃣ Condition: `WHEN value IS ABOVE 500Mi`  
4️⃣ Action: **Send Email via SNS or Slack Webhook**  
5️⃣ Click **Save Alert**  

✅ **Grafana will notify you when memory usage exceeds the threshold!** 📩

---

## **6️⃣ Summary**
✅ **Step 1:** Install Prometheus to collect Kubernetes metrics  
✅ **Step 2:** Install Grafana for visualization  
✅ **Step 3:** Connect Prometheus as a Data Source in Grafana  
✅ **Step 4:** Import pre-built Kubernetes monitoring dashboards  
✅ **Step 5:** Create Alerts for Kubernetes metrics  

---

## **7️⃣ Next Steps**
🔹 **Set Up Loki for Kubernetes Log Monitoring in Grafana**  
🔹 **Monitor EKS Node and Cluster Autoscaling Events**  
🔹 **Use AWS Managed Prometheus & Grafana for Scaling**  

Would you like to set up **Loki for Log Monitoring in Grafana** next? 🚀

# **Set Up Loki for Kubernetes Log Monitoring in Grafana on AWS EKS**
Loki is a **lightweight, scalable logging system** for Kubernetes, designed to work with **Grafana**. Unlike ELK, it **doesn’t index logs**, making it **faster and cheaper**.

✅ **Step 1:** Deploy **Loki** in EKS  
✅ **Step 2:** Deploy **Promtail** to Collect Logs  
✅ **Step 3:** Configure **Loki as a Data Source in Grafana**  
✅ **Step 4:** View and Query Logs in Grafana  
✅ **Step 5:** Create **Alerts for Log Monitoring**  

---

## **1️⃣ Step 1: Deploy Loki in AWS EKS**
Loki stores logs **without indexing**, making it **faster & cost-effective** for Kubernetes.

### **Create a Namespace for Logging**
```sh
kubectl create namespace logging
```

### **Deploy Loki with Helm**
First, **install Helm** if you don’t have it:
```sh
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```

Then, **add the Loki Helm repository**:
```sh
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

Now, **install Loki**:
```sh
helm upgrade --install loki grafana/loki-stack --namespace=logging --set loki.persistence.enabled=false
```

✅ **Verify Loki is Running**
```sh
kubectl get pods -n logging
```
Expected Output:
```
loki-xyz123  1/1 Running
```

---

## **2️⃣ Step 2: Deploy Promtail to Collect Logs**
Promtail **collects logs** from **Kubernetes pods** and **forwards them to Loki**.

### **Deploy Promtail**
```sh
kubectl apply -f https://raw.githubusercontent.com/grafana/loki/main/production/ksonnet/promtail/promtail.yaml
```

✅ **Verify Promtail is Running**
```sh
kubectl get pods -n logging
```
You should see:
```
promtail-xyz123  1/1 Running
```

---

## **3️⃣ Step 3: Configure Loki as a Data Source in Grafana**
Now, add **Loki as a log source** in Grafana.

1️⃣ Open **Grafana** at `http://localhost:3000`  
2️⃣ Go to **Configuration → Data Sources**  
3️⃣ Click **"Add Data Source"**  
4️⃣ Select **Loki**  
5️⃣ Set **URL:** `http://loki.logging.svc.cluster.local:3100`  
6️⃣ Click **Save & Test**  

✅ **Loki is now connected to Grafana! 🎯**

---

## **4️⃣ Step 4: View and Query Logs in Grafana**
Now that logs are **collected in Loki**, you can **query them in Grafana**.

### **View Logs in Grafana**
1️⃣ Go to **Explore**  
2️⃣ Select **Loki** as the data source  
3️⃣ Run this **Query to See Logs from All Pods**:
```logql
{namespace="default"}
```
📌 This will show **all logs from all pods in the `default` namespace**.

### **Filter Logs by Pod Name**
```logql
{pod="nodejs-app-xyz"}
```

### **Filter Logs by Error Messages**
```logql
{pod=~"nodejs.*"} |= "error"
```
📌 This will show **logs containing the word "error" from all pods with "nodejs" in the name**.

---

## **5️⃣ Step 5: Create Alerts for Log Monitoring**
1️⃣ Go to **Alerts → Create New Alert**  
2️⃣ Select **Loki** as the Data Source  
3️⃣ Query:
```logql
count_over_time({namespace="default"} |= "error" [5m])
```
4️⃣ Condition: **Trigger if errors > 5 in 5 minutes**  
5️⃣ Set **Action → Send Email via SNS or Slack Webhook**  
6️⃣ Click **Save Alert**  

✅ **Grafana will notify you when error logs exceed the threshold! 📩**

---

## **6️⃣ Summary**
✅ **Step 1:** Deploy Loki in Kubernetes for log storage  
✅ **Step 2:** Deploy Promtail to collect logs  
✅ **Step 3:** Configure Loki as a Data Source in Grafana  
✅ **Step 4:** Query logs using LogQL in Grafana  
✅ **Step 5:** Create Alerts for critical logs  

---

## **7️⃣ Next Steps**
🔹 **Set Up AWS CloudWatch Logs with Loki for Hybrid Logging**  
🔹 **Use Loki for Multi-Cluster Log Aggregation**  
🔹 **Monitor EKS Network Traffic with VPC Flow Logs**  