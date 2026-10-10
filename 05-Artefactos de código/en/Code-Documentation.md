---
title: "Technical Code Documentation — FaceAttendEDU"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
date: "October 10, 2026"
abstract: |
  This document constitutes the formal documentation of the FaceAttendEDU source code, authored under the standards of the seventh edition of the APA guidelines. It describes the implementation of a polyglot modular monolith, structured through a hexagonal architecture (ports and adapters) to ensure the segregation of business logic from infrastructure. The system integrates nine specialized microservices developed in Java, TypeScript, Python, and Go, coordinated via an API Gateway (Kong) and an asynchronous event bus (Apache Kafka). Polyglot persistence is detailed using PostgreSQL, MongoDB, and Redis, as well as the implementation of cosine similarity algorithms for biometric processing and weighted scoring models for quality assessment based on the ISO/IEC 25010 standard. This documentation serves as technical evidence of the alignment between the architectural design and the actual software implementation.
keywords:
  - hexagonal architecture
  - polyglot modular monolith
  - polyglot persistence
  - facial biometrics
  - ISO/IEC 25010
  - APA 7th Edition
---

# Introduction

The technical implementation of FaceAttendEDU represents the materialization of a complex academic attendance management ecosystem, where biometric precision and data security are the fundamental pillars. The system's development was not limited to the choice of a single programming language but adopted a technological selection strategy based on functional specialization, resulting in a polyglot environment.

## Problem Statement
Academic attendance management often faces problems of identity spoofing and the inefficiency of manual records. FaceAttendEDU addresses this challenge through the automation of recording via facial and fingerprint biometrics, eliminating human error and fraud.

## Document Objectives
The primary objective of this document is to provide an exhaustive and formal description of the source code implementation. It seeks to document not only "what" the system does but "how" it is built, detailing the relationship between domain entities, use cases, and infrastructure adapters, ensuring that the system is maintainable, auditable, and scalable.

## Scope of Documentation
This document covers the entirety of the system's back-end, including the nine business microservices and the API Gateway. It details the logic implemented in Java, TypeScript, Python, and Go, as well as the data infrastructure configuration and container orchestration.

# System Architecture

The FaceAttendEDU system is based on a **Polyglot Modular Monolith** paradigm. Unlike traditionally distributed microservices, the system is organized into independent modules within a single repository, allowing coordinated deployment while maintaining the logical autonomy of each bounded context.

## Architectural Patterns

### Polyglot Modular Monolith
The choice of a polyglot approach responds to the need to optimize performance according to the task. Java was implemented for robust process orchestration, Python for the scientific processing of biometric vectors, Go for high-concurrency notifications, and TypeScript for lightweight services. This strategy allows each module to be developed with the most efficient tool for its specific purpose.

### Hexagonal Architecture (Ports and Adapters)
To mitigate technological coupling, the system implements Hexagonal Architecture. This structure organizes the code into three concentric layers:
1. **Domain Layer (The Core)**: Contains pure entities and business rules, completely independent of any external framework or library.
2. **Application Layer**: Defines the "Use Cases," which act as orchestrators that receive data from input ports and coordinate the execution of the domain.
3. **Infrastructure Layer (Adapters)**: Implements the ports defined by the domain. It includes REST controllers (input) and persistence or messaging implementations (output).

## Infrastructure and Technology Stack

### Polyglot Persistence Strategy
The system avoids dependency on a single database engine, applying persistence according to the nature of the data:
- **PostgreSQL 17**: Used for relational and structured data across 8 independent schemas, ensuring ACID consistency.
- **MongoDB 7**: Used exclusively by the biometric service for the storage of embeddings (high-dimensional vectors), optimizing similarity searches.
- **Redis 7**: Implemented for session management and traffic control (rate limiting) in the Gateway, reducing response latency.

### Event-Driven Communication
Synchronization between modules is carried out via **Apache Kafka 3.8.0**. The system employs a publish/subscribe model where services emit "Domain Events" (e.g., `UserCreated`, `AttendanceRecorded`). This allows services such as the Notification service to react asynchronously to changes in other modules without creating circular dependencies.

### Traffic Management and Security (Edge Routing)
Access to the system is centralized in **Kong Gateway 3.6**, configured in *DB-less* mode. The Gateway is responsible for:
- **Routing**: Mapping external routes to internal services.
- **Security**: JWT RS256 token validation and application of CORS policies.
- **Resilience**: Rate limiting to prevent denial-of-service attacks.

# Microservice Implementation

## 01-ms-identity: Identity and Session Management
This service constitutes the security core of the system, managing the legal existence of persons and their access credentials.

### Domain Model and Use Cases
The domain focuses on the `Person`, `User`, and `UserSession` entities. Primary use cases include user registration, authentication via hashed passwords, and account recovery using temporary SHA-256 codes with a 10-minute TTL.

### Implementation Logic
It implements a **Lazy Expiration** mechanism. Sessions are not actively removed by a background process but are validated upon each request; if the lifetime has expired, the service closes the session at the time of reading.

