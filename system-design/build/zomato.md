# 🍽️ **Zomato System Design (Food Delivery Platform)**

Designing a system like **Zomato** involves building a highly scalable and efficient platform that handles **real-time food ordering**, **restaurant discovery**, **order tracking**, and **user management**. Zomato acts as a bridge between **restaurants**, **delivery agents**, and **customers**, making it a complex yet interesting system to design.

---

## 🎯 **Core Functional Requirements:**

### ✅ **Customer-Facing Features:**
- Restaurant discovery based on location, cuisine, and ratings.
- Food ordering & cart management.
- Real-time order tracking (GPS-based).
- Secure payments (multiple gateways).
- Reviews & ratings for restaurants and delivery agents.

### ✅ **Restaurant-Side Features:**
- Menu management.
- Order notifications & status updates.
- Analytics for sales, order volume, etc.

### ✅ **Delivery Agent Features:**
- Real-time order assignment.
- Route optimization for deliveries.
- Order pick-up and drop-off status updates.

---

## ❌ **Non-Functional Requirements:**
- **Scalability** → Handle millions of users, restaurants, and delivery partners.
- **High Availability** → 99.99% uptime.
- **Low Latency** → Especially for order tracking & delivery.
- **Data Consistency** → For orders, payments, and user data.
- **Security & Compliance** → Secure payment gateways (PCI-DSS compliance).

---

## 📊 **High-Level Architecture Overview:**

```
                           +-------------------------+
                           |    Load Balancer (ALB)  |
                           +-----------+-------------+
                                       |
                        +--------------v-------------+
                        |        API Gateway         |
                        +--------------+-------------+
                                       |
     +----------------------+----------------------+------------------+
     |                      |                      |                  |
+----v-----+          +-----v------+         +-----v-----+       +----v-----+
| User     |          | Order      |         | Restaurant|       | Delivery |
| Service  |          | Service    |         | Service   |       | Service  |
+----+-----+          +-----+------+         +-----+-----+       +----+-----+
     |                      |                      |                  |
     |                      |                      |                  |
+----v-----+         +------v-----+         +------v-----+      +-----v-----+
| User DB  |         | Order DB   |         | Menu DB    |      | Delivery  |
| (MySQL)  |         | (Postgres) |         | (MongoDB)  |      | DB (Redis)|
+----------+         +------------+         +------------+      +-----------+

                  +-------------------------------------------+
                  |            Kafka (Event Bus)              |
                  +-------------------------------------------+

      +----------------------+                 +----------------------+
      | Payment Service      |                 | Notification Service |
      | (Stripe/Razorpay)    |                 | (Push/SMS/Email)     |
      +----------------------+                 +----------------------+

                  +-------------------------------------------+
                  |      Analytics & ML (for Recommendations) |
                  +-------------------------------------------+
```

---

# 🏆 **1. Core Components Explained:**

## ✅ **1.1 API Gateway**
- Handles all incoming API requests from web/mobile.
- Performs rate-limiting, authentication, and load balancing.
- Services communicate internally via **gRPC** or **REST APIs**.

---

## ✅ **1.2 User Service**
- Handles user registration, login, and profile management.
- Supports **JWT tokens** for authentication.
- Stores data in **MySQL/PostgreSQL**.

---

## ✅ **1.3 Restaurant Service**
- Manages restaurant profiles, menus, and availability.
- **MongoDB** for flexible menu structures (nested categories, items).
- Supports geo-location-based searches using **ElasticSearch**.

---

## ✅ **1.4 Order Service**
- Core service for managing order placement, status updates, and order history.
- Uses **PostgreSQL** (ACID-compliant) for order consistency.
- Coordinates with:
  - **Payment Service** for transactions.
  - **Delivery Service** for assigning delivery agents.

---

## ✅ **1.5 Delivery Service**
- Tracks delivery agents in real-time using **GPS**.
- Uses **Redis** for quick geo-queries and location tracking.
- Uses a **matching algorithm** to assign the nearest delivery agent.

---

## ✅ **1.6 Payment Service**
- Integrates with **Stripe, Razorpay**, or other gateways.
- Handles pre-authorization and payment capture post-delivery.
- Ensures PCI-DSS compliance.

