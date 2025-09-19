# 🌊 Flow: Graph-Based ML Orchestrator

## Overview

This project is a **Graph-based Machine Learning Orchestrator** designed to manage, schedule, and track ML workflows. These workflows are structured as graphs, with the system using **Neo4j** for metadata storage and **Redis** for caching and state management. The backend is a **Spring Boot** application, the frontend is built with **React**, and the entire system is monitored using **Prometheus** and **Grafana**.

---

##  System Architecture

* **Backend**: A **Spring Boot** application that handles workflow orchestration, database interaction, caching, and exposes REST APIs.
* **Frontend**: A **React** application (managed with `pnpm`) for visualizing workflows, task statuses, and interacting with the orchestrator.
* **Infrastructure**: **Docker-managed services** including Neo4j, Redis, Prometheus, Grafana, and an optional PostgreSQL instance.

---

##  Development Environment Setup

The development environment is managed by a **Nix flake** to ensure reproducible versions of all necessary tools.

1.  Enter the development shell:
    `nix develop`
2.  Verify the tools are correctly installed:
    * `java -version`
    * `mvn -v`
    * `pnpm -v`
    * `docker --version`
    * `docker-compose --version`

---

##  Backend Configuration & Execution

### Configuration

Ensure the following are correctly set in `application.properties` or `application.yml`:

* **Neo4j** URI, username, and password.
* **Redis** host and port.
* **Postgres** configuration (optional).
* **Actuator endpoints** for health and Prometheus metrics must be enabled.

### Execution

* **Maven**: `mvn spring-boot:run`
* **Gradle**: `./gradlew bootRun`
* **Verify Endpoints**: Check that `/actuator/health` and `/actuator/prometheus` are accessible.

---

##  Frontend Execution

1.  Navigate to the `frontend/` directory.
2.  Install dependencies: `pnpm install`
3.  Start the development server: `pnpm dev`
4.  The application will be available at `http://localhost:5173`.

---

## 🐳 Docker Services

The following services are managed by Docker Compose for the development environment.

* **Neo4j**: Stores the workflow graph metadata.
* **Redis**: Caches ephemeral workflow states and supports message queues.
* **Postgres** (optional): Provides persistent relational storage.
* **Prometheus**: Scrapes metrics from the Spring Boot backend and other infrastructure services.
* **Grafana** (optional): Visualizes metrics and workflow execution dashboards.

To start all services, run:
`docker-compose -f docker-compose.dev.yml up -d`

**Note**: Always check container health and logs to ensure all services are running correctly before starting the backend.

---

##  Core Concepts

* **Workflows as Graphs**: Workflows are modeled as directed graphs in **Neo4j**. Nodes represent tasks (e.g., data preprocessing, model training), and edges define dependencies and execution order.
* **Asynchronous Orchestration**: The backend orchestrator schedules tasks asynchronously, maintaining their real-time state in **Redis**.
* **Comprehensive Monitoring**: Metrics like task execution time, queue length, Redis cache hits, and Neo4j query performance are exposed via the Spring Boot Actuator, collected by **Prometheus**, and visualized in **Grafana**.
* **Interactive UI**: The frontend allows users to submit new workflows, monitor execution in real-time, and visualize the graph structure and the status of each task.

This design enables flexible orchestration of complex ML workflows while keeping task dependencies explicit and easily queryable.

---

##  Recommended Development Workflow

1.  Start all required infrastructure services: `docker-compose -f docker-compose.dev.yml up -d`
2.  Launch the Spring Boot backend using Maven or Gradle.
3.  Start the React frontend: `pnpm dev`
4.  Access the different components at their default URLs:
    * **Backend**: `http://localhost:8080`
    * **Frontend**: `http://localhost:5173`
    * **Prometheus**: `http://localhost:9090`
    * **Grafana**: `http://localhost:3000`
5.  Verify the system's health by checking the `/actuator/health` endpoint. All connected components (Neo4j, Redis, etc.) should report a status of `UP`.

---

##  Notes

* Ensure that the passwords and ports configured for **Neo4j** and **Redis** in your backend application match the ones in your `docker-compose` file.
* For large-scale or distributed orchestration, consider integrating a dedicated message broker like **Spring Cloud Stream** or **Kafka** for dispatching workflow tasks.
* The entire toolchain is made reproducible via the **Nix flake**, ensuring a consistent development environment across different machines.