## 02-ms-authorization: Role-Based Access Control (RBAC)
Implements granular security through the RBAC pattern, ensuring that each user accesses only the functions permitted by their role.

### RBAC Implementation
The system maps the relationship `User $\rightarrow$ Role $\rightarrow$ Permission`. To optimize real-time permission evaluation, the service uses **Redis** to cache the permission tree for each user, avoiding repetitive queries to PostgreSQL.

## 03-ms-academic: Academic Structure
Manages the organizational hierarchy of the institution, from the campus to the student.

### Data Modeling
Uses a complex relational model that links `School` $\rightarrow$ `Program` $\rightarrow$ `Cohort` $\rightarrow$ `Course`. To manage these relations with high efficiency, it implements **Drizzle ORM**, facilitating the execution of optimized JOINs over the `academic` schema.

## 04-ms-scheduling: Session Scheduling
Translates the academic structure into an operational class calendar.

### Conflict Prevention
The core logic implements uniqueness constraints in the database on the triplet `(environment, day, time)`, preventing the scheduling of two classes in the same physical space or the assignment of an instructor to two simultaneous sessions.

## 05-ms-attendance: Attendance Recording and Justifications
This is the transactional module where entry and exit timestamps are recorded.

### Justification Workflow
Implements a state flow for managing absences: `Submitted` $\rightarrow$ `Pending` $\rightarrow$ `Approved/Rejected`. Digital supports are stored in an S3-compatible object system (MinIO).

## 06-ms-biometric: Biometric Vector Processing
This is the most technically complex component, responsible for the extraction and comparison of facial and fingerprint features.

### Cosine Similarity Algorithm
For 1:N identification, the system extracts a feature vector (embedding) from the captured image using deep neural network models (FaceNet/ArcFace) via **OpenCV DNN**. The comparison is performed using **Cosine Similarity**, calculating the cosine of the angle between two vectors in a multi-dimensional space:
$$\text{similitud} = \frac{\mathbf{A} \cdot \mathbf{B}}{\|\mathbf{A}\| \|\mathbf{B}\|}$$
Where $\mathbf{A}$ is the vector stored in MongoDB and $\mathbf{B}$ is the vector captured.

## 07-ms-configuration: Parameters and Updates
Manages global system configuration and the approval flow for biometric updates.

### Biometric Update Workflow
Implements an audit process where any request to change a biometric template must pass through a `Review` state before being `Approved`, preventing a user from altering their data to impersonate another.

## 08-ms-notification: Alerting and Notification System
Implemented in **Go**, this service optimizes the mass sending of alerts and push notifications.

## Event-Driven Orchestration
The service consumes Kafka topics from all other modules. For example, upon detecting an `AbsenteeismDetected` event, the service automatically triggers a notification to the tutor and the student via Firebase and SMTP.

## 09-ms-quality: Software Quality Evaluation
Implements measurement instruments based on international standards to evaluate the system's maturity.

### Weighted Scoring Model
For evaluation under the **ISO/IEC 25010** standard, the system implements a weighted scoring model. Each quality characteristic (e.g., Portability, Maintainability) has a relative weight. The global score is calculated as:
$$\text{Global Score} = \sum (\text{Characteristic Score}_i \times \text{Weight}_i)$$

## 99-api-gateway: API Orchestration
The Gateway acts as the facade of the system, abstracting the complexity of the internal microservices.

### Kong Implementation
Configured in *DB-less* mode, the Gateway uses a declarative `kong.yml` file to define routing. It implements security at the "edge," validating that each request possesses a valid JWT token before redirecting it to the corresponding service.

# Cross-Cutting Concerns

## Security and Authorization
Security is implemented in layers. The Gateway validates identity, while the Authorization service validates the specific permission. This separation allows changing the permission policy without affecting the authentication mechanism.

## Observability and Quality
The implementation of the **ISO/IEC 29110** standard is reflected in the development process, while the quality service (`09-ms-quality`) allows the system to periodically self-evaluate, generating conformity reports.

# Discussion and Conclusion

The adoption of a polyglot modular monolith has allowed FaceAttendEDU to optimize performance in critical areas, such as biometrics and notifications, without sacrificing deployment simplicity. Hexagonal Architecture has proven effective in isolating business logic, allowing the system to evolve technologically without risk of regressions in the rules of business.

Despite the complexity added by the management of multiple languages, the benefits in terms of computational efficiency and scalability modularity justify the implemented architecture. The system is presented as a robust solution aligned with international software engineering standards.

# References

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). https://doi.org/10.1037/0000165-000

Cockburn, A. (2005). *Hexagonal Architecture (Ports and Adapters)*.

International Organization for Standardization. (2020). *Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — System and software quality models* (ISO/IEC 25010:2020).

International Organization for Standardization. (2020). *Software engineering — Lifecycle profiles for Very Small Entities (VSEs)* (ISO/IEC 29110:2020).

IEEE. (n.d.). *Standard for Biometric Data Interchange*.
