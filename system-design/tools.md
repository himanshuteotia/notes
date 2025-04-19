Here's a clear and organized list of **distributed system tools** and what each is used for. This will help you simulate real AWS/cloud-like deployments **locally** and understand their **production use** too.

---

## ✅ Tool List: "What I Use and Why"

| 🔧 **Tool** | 📌 **Purpose** | 💡 **Used For** |
|------------|----------------|------------------|
| **LocalStack** | AWS Service Emulator | Run AWS (S3, Lambda, API Gateway, DynamoDB, SQS, SNS) locally |
| **Docker** | Containerization | Run each microservice, DB, message broker as isolated service |
| **Docker Compose** | Service Orchestration | Start multiple services together (e.g., Node.js app + Redis + MongoDB) |
| **Minikube / Kind** | Local Kubernetes | Run a real Kubernetes cluster locally for testing deployments |
| **Terraform** | Infrastructure as Code | Deploy AWS or LocalStack infra using declarative `.tf` files |
| **K6 / Artillery** | Load Testing | Simulate traffic to test performance (like 1000 requests/sec) |
| **MongoDB (Docker)** | NoSQL Database | Store JSON documents; can be used with sharding/replication |
| **Redis (Docker)** | In-memory Cache | Caching, pub-sub, queues |
| **RabbitMQ / Kafka** | Message Broker | For asynchronous communication in microservices |
| **MinIO** | Local S3 Alternative | Test file uploads like S3 buckets |
| **PostgreSQL / MySQL (Docker)** | RDBMS | Use for SQL-based storage (with replicas) |
| **Prometheus + Grafana** | Monitoring | Track CPU, memory, API calls, latency, etc. with graphs |
| **ELK Stack (Elasticsearch + Logstash + Kibana)** | Logging | View logs, search errors, filter traces from services |
| **Jaeger / Zipkin** | Distributed Tracing | Trace the flow of requests across microservices |
| **Toxiproxy** | Fault Injection | Simulate network issues (latency, drop packets) for testing |
| **AWS CLI / awslocal** | Command Line for AWS / LocalStack | Run CLI commands like `aws s3 ls` or `awslocal lambda invoke` |
| **GitHub Actions + `act` CLI** | CI/CD on Local | Run GitHub Actions workflow locally to test deployments |

---

### ✅ Example Use Case: Full Local AWS-Like Setup

**Goal**: Run a Node.js microservice system with queue, caching, file uploads, and monitoring.

| Component | Tool |
|----------|------|
| App Servers | Node.js in Docker |
| Messaging | RabbitMQ (Docker) or `awslocal sqs` |
| Caching | Redis (Docker) |
| File Storage | MinIO or `awslocal s3` |
| Monitoring | Prometheus + Grafana |
| Logs | Kibana (via ELK) |
| Tracing | Jaeger |
| Infra Code | Terraform (using LocalStack provider) |
