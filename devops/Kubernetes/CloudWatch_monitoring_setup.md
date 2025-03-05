# **Monitoring Kubernetes Autoscaling with AWS CloudWatch**

To monitor **Horizontal Pod Autoscaler (HPA)** and **Cluster Autoscaler (CA)** in AWS **EKS**, we will:
✅ Enable **Kubernetes Metrics in CloudWatch**  
✅ Set up **CloudWatch Container Insights**  
✅ View **HPA and Node Scaling Metrics**  
✅ Create **Alarms and Dashboards** for autoscaling events  

---

## **1️⃣ Step 1: Enable CloudWatch Metrics for EKS**
AWS **Container Insights** allows CloudWatch to collect Kubernetes **pod, node, and cluster** metrics.

### **Enable CloudWatch Logs for Your EKS Cluster**
Run:
```sh
aws eks update-cluster-config --name my-k8s-cluster --logging '{"clusterLogging":[{"types":["api", "audit", "authenticator", "controllerManager", "scheduler"],"enabled":true}]}' --region us-east-1
```
✅ This enables **API, audit, authenticator, scheduler, and controller logs** in CloudWatch.

---

## **2️⃣ Step 2: Install CloudWatch Agent on Kubernetes**
AWS provides a **CloudWatch Agent** to push Kubernetes **HPA & Node metrics** to CloudWatch.

### **Create a CloudWatch IAM Policy**
Create a policy to allow Kubernetes to push logs:
```sh
aws iam create-policy --policy-name CloudWatchAgentServerPolicy --policy-document file://cloudwatch-policy.json
```

Create a file `cloudwatch-policy.json`:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:PutLogEvents",
        "logs:DescribeLogStreams",
        "logs:DescribeLogGroups",
        "logs:CreateLogStream",
        "logs:CreateLogGroup",
        "cloudwatch:PutMetricData",
        "ec2:DescribeTags"
      ],
      "Resource": "*"
    }
  ]
}
```
Attach this policy to your **EKS worker node IAM role**:
```sh
aws iam attach-role-policy --role-name <your-node-role> --policy-arn arn:aws:iam::<your-account-id>:policy/CloudWatchAgentServerPolicy
```
✅ **Check if the IAM policy is attached**:
```sh
aws iam list-attached-role-policies --role-name <your-node-role>
```

### **Deploy the CloudWatch Agent to EKS**
Create a namespace:
```sh
kubectl create namespace amazon-cloudwatch
```
Deploy the **CloudWatch Agent**:
```sh
kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-yaml-templates/cwagent-daemonset.yaml
```
✅ Verify if the CloudWatch Agent is running:
```sh
kubectl get pods -n amazon-cloudwatch
```
You should see something like:
```
NAME                             READY   STATUS    RESTARTS   AGE
cloudwatch-agent-xyz123          1/1     Running   0          2m
```

---

## **3️⃣ Step 3: Enable HPA Metrics in CloudWatch**
Now, enable **Kubernetes Pod Autoscaling Metrics**.

### **Deploy CloudWatch Metrics Config**
Create a ConfigMap:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cwagent-config
  namespace: amazon-cloudwatch
data:
  cwagentconfig.json: |
    {
      "metrics": {
        "namespace": "ContainerInsights",
        "metrics_collected": {
          "kubernetes": {
            "cluster_name": "my-k8s-cluster",
            "metrics_collection_interval": 60
          }
        }
      }
    }
```
Apply it:
```sh
kubectl apply -f cwagent-config.yaml
```
✅ **Restart the CloudWatch Agent**:
```sh
kubectl delete pod -n amazon-cloudwatch -l name=cloudwatch-agent
```

---

## **4️⃣ Step 4: View Kubernetes Metrics in CloudWatch**
Now, go to **AWS CloudWatch → Metrics → Container Insights**.

### **Available Metrics**
- **Node Scaling Metrics**:
  - `node_cpu_utilization`
  - `node_memory_utilization`
  - `node_filesystem_utilization`

- **Pod Scaling (HPA) Metrics**:
  - `pod_cpu_utilization`
  - `pod_memory_utilization`
  - `pod_network_tx_bytes`

✅ **Verify HPA Scaling Metrics**:
```sh
kubectl get hpa
```
Expected Output:
```
NAME         REFERENCE         TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
nodejs-hpa   Deployment/nodejs-app   60%/50%   2         10        5          5m
```
➡ CloudWatch will now **track pod replicas based on CPU/memory usage**.

