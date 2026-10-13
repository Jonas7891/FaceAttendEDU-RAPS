---
title: "Dependencies and Libraries — FaceAttend EDU"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
date: "October 13, 2026"
abstract: |
  This document inventories the dependencies and libraries of FaceAttend EDU from the build files in FULL/back-end and FULL/front-end: Java/Spring Boot, Node/TypeScript, Python/FastAPI and Go microservices, the Kong DB-less gateway, and the Expo web and mobile applications, with declared versions and usage notes.
keywords:
  - dependencies
  - libraries
  - microservices
  - build files
  - APA 7th Edition
---

# Dependencies and Libraries — FaceAttend EDU

Source of truth: build files in `FULL/back-end` and `FULL/front-end` ( surveyed 2026-10-13 ). Versions below are the declared ones; Spring Boot starters inherit their version from the parent BOM unless stated.

## 1. Back-end

### 01-ms-identity — Java 21 / Spring Boot 4.1.1 (Maven, `identity-service:0.0.1-SNAPSHOT`)

| Dependency | Version | Use |
|---|---|---|
| spring-boot-starter-webmvc | BOM 4.1.1 | REST API (`/persons`, `/users`, `/auth`, `/sessions`) |
| spring-boot-starter-data-jpa | BOM | JPA/Hibernate persistence (`identity` schema) |
| spring-boot-starter-data-redis | BOM | Cache/sessions in Redis (only identity has it) |
| spring-boot-starter-security | BOM | Auth filter, password hashing |
| spring-boot-starter-validation | BOM | Bean Validation of DTOs |
| spring-boot-starter-kafka | BOM | Inter-service events |
| spring-boot-starter-liquibase | BOM | Versioned schema migrations |
| spring-boot-starter-actuator | BOM | Health/metrics |
| springdoc-openapi-starter-webmvc-ui | 3.1.0 | OpenAPI/Swagger UI |
| postgresql (runtime) | BOM | PostgreSQL 17 driver |
| lombok (optional) | BOM | Boilerplate reduction |
| *-test slices + h2 (test) | BOM / 2.4.240 | Tests with embedded DB |

### 02-ms-authorization — Java 21 / Spring Boot 4.1.1 (`authorization-service`)

Same set as identity **minus Redis**: webmvc, data-jpa, security, validation, kafka, liquibase, actuator, springdoc 3.1.0, postgresql, lombok, test slices + h2. RBAC endpoints (`/roles`, `/permissions`, `/user-roles`, `/auth/evaluate`).

### 03-ms-academic — Node + TypeScript (`academic-service:0.1.0`)

| Dependency | Version | Use |
|---|---|---|
| fastify | ^4.26.0 | HTTP framework |
| drizzle-orm | ^0.31.0 | SQL ORM (only Node service with Drizzle) |
| pg | ^8.11.0 | PostgreSQL driver |
| zod | ^3.22.0 | DTO schema validation |
| kafkajs | ^2.2.4 | Kafka client |
| pino | ^8.16.0 | Structured logging |
| @opentelemetry/sdk-node | ^0.45.0 | Tracing (only Node service with OTel) |
| typescript / ts-node / ts-node-dev / @types/node | ^5.4/^10.9/^2.0/^20.11 | Build and dev |
| drizzle-kit / @types/pg | ^0.20/^8.10 | Drizzle migrations + PG types |
| jest / ts-jest / @types/jest / supertest / testcontainers | ^29.7/^29.1/^29.5/^6.3/^10.4 | Unit, HTTP and container integration tests |
| eslint | ^8.56.0 | Linter (only academic declares it) |

### 04-ms-scheduling — Java 21 / Spring Boot 4.1.1 (`scheduling-service`)

Same set as authorization: webmvc, data-jpa, security, validation, kafka, liquibase, actuator, springdoc 3.1.0, postgresql, lombok, test slices + h2. Manages environments, schedule blocks, class sessions.

### 05-ms-attendance — Java 21 / Spring Boot 4.1.1 (`attendance-service`)

Same set as authorization/scheduling. Manages attendance records, justifications, reports.

### 06-ms-biometric — Python 3.12 / FastAPI / Poetry (`biometric-service:0.1.0`)

| Dependency | Version | Use |
|---|---|---|
| fastapi | ^0.110.0 | API framework |
| uvicorn[standard] | ^0.28.0 | ASGI server |
| pydantic / pydantic-settings | ^2.6/^2.1 | Models/validation and env config |
| motor / pymongo | ^3.3/^4.6 | MongoDB access (embeddings, async + sync) |
| opencv-python-headless | ^4.9.0 | Image processing without libGL/X11 |
| numpy | ^1.26.0 | Embedding vector math |
| face-recognition | ^1.3.0 | Facial embedding computation (dlib) |
| face-recognition-models (git) | ageitgey repo | Model data (explicit requirement) |
| setuptools | <81 | Provides `pkg_resources` required by the models package |
| Pillow | ^10.0.0 | Image I/O |
| python-multipart | ^0.0.9 | Multipart uploads |
| structlog | ^24.1.0 | Structured logging |
| httpx | ^0.27.0 | HTTP client |
| pytest / pytest-asyncio / testcontainers[mongodb] (dev) | ^8.0/^0.23/^4.0 | Async tests + Mongo in container |

