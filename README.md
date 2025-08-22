

# 🚖 Uber-Lite at Scale

A **real-time ride-hailing system** built with **microservices**, **event-driven architecture**, and **real-time matching**, designed to handle high concurrency with **scalability** and **resilience** in mind.

---

## 📊 Architecture

![Architecture Diagram](https://github.com/user-attachments/assets/ff7f95ae-2777-4cca-a7cc-baac6413432b)

**Key Components:**

* **API Gateway**: Single entry point with rate limiting and authentication
* **Microservices**: Rider, Driver, Matching, Ride, Pricing, Payment, Notification, Geo, Analytics
* **Message Bus**: Kafka for event-driven communication
* **Storage**: MySQL/PostgreSQL with PostGIS; Redis for caching
* **Real-time Communication**: WebSocket for live tracking and notifications

---

## 🔧 Microservices & Responsibilities

| Service                  | Responsibilities                                        |
| ------------------------ | ------------------------------------------------------- |
| **Rider Service**        | User profile, saved locations, ride history             |
| **Driver Service**       | Driver onboarding, vehicle info, availability status    |
| **Matching Service**     | Nearest-driver selection, dispatch logic, match retries |
| **Ride Service**         | Ride lifecycle management, saga pattern                 |
| **Pricing Service**      | Dynamic surge pricing, fare calculation                 |
| **Payment Service**      | Idempotent payment processing, double-entry ledger      |
| **Notification Service** | WebSocket push notifications, email/SMS fallbacks       |
| **Geo Service**          | Geohash-based location queries, ETA, routing            |
| **Analytics Service**    | Data processing, KPI dashboards, business intelligence  |

---

## 💾 Database & Caching Design

**Primary Databases (MySQL/PostgreSQL)**

* `rides` – ride\_id, rider\_id, driver\_id, status, pickup/dropoff, fare, created\_at
* `drivers` – driver\_id, vehicle\_info, current\_status, rating, online\_status
* `driver_locations` – driver\_id, geohash, lat/lng, updated\_at (PostGIS GiST index)
* `payments` – txn\_id, ride\_id, amount, status, idempotency\_key

**Caching (Redis)**

* Nearby drivers (TTL: 30s)
* Surge pricing zones (TTL: 1m)
* Active ride states (TTL: 1h)
* Driver locations (TTL: 15s)

---

## 🧠 Key Concepts

* **Event-Driven Architecture** – Kafka events for inter-service communication
* **CQRS Pattern** – Separate read/write models for optimized queries
* **Microservices** – Independent, scalable services with bounded contexts
* **Caching Strategy** – Redis with TTL & cache stampede prevention
* **Security** – JWT auth, token rotation, role-based access
* **Resilience Patterns** – Circuit breakers, retries, idempotency keys

---

## 🌐 API Boundaries (High-Level)

**Rider Service**

* `POST /api/riders` – Create rider profile
* `GET /api/riders/{id}/rides` – Get ride history

**Driver Service**

* `POST /api/drivers/availability` – Update driver availability
* `GET /api/drivers/{id}/earnings` – Get driver earnings

**Matching Service**

* `POST /api/matching/request` – Request ride matching
* `GET /api/matching/nearby-drivers` – Find nearby drivers

**Ride Service**

* `POST /api/rides` – Create ride request
* `PATCH /api/rides/{id}/status` – Update ride status

**Geo Service**

* `GET /api/geo/nearby-drivers` – Drivers near location
* `GET /api/geo/eta` – Calculate ETA

**Payment Service**

* `POST /api/payments/charge` – Process payment
* `POST /api/payments/refund` – Initiate refund

---

## 🚀 Getting Started

**Prerequisites**

* Docker & Docker Compose
* Java 17+ (for Spring Boot services)
* Node.js (for API Gateway)

**Setup**

```bash
# Clone repo
git clone https://github.com/your-username/uber-lite-scale.git
cd uber-lite-scale

# Start infrastructure services
docker-compose up -d zookeeper kafka mysql redis

# Setup databases
./scripts/setup-databases.sh

# Build microservices
mvn clean package -DskipTests

# Start each service (separate terminal)
java -jar rider-service/target/rider-service.jar
java -jar driver-service/target/driver-service.jar
# Repeat for other services

# Start API Gateway
cd api-gateway
npm install
npm start
```

**Test Example**

```bash
curl -X POST http://localhost:8080/api/riders \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "email": "john@example.com"}'
```

---

## 📈 Observability & DevOps

* **Monitoring**: Prometheus & Grafana dashboards
* **Logging**: ELK stack
* **Resilience**: Circuit breakers, retries, idempotency keys
* **Deployment**: Kubernetes, Helm, Terraform, GitHub Actions CI/CD

---

## ✨ Features & Wow Factors

* Live Driver Heatmap
* Smart Matching with escalating radius & retries
* Dynamic Surge Pricing
* Offline-first Driver App with local queue sync
* In-App Chat with number masking
* Admin Dashboard with live event viewer

---

## 🛠️ Technology Stack

* **Backend**: Spring Boot, Spring WebFlux, JPA
* **API Gateway**: Node.js, Express
* **Database**: MySQL/PostgreSQL with PostGIS
* **Cache**: Redis
* **Message Broker**: Apache Kafka
* **Real-time**: WebSocket, Socket.IO
* **Monitoring**: Prometheus, Grafana, ELK
* **Containerization**: Docker, Kubernetes
* **IaC**: Terraform

---

## 🤝 Contributing

1. Fork the repo
2. Create a branch: `git checkout -b feature/AmazingFeature`
3. Commit changes: `git commit -m 'Add some AmazingFeature'`
4. Push branch: `git push origin feature/AmazingFeature`
5. Open Pull Request

---

## 📝 License

MIT License – see [LICENSE](LICENSE) file for details

---


