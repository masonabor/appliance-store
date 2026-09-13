# Appliance Store

## Project Description
Appliance Store is a full-stack e-commerce application designed for managing and selling home appliances online. It allows clients to browse products and place orders, while employees and administrators can manage the catalog, oversee orders, and manage users. The application also includes an asynchronous notification system to keep users updated via email on their order statuses.

## General Architecture
The project is designed using a **modular, event-driven architecture** consisting of the following core components:

* **Backend API (`appliance-store-backend`)**: 
  The core of the system is built with Java and Spring Boot. It follows a classic three-tier architecture ensuring a clear separation of concerns:
  * **Controllers** handle HTTP requests, route definitions, input validation, and internationalization (i18n).
  * **Services** contain the core business logic (e.g., processing orders, calculating totals).
  * **Repositories** interact with the PostgreSQL database using Spring Data JPA and Hibernate.
  * **Security Layer** is implemented using Spring Security with OAuth2. It secures the REST endpoints using JWT tokens and enforces Role-Based Access Control (RBAC) for Clients, Employees, and Admins.

* **Frontend Web App (`appliance-store-frontend`)**: 
  A React-based Single Page Application (SPA). It communicates with the backend via REST APIs. The UI is designed to provide role-specific views (e.g., an Employee dashboard vs. a Client storefront).

* **Event-Driven Notification Service (`appliance-notification`)**: 
  To decouple user communication from the core business logic, the application uses an event-driven approach. When a significant action occurs (e.g., an order is approved), the backend publishes an event to an Apache Kafka topic. The independent Notification Service consumes these events and dispatches asynchronous emails via SMTP, preventing the main backend from being blocked by email processing.

* **Database & Migrations**: 
  The system uses PostgreSQL as the primary relational database. Database schemas are versioned and managed using Flyway, ensuring that the database state is consistent and reproducible across all environments.

## Technologies Used

### Backend & Core Logic
* **Java 17 & Spring Boot 3**: Provides the foundational framework, Dependency Injection, and embedded web server.
* **Spring Data JPA & Hibernate**: ORM framework for mapping Java objects to database tables securely and efficiently.
* **Spring Security (OAuth2/JWT)**: For authentication, authorization, and securing API endpoints.
* **MapStruct**: Generates type-safe mapping code between Data Transfer Objects (DTOs) and internal Domain Entities, keeping the API layer decoupled from the database layer.
* **Lombok**: Reduces boilerplate code (getters, setters, constructors) through annotations, keeping classes clean.
* **Flyway**: Database migration tool for managing SQL schema changes.

### Frontend
* **React 19**: A modern JavaScript library for building interactive, component-based user interfaces.
* **JavaScript (ES6+) & CSS**: Core web technologies used for styling and client-side logic.
* **Cypress & React Testing Library**: For writing End-to-End (E2E) and UI component tests.

### Messaging & Integration
* **Apache Kafka**: A distributed streaming platform used as a message broker to facilitate highly reliable, asynchronous communication between the Backend and Notification Service.
* **JavaMailSender (SMTP)**: Used by the Notification Service to format and send real emails to clients.

### Infrastructure & Testing
* **PostgreSQL**: The robust, production-ready relational database storing all persistent data.
* **H2 Database**: An in-memory database used for fast execution of unit tests.
* **Testcontainers**: A Java library that provides lightweight, throwaway instances of common databases (like PostgreSQL) in Docker containers for reliable integration testing.
* **Docker & Docker Compose**: Used to containerize the application and orchestrate the entire environment (Frontend, Backend, DB, Kafka, Zookeeper) with a single command.
* **Swagger (SpringDoc OpenAPI)**: Automatically generates interactive API documentation for backend endpoints.

## How to Run via Docker
The easiest way to start the entire infrastructure (Frontend, Backend, Notification Service, Database, and Kafka) is by using Docker Compose.

1. Ensure you have **Docker** and **Docker Compose** installed on your machine.
2. Open your terminal and navigate to the root directory of the project (where the `docker-compose.yaml` file is located).
3. Run the following command to build and start the containers in detached mode:
   ```bash
   docker-compose up --build -d
   ```
4. Once the containers are successfully built and running, the services will be available at:
   * **Frontend Application:** [http://localhost:3000](http://localhost:3000)
   * **Backend API:** [http://localhost:8080](http://localhost:8080) (Swagger UI available at `http://localhost:8080/swagger-ui.html`)
   * **Kafka UI:** [http://localhost:8081](http://localhost:8081)

To stop and remove the running containers, run:
```bash
docker-compose down
```