### 07-ms-configuration — Node + TypeScript (`configuration-service:0.1.0`)

fastify ^4.26.0, kafkajs ^2.2.4, pg ^8.11.0 (direct SQL, no ORM), pino ^8.16.0, zod ^3.22.0; dev: jest/ts-jest/@types/jest/typescript/ts-node/ts-node-dev/@types/node/@types/pg.

### 08-ms-notification — Go 1.22 (`github.com/faceattend/notification-service`)

| Direct dependency | Version | Use |
|---|---|---|
| github.com/gin-gonic/gin | v1.9.1 | HTTP router/framework |
| github.com/go-playground/validator/v10 | v10.16.0 | Struct validation |
| github.com/golang-migrate/migrate/v4 | v4.17.0 | SQL migrations |
| github.com/jackc/pgx/v5 | v5.5.0 | PostgreSQL driver/pool |
| github.com/stretchr/testify | v1.8.4 | Test assertions/mocks |
| go.opentelemetry.io/otel | v1.24.0 | Tracing |
| go.uber.org/zap | v1.26.0 | High-performance structured logger |

### 09-ms-quality — Node + TypeScript (`quality-service:0.1.0`)

Identical set to 07-configuration (fastify, kafkajs, pg, zod, pino + TS/Jest toolchain).

### 10-ms-face-auth — (no build)

Directory contains only `.pytest_cache`; no `pyproject.toml`, `requirements*.txt` or functional Dockerfile. Facial logic lives in `06-ms-biometric` today.

### 99-api-gateway — Kong DB-less (no code dependencies)

Declarative config in `kong/kong.yml` (`_format_version: "3.0"`). "Libraries" are Kong plugins: `cors` (origins `*`, methods GET/POST/PUT/PATCH/DELETE/OPTIONS, credentials) and `rate-limiting` (e.g. 60/min identity, 100/min authorization; `policy: redis`, `fault_tolerant: true`, `redis_host: redis:6379`). Associated infra: Kong + Redis.

## 2. Front-end (Expo SDK 57 + React 19.2.3 + RN 0.86.3 + React Navigation 7)

### Web (`front-end/Web`, `FaceAttend-EDU v1.0.0`)

40 dependencies, including: `@digitalpersona/fingerprint` ^1.0.0 and `@digitalpersona/websdk` ^1.1.0 (fingerprint capture; websdk vendored via `scripts/copy-digitalpersona.mjs`), `@vladmandic/face-api` ^1.7.14 (web facial recognition), `xlsx` ^0.18.5 (report import/export), `tinycolor2` ^1.6.0 (theming), `@gorhom/bottom-sheet` latest, expo packages (constants, document-picker, font, haptics, image, image-picker, linear-gradient, linking, splash-screen, status-bar, symbols, system-ui, web-browser), navigation (bottom-tabs ^7.4.0, elements ^2.6.3, native ^7.2.2, native-stack ^5.0.5), gesture-handler ~2.32.0, reanimated 4.5.1, safe-area-context ~5.7.0, screens ~4.26.0, svg 15.15.4, async-storage 2.2.0, datetimepicker 9.1.0, slider 5.2.0. Dev: typescript ~6.0.3, eslint ^9.25.0, eslint-config-expo ~57.0.2, jest ^29.7.0, jest-expo ~57.0.5, @types/react ~19.2.4.

### Mobile (`front-end/Mobile`, `faceattendedu v1.0.0`)

27 dependencies. Only-Mobile: `axios` ^1.16.1, `expo-camera` ~57.0.4 (facial capture), `expo-secure-store` ~57.0.3 (tokens), `expo-file-system` ~57.0.6, `expo-localization` ~57.0.1, `i18next` ^26.0.4 + `react-i18next` ^17.0.2, `jwt-decode` ^4.0.0, datetimepicker 8.4.4. It does NOT include DigitalPersona, face-api or xlsx. Dev: typescript ~5.9.2, jest ^29.7.0, jest-expo ~57.0.5, @react-native/jest-preset ^0.86.3, @types/react ~19.1.10.

## 3. Cross-cutting notes

- Java (01/02/04/05): homogeneous Boot 4.1.1/Java 21 stack; only divergence is Redis in identity.
- Node (03/07/09): common Fastify/pg/kafkajs/zod/pino base; 03-academic is the most equipped (Drizzle + OTel + eslint + supertest/testcontainers).
- Python (06): only one with facial ML and MongoDB.
- Go (08): only one with Gin + pgx + golang-migrate + zap.
- Gateway: no own code; policy in Kong plugins (CORS, Redis-backed rate-limit).

# References

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). https://doi.org/10.1037/0000165-000

Newman, S. (2021). *Building microservices* (2nd ed.). O'Reilly Media.

Spring. (2024). *Spring Boot reference documentation*. https://docs.spring.io/spring-boot/