---

## **5️⃣ Step 5: Create CloudWatch Dashboards for Autoscaling**
To visualize autoscaling in CloudWatch:

### **Create a Custom Dashboard**
1️⃣ **Go to AWS CloudWatch** → **Dashboards**  
2️⃣ **Create a New Dashboard** (e.g., `EKS-Autoscaling`)  
3️⃣ **Add the Following Widgets**:
   - **Line Chart: `ClusterAutoscaler.NodeGroup.DesiredCapacity`**
   - **Line Chart: `ClusterAutoscaler.NodeGroup.RunningCapacity`**
   - **Bar Chart: `ClusterAutoscaler.NodeGroup.UnneededCapacity`**
   - **CPU/Memory Utilization for Pods**
   - **HPA Metrics: `pod_cpu_utilization`, `pod_memory_utilization`**

Now, you can monitor **HPA & Cluster Autoscaler scaling activity** in real time! 🎯

---

## **6️⃣ Step 6: Set CloudWatch Alarms for Scaling Events**
Set up **alerts** for when Kubernetes scales too aggressively.

### **Create an Alarm for High CPU Utilization**
1️⃣ Go to **AWS CloudWatch → Alarms**  
2️⃣ Click **Create Alarm**  
3️⃣ Select **Metric → Container Insights → pod_cpu_utilization**  
4️⃣ Set a threshold (e.g., **80% CPU Usage**)  
5️⃣ Select **Action → SNS Notification → Email Alert**  
6️⃣ Click **Create Alarm**  

➡ This will **alert you** when pod CPU usage exceeds 80%, so you can monitor HPA scaling.

---

## **7️⃣ Summary**
✅ **Step 1:** Enable CloudWatch Logs for EKS  
✅ **Step 2:** Install CloudWatch Agent in Kubernetes  
✅ **Step 3:** Enable HPA Metrics Collection  
✅ **Step 4:** View EKS Cluster Metrics in CloudWatch  
✅ **Step 5:** Create CloudWatch Dashboards for Autoscaling  
✅ **Step 6:** Set CloudWatch Alarms for High CPU Utilization  

---

## **🔹 Next Steps**
🔹 **Monitor logs in AWS CloudWatch Logs**  
🔹 **Set up Prometheus & Grafana for advanced monitoring**  
🔹 **Use AWS X-Ray for tracing EKS services**  


# **Monitor Logs in AWS CloudWatch Logs for EKS**
To effectively **monitor logs** in AWS **CloudWatch Logs** for your **EKS cluster**, follow these steps:

✅ **Step 1:** Enable **EKS Logging to CloudWatch**  
✅ **Step 2:** Deploy **Fluent Bit** to send logs to CloudWatch  
✅ **Step 3:** Verify logs in **CloudWatch Log Groups**  
✅ **Step 4:** Set up **Log Insights for Analysis**  
✅ **Step 5:** Create **CloudWatch Alarms for Log Monitoring**  

---

## **1️⃣ Step 1: Enable EKS Logging in CloudWatch**
AWS allows **EKS control plane logs** to be sent to **CloudWatch Logs**.

Run the following command to enable logging:
```sh
aws eks update-cluster-config --name my-k8s-cluster \
  --logging '{"clusterLogging":[{"types":["api", "audit", "authenticator", "controllerManager", "scheduler"],"enabled":true}]}' \
  --region us-east-1
```
📌 **Control Plane Logs Available in CloudWatch:**
- **API Server Logs** → Requests made to Kubernetes API
- **Audit Logs** → Security and authentication logs
- **Scheduler Logs** → Information on scheduling decisions
- **Controller Logs** → Logs related to controllers running in Kubernetes

✅ **Verify Logging is Enabled**:
```sh
aws eks describe-cluster --name my-k8s-cluster --query "cluster.logging"
```

---

## **2️⃣ Step 2: Deploy Fluent Bit to Collect Application Logs**
By default, **EKS logs control plane events**, but **application logs (Pods, Services, and Events)** must be manually forwarded.

### **Why Use Fluent Bit?**
Fluent Bit is a lightweight log processor that **sends Kubernetes logs to CloudWatch**.

