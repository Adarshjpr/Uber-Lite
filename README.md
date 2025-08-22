<img width="4537" height="1752" alt="deepseek_mermaid_20250822_9d02e2" src="https://github.com/user-attachments/assets/28ae69bb-fa63-454f-8f04-2febcd476a70" />Uber-Lite at Scale 🚖
A real-time ride-hailing system built with microservices, event-driven architecture, and real-time matching. Designed to handle high concurrency with scalability and resilience in mind.

📊 Architecture Diagram
<img width="1847" height="734" alt="image" src="https://github.com/user-attachments/assets/ff7f95ae-2777-4cca-a7cc-baac6413432b" />


Key Components:

API Gateway: Single entry point with rate limiting and authentication

Microservices: Rider, Driver, Matching, Ride, Pricing, Payment, Notification, Geo, Analytics

Message Bus: Kafka for event-driven communication

Storage: MySQL/PostgreSQL with PostGIS for primary data, Redis for caching

Real-time Communication: WebSocket for live tracking and notifications

🔧 Microservices & Responsibilities
Service	Responsibilities
Rider Service	User profile, saved locations, ride history
Driver Service	Driver onboarding, vehicle info, availability status
Matching Service	Nearest-driver selection, dispatch logic, match retries
Ride Service	Ride lifecycle management, saga pattern implementation
Pricing Service	Dynamic surge pricing, fare calculation
Payment Service	Idempotent payment processing, double-entry ledger
Notification Service	WebSocket push notifications, email/SMS fallbacks
Geo Service	Geohash-based location queries, ETA calculations, routing
Analytics Service	Data processing, KPI dashboards, business intelligence
💾 Database & Caching Design
Primary Databases (MySQL/PostgreSQL):

rides table: ride_id, rider_id, driver_id, status, pickup_location, dropoff_location, fare, created_at

drivers table: driver_id, vehicle_info, current_status, rating, online_status

driver_locations table: driver_id, geohash, latitude, longitude, updated_at (PostGIS GiST index)

payments table: txn_id, ride_id, amount, status, idempotency_key

Caching (Redis):

Nearby drivers list (TTL: 30 seconds)

Surge pricing zones (TTL: 1 minute)

Active ride states (TTL: 1 hour)

Driver locations cache (TTL: 15 seconds)

🧠 Key Concepts
Event-Driven Architecture: Kafka events for inter-service communication

CQRS Pattern: Separate read/write models for optimized queries

Microservices: Independent, scalable services with bounded contexts

Caching Strategy: Redis with TTL and cache stampede prevention

Security: JWT authentication, token rotation, role-based access

Resilience Patterns: Circuit breakers, retries, idempotency keys

🌐 API Boundaries (High-Level)
Rider Service:

POST /api/riders - Create rider profile

GET /api/riders/{id}/rides - Get ride history

Driver Service:

POST /api/drivers/availability - Update driver availability

GET /api/drivers/{id}/earnings - Get driver earnings

Matching Service:

POST /api/matching/request - Request ride matching

GET /api/matching/nearby-drivers - Find nearby drivers

Ride Service:

POST /api/rides - Create ride request

PATCH /api/rides/{id}/status - Update ride status

Geo Service:

GET /api/geo/nearby-drivers - Get drivers near location

GET /api/geo/eta - Calculate estimated arrival time

Payment Service:

POST /api/payments/charge - Process payment

POST /api/payments/refund - Initiate refund

🚀 Getting Started
Prerequisites
Docker and Docker Compose

Java 17+ (for Spring Boot services)

Node.js (for API Gateway)

Local Setup
Clone the repository

bash
git clone https://github.com/your-username/uber-lite-scale.git
cd uber-lite-scale
Start infrastructure services

bash
docker-compose up -d zookeeper kafka mysql redis
Setup databases

bash
./scripts/setup-databases.sh
Build and start services

bash
# Build all microservices
mvn clean package -DskipTests

# Start services (each in separate terminal)
java -jar rider-service/target/rider-service.jar
java -jar driver-service/target/driver-service.jar
# Repeat for other services
Start API Gateway

bash
cd api-gateway
npm install
npm start
Test the system

bash
# Create a rider
curl -X POST http://localhost:8080/api/riders \
  -H "Content-Type: application/json" \
  -d '{"name": "John Doe", "email": "john@example.com"}'
📈 Observability & DevOps
Monitoring:

Prometheus metrics collection

Grafana dashboards for service metrics

ELK stack for centralized logging

Resilience Features:

Circuit breakers for dependent services

Retry mechanisms with exponential backoff

Idempotency keys for duplicate request prevention

Deployment:

Kubernetes manifests for container orchestration

Helm charts for service deployment

GitHub Actions for CI/CD pipeline

✨ Features & Wow Factors
Live Driver Heatmap: Real-time visualization of driver locations

Smart Matching: Escalating radius search with multiple retry attempts

Dynamic Surge Pricing: Real-time pricing based on demand/supply

Offline-First Driver App: Local queue with sync capability

In-App Chat: Real-time communication with number masking

Admin Dashboard: Live event viewer and operational controls

🛠️ Technology Stack
Backend: Spring Boot, Spring WebFlux, JPA

API Gateway: Node.js, Express

Database: MySQL, PostgreSQL with PostGIS

Cache: Redis

Message Broker: Apache Kafka

Real-time: WebSocket, Socket.IO

Monitoring: Prometheus, Grafana, ELK

Containerization: Docker, Kubernetes

Infrastructure as Code: Terraform

📝 License
This project is licensed under the MIT License - see the LICENSE file for details.

🤝 Contributing
Fork the project

Create your feature branch (git checkout -b feature/AmazingFeature)

Commit your changes (git commit -m 'Add some AmazingFeature')

Push to the branch (git push origin feature/AmazingFeature)

Open a Pull Request

For detailed API documentation, check out our API Docs. For deployment guide, see Deployment Guide.