---

## ✅ **1.7 Notification Service**
- Manages push notifications, emails, and SMS updates.
- Uses services like **AWS SNS** or **Firebase Cloud Messaging (FCM)**.

---

# 📡 **2. Data Flow (Order Placement to Delivery):**

1. **Customer selects a restaurant and places an order.**
2. **API Gateway** routes the request to the **Order Service**.
3. **Payment Service** pre-authorizes the payment.
4. **Order Service** writes the order into the **Order DB** and triggers events via **Kafka**.
5. **Delivery Service** picks up the event and assigns a delivery agent based on proximity.
6. **Restaurant Service** notifies the restaurant to start preparing the food.
7. Once the food is picked up, **real-time GPS tracking** starts.
8. On delivery completion, the payment is captured, and the customer is notified.

---

# 📊 **3. Databases Used:**

| **Data Type**               | **Database**             | **Reason**                                  |
|-----------------------------|--------------------------|---------------------------------------------|
| User Data                    | MySQL/PostgreSQL         | ACID compliance for user credentials       |
| Restaurant Menu & Details    | MongoDB                  | Flexible schema for menu items             |
| Orders                       | PostgreSQL               | Strong consistency for transactions        |
| Real-Time Locations          | Redis                    | Fast reads/writes for GPS data             |
| Search & Recommendations     | ElasticSearch            | Full-text and geo-spatial search           |
| Analytics                    | Redshift/BigQuery        | Large-scale data analysis                  |

---

# 🤖 **4. Algorithms Involved:**

### 📍 **4.1 Nearest Delivery Agent Algorithm**
- Uses **Haversine formula** or **GeoHash** for calculating distances.
- Delivery agents' live locations stored in **Redis** (as geo-indexed data).

### ⭐ **4.2 Recommendation System**
- Collaborative filtering for personalized restaurant suggestions.
- Content-based filtering based on cuisine preferences and order history.
- Trending items identified using real-time data streams.

### 🔄 **4.3 Dynamic Pricing Algorithm**
- Surge pricing for delivery fees based on demand, weather, or time.

---

# ⚡ **5. Scaling Strategy:**

| **Component**               | **Scaling Method**                          |
|-----------------------------|---------------------------------------------|
| API Gateway                  | Auto-scale using AWS ALB                   |
| Kafka (Event Broker)          | Scale brokers and partitions               |
| Order Service                 | Horizontal scaling with stateless nodes    |
| Redis (Location Tracking)     | Use Redis Cluster for scalability          |
| Elasticsearch (Search)        | Shard indexes for high query throughput    |
| Data Warehouses               | Use Redshift or BigQuery for analytics     |
| Machine Learning Models       | Deploy with AWS SageMaker or TensorFlow    |

---

# 🧠 **6. Challenges & Solutions:**

| **Challenge**                        | **Solution**                               |
|--------------------------------------|--------------------------------------------|
| Real-Time Order Tracking             | Use Redis with TTL-based geo-queries       |
| Hotspot Issue (High Demand Areas)    | Load balance using regional Kafka brokers  |
| High Latency During Peak Hours       | Implement autoscaling & request throttling |
| Data Consistency (Orders & Payments) | Use two-phase commit or eventual consistency|
| Cold Start Problem in Recommendations| Use content-based filtering as fallback    |

---

# 🔥 **7. Tech Stack:**

| **Layer**          | **Technology**                            |
|--------------------|------------------------------------------|
| Frontend           | React.js / Flutter (for Mobile)           |
| Backend            | Node.js / Java Spring Boot                |
| API Gateway        | AWS API Gateway / NGINX                   |
| Real-Time Messaging| Kafka / RabbitMQ                          |
| Database           | PostgreSQL, MongoDB, Redis                |
| Search Engine      | ElasticSearch                             |
| Cloud Services     | AWS (EC2, S3, Lambda, RDS, SNS, SQS)      |
| Monitoring & Logs  | Prometheus, Grafana, ELK Stack            |

---

# 🎉 **Final Thoughts:**

Designing a system like **Zomato** requires integrating multiple complex components such as **real-time tracking**, **recommendation systems**, and **secure payment handling**. Scalability and reliability are critical, especially during peak ordering hours.