### **Create IAM Policy for Fluent Bit**
Create a new **IAM policy**:
```sh
aws iam create-policy --policy-name FluentBitPolicy --policy-document file://fluentbit-policy.json
```
Create `fluentbit-policy.json`:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams"
            ],
            "Resource": "*"
        }
    ]
}
```
Attach this policy to the EKS worker node role:
```sh
aws iam attach-role-policy --role-name <your-node-role> --policy-arn arn:aws:iam::<your-account-id>:policy/FluentBitPolicy
```

### **Deploy Fluent Bit**
1️⃣ Create a **namespace** for Fluent Bit:
```sh
kubectl create namespace amazon-cloudwatch
```

2️⃣ **Deploy Fluent Bit ConfigMap:**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: fluent-bit-config
  namespace: amazon-cloudwatch
data:
  fluent-bit.conf: |
    [SERVICE]
        Flush         1
        Daemon        Off
        Log_Level     info
    [INPUT]
        Name          tail
        Path          /var/log/containers/*.log
        Parser        docker
        Tag           kube.*
    [OUTPUT]
        Name          cloudwatch
        Match         kube.*
        region        us-east-1
        log_group_name fluent-bit-logs
        auto_create_group true
        log_stream_name_from_tag true
```
Apply the ConfigMap:
```sh
kubectl apply -f fluentbit-config.yaml
```

3️⃣ **Deploy Fluent Bit DaemonSet:**
```sh
kubectl apply -f https://raw.githubusercontent.com/aws-samples/amazon-cloudwatch-container-insights/master/k8s-yaml-templates/fluent-bit-daemonset.yaml
```

✅ **Check if Fluent Bit is running:**
```sh
kubectl get pods -n amazon-cloudwatch
```
You should see:
```
fluent-bit-abc123   1/1 Running
```

---

## **3️⃣ Step 3: Verify Logs in CloudWatch**
Now that **Fluent Bit is running**, check if logs are being sent.

### **View Logs in AWS CloudWatch**
1️⃣ **Go to AWS Console → CloudWatch**  
2️⃣ Click **Logs → Log Groups**  
3️⃣ Look for **fluent-bit-logs**  
4️⃣ Click on a log stream to view **real-time Kubernetes logs**.

---

## **4️⃣ Step 4: Use CloudWatch Logs Insights for Analysis**
To **search & filter logs**, use **CloudWatch Logs Insights**.

### **Example Queries**
📌 **Find all logs from a specific pod:**
```sql
fields @timestamp, @message
| filter kubernetes.pod_name = "nodejs-app-xyz"
| sort @timestamp desc
```
📌 **Find logs containing errors:**
```sql
fields @timestamp, @message
| filter @message like /error/
| sort @timestamp desc
```

📌 **Count errors by pod:**
```sql
fields kubernetes.pod_name, count(*) as error_count
| filter @message like /error/
| stats count(*) by kubernetes.pod_name
```
These queries help **debug application errors faster**.

---

## **5️⃣ Step 5: Create CloudWatch Alarms for Log Monitoring**
To **get alerts** when errors occur in logs:

### **Create a Log Filter**
1️⃣ Go to **AWS CloudWatch → Logs → Log Groups**  
2️⃣ Select **fluent-bit-logs**  
3️⃣ Click **Create Metric Filter**  
4️⃣ Enter **Filter Pattern:**
```sh
?error ?failed ?timeout
```
5️⃣ Click **Create Filter**  
6️⃣ Set a metric name like `ErrorCountMetric`  

### **Create an Alarm**
1️⃣ Go to **AWS CloudWatch → Alarms**  
2️⃣ Click **Create Alarm**  
3️⃣ Select the **ErrorCountMetric** created above  
4️⃣ Set the threshold (e.g., **trigger if errors > 5 in 5 minutes**)  
5️⃣ Set **Notification** to **Send Email via SNS**  
6️⃣ Click **Create Alarm**  

✅ **You will now receive email alerts if errors occur in Kubernetes logs!** 📩

---

## **6️⃣ Summary**
✅ **Step 1:** Enable Kubernetes logging in CloudWatch  
✅ **Step 2:** Deploy Fluent Bit to collect application logs  
✅ **Step 3:** View logs in CloudWatch Log Groups  
✅ **Step 4:** Use Log Insights to analyze logs  
✅ **Step 5:** Set CloudWatch Alarms for error monitoring  

---

## **7️⃣ Next Steps**
🔹 **Use Prometheus & Grafana for real-time monitoring**  
🔹 **Set up AWS X-Ray for tracing EKS service requests**  
🔹 **Enable VPC Flow Logs to analyze EKS network traffic**  