---

Flow: Graph-Based ML Orchestrator

Overview:
This project implements a Graph-based Machine Learning Orchestrator. Its primary function is to manage, schedule, and track machine learning workflows represented as graph structures. The system leverages Neo4j for workflow metadata storage and Redis for caching and ephemeral state management. The backend is implemented using Spring Boot, exposing REST APIs for orchestrator operations, while the frontend is a React application managed via pnpm. Monitoring is handled using Prometheus, with optional Grafana dashboards for visualization.

System Architecture:

* Backend: Spring Boot application responsible for workflow orchestration, database interaction, caching, and exposing API endpoints.
* Frontend: React application for visualizing workflows, task status, and interacting with the orchestrator.
* Infrastructure: Docker-managed services including Neo4j, Redis, optional PostgreSQL, Prometheus, and Grafana.

Development Environment Setup:
The development environment is provided via a Nix flake for reproducible versions of Java, Node.js, pnpm, Docker, and auxiliary tools.

Steps:

1. Enter the dev shell: `nix develop`
2. Verify tools: `java -version`, `mvn -v`, `pnpm -v`, `docker --version`, `docker-compose --version`

Backend Configuration:

* Neo4j URI, username, and password must be specified in `application.properties` or `application.yml`.
* Redis host and port must be configured.
* Optional Postgres configuration can be included for relational storage.
* Actuator endpoints for health and Prometheus metrics must be enabled.

Backend Execution:

* Maven: `mvn spring-boot:run`
* Gradle: `./gradlew bootRun`
* Verify endpoints: `/actuator/health` and `/actuator/prometheus`

Frontend Execution:

* Navigate to `frontend/` and install dependencies: `pnpm install`
* Start development server: `pnpm dev`
* Default URL: `http://localhost:5173`

Docker Services:

* Neo4j: stores workflow graph metadata
* Redis: caches ephemeral workflow state and supports queues
* Postgres (optional): persistent relational storage
* Prometheus: collects metrics from Spring Boot and infrastructure services
* Grafana (optional): visualizes metrics and workflow execution

Start services: `docker-compose -f docker-compose.dev.yml up -d`
Check container health and logs to ensure services are running before starting the backend.

Flow: Graph-Based ML Orchestrator:

* Workflows are represented as directed graphs in Neo4j. Nodes correspond to tasks such as data preprocessing, model training, or evaluation. Edges represent dependencies or execution order between tasks.
* The backend orchestrator schedules tasks asynchronously and maintains state in Redis.
* Metrics including task execution, queue length, Redis hits, and Neo4j query performance are exposed via Spring Boot Actuator, scraped by Prometheus, and visualized in Grafana.
* The frontend allows users to submit new workflows, monitor real-time execution, visualize graph structure and task statuses, and interact with the orchestrator.
* This design allows flexible orchestration of complex ML workflows while keeping task dependencies explicit and queryable.

Recommended Development Workflow:

1. Start Docker services with `docker-compose -f docker-compose.dev.yml up -d`
2. Start backend via Maven or Gradle
3. Start frontend using `pnpm dev`
4. Access backend (`http://localhost:8080`), frontend (`http://localhost:5173`), Prometheus (`http://localhost:9090`), Grafana (`http://localhost:3000`)
5. Verify `/actuator/health` to ensure all components (Neo4j, Redis, optional Postgres) report `UP`

Notes:

* Ensure passwords and ports for Neo4j and Redis match the backend configuration.
* For distributed or large-scale orchestration, consider integrating Spring Cloud Stream or Kafka for workflow dispatch.
* All tooling is reproducible via Nix flake for consistent development across machines.

---
