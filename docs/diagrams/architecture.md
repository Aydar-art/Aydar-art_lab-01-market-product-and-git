# Product & Architecture Description

## Product Choice

- **Product name:** Yandex Go  
- **Link:** [https://go.yandex/](https://go.yandex/)  
- **Description:** Yandex Go is a multi-service on-demand mobility platform that allows users to order rides (including taxis and car-sharing), delivery services, and other urban transportation options via a mobile app.

## Main components

[tYandex Go Component Diagramt](src/yandex-go/architecture-component.puml)

![Yandex Go Component Diagram Code](<out/yandex-go/architecture-component/Component Diagram.svg>)
**Selected components:**

1. **Mobile App** – The user-facing application that allows customers to request rides, track drivers, and manage payments.
2. **API Gateway** – Acts as the entry point for all client requests, routing them to appropriate backend services.
3. **Order Service** – Manages ride/delivery orders, including creation, status updates, and assignment to drivers.
4. **Pricing Service** – Calculates trip costs based on distance, time, demand, and other dynamic factors.
5. **Driver Management Service** – Handles driver registration, availability, location updates, and assignment to orders.

## Data flow

![Yandex Go Sequence Diagram](<out/yandex-go/architecture-sequence/Sequence Diagram.svg>)

[Yandex Go Sequence Diagram Codet](src/yandex-go/architecture-sequence.puml)

**Chosen group:** “Order Creation Flow”

**Description:**  
When a user requests a ride, the Mobile App sends order details to the API Gateway, which forwards the request to the Order Service. The Order Service validates the request, asks the Pricing Service for a fare estimate, and then contacts the Driver Management Service to find an available driver nearby. Once a driver accepts, the Order Service confirms the order and notifies the user.

**Components interacting:**
- Mobile App → API Gateway: Sends pickup/destination details.
- API Gateway → Order Service: Forwards order request.
- Order Service → Pricing Service: Requests fare calculation.
- Order Service → Driver Management Service: Requests driver matching.

**Data exchanged:**  
User location, destination, fare estimate, driver ID, and order status.

## Deployment

![Yandex Go Deployment Diagram](<out/yandex-go/architecture-deployment/Deployment Diagram.svg>)

[Yandex Go Deployment Diagram Code](src/yandex-go/architecture-deployment.puml)

**Deployment description:**  
The system is deployed across multiple cloud regions for low latency and high availability. The Mobile App runs on users’ devices, while backend services (API Gateway, Order Service, Pricing Service, etc.) are hosted in Kubernetes clusters. Databases are replicated, and caching layers (like Redis) are used for performance. Load balancers distribute traffic among service instances.

## Assumptions

1. I assume the Pricing Service uses real-time demand data and historical patterns to calculate surge pricing.
2. I assume the Driver Management Service uses geospatial indexing to efficiently match drivers with nearby ride requests.
3. I assume the system uses message queues (like Kafka) to handle high-volume event streams such as driver location updates.

## Open questions

1. How does Yandex Go ensure data consistency between services during partial failures (e.g., when a driver cancels after assignment)?
2. What specific authentication and authorization mechanisms are used to secure communication between mobile apps and backend services?
3. How is real-time location data processed and stored to balance accuracy, performance, and privacy?