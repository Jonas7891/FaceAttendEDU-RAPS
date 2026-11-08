---
title: "Dependencies and Libraries — FaceAttend EDU"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
date: "November 8, 2026"
abstract: |
  This document inventories the dependencies and libraries of FaceAttend EDU from the build files of the ProyectoFaceAttendEDU repository at the final-delivery-2026 tag: Java/Spring Boot, Node/TypeScript and Python/FastAPI microservices, the Kong DB-less gateway, and the Expo web and mobile applications, with declared versions and usage notes.
keywords:
  - dependencies
  - libraries
  - microservices
  - build files
  - APA 7th Edition
---

# Dependencies and Libraries — FaceAttend EDU

Source of truth: the build files of the `ProyectoFaceAttendEDU` repository (`pom.xml`, `package.json`, `pyproject.toml`, `kong.yml`) at the `final-delivery-2026` tag, surveyed on 2026-11-08. Versions are the declared ones. Spring Boot starters inherit their version from the parent BOM (Spring, 2024) unless stated otherwise. The exact installed versions are pinned in `package-lock.json` and `poetry.lock`.

## 1. Back-end

### 01-ms-identity — Java 21 / Spring Boot 4.1.2 (Maven, `identity-service:0.0.1-SNAPSHOT`)

| Dependency | Version | Use |
|---|---|---|
| spring-boot-starter-webmvc | BOM 4.1.2 | REST API (`/persons`, `/users`, `/auth`, `/sessions`) |
| spring-boot-starter-data-jpa | BOM | JPA/Hibernate persistence (`identity` schema) |
| spring-boot-starter-data-redis | BOM | Login attempts and recovery codes in Redis (only identity has it) |
| spring-boot-starter-security | BOM | Authentication filter and password hashing |
| spring-boot-starter-validation | BOM | Bean validation of DTOs |
| spring-boot-starter-kafka | BOM | Events between microservices |
| spring-boot-starter-actuator | BOM | Health and metrics |
| springdoc-openapi-starter-webmvc-ui | 3.1.0 | OpenAPI / Swagger UI |
| faceattend-auth | 1.0.0 | Shared session guard (`back-end/shared/auth-java`) |
| minio + okhttp-jvm | 8.6.0 / 5.1.0 | Guardian documents in S3-compatible storage |
| lz4-java, bcprov-jdk18on (`dependencyManagement`) | 1.11.4 / 1.86 | Pinned transitive versions (Kafka test jar and MinIO) that close published advisories (GHSA) |
| postgresql (runtime) | BOM | PostgreSQL 17 driver |
| lombok (optional) | BOM | Less boilerplate |
| `*-test` starters + h2 (test) | BOM | Tests with an embedded database |
| jacoco-maven-plugin | 0.8.13 | Coverage; the build fails below the minimum set in `pom.xml` |

Migrations do not live in the service: Liquibase applies them from `database/`.

### 02-ms-authorization — Java 21 / Spring Boot 4.1.2 (`authorization-service`)

Same set as identity **without Redis or MinIO**: webmvc, data-jpa, security, validation, kafka, actuator, springdoc 3.1.0, faceattend-auth 1.0.0, pinned lz4-java 1.11.4, postgresql, lombok, test starters + h2 and JaCoCo 0.8.13. RBAC endpoints (`/roles`, `/permissions`, `/user-roles`, `/auth/evaluate`).

### 03-ms-academic — Node 24 + TypeScript (`academic-service:0.1.0`)

| Dependency | Version | Use |
|---|---|---|
| fastify | ^5.12.7 | HTTP framework |
| pg | ^8.11.0 | PostgreSQL driver (plain SQL, no ORM) |
| zod | ^4.6.7 | DTO schema validation |
| kafkajs | ^2.2.4 | Kafka client |
| pino | ^9.14.0 | Structured logging |
| @faceattend/auth, @faceattend/infra | `file:../shared/*` | Session guard, central HTTP error handling, database access |
| typescript / ts-node / ts-node-dev / @types/node | ^5.4 / ^10.9 / ^2.0 / ^24.19 | Build and development |
| jest / ts-jest / @types/jest / @types/pg | ^29.7 / ^29.1 / ^29.5 / ^8.10 | Unit tests with a coverage threshold in `jest.config.js` |

### 04-ms-scheduling — Java 21 / Spring Boot 4.1.2 (`scheduling-service`)

Same set as authorization. Manages environments, time blocks and class sessions.

### 05-ms-attendance — Java 21 / Spring Boot 4.1.2 (`attendance-service`)

The authorization set plus **minio 8.6.0 + okhttp-jvm 5.1.0** (justification attachments, with pinned bcprov-jdk18on 1.86) and **testcontainers-postgresql / testcontainers-junit-jupiter** for integration tests against a real PostgreSQL. Manages attendance records, justifications and reports.

### 06-ms-biometric — Python 3.12 / FastAPI / Poetry (`biometric-service:0.1.0`)

| Dependency | Version | Use |
|---|---|---|
| fastapi / starlette | ^0.162.0 / ^1.10.0 | API framework |
| uvicorn[standard] | ^0.54.0 | ASGI server |
| pydantic / pydantic-settings | ^2.6 / ^2.1 | Models, validation and environment settings |
| motor / pymongo | ^3.3 / ^4.6 | MongoDB access (embeddings, async and sync) |
| opencv-python-headless | ^4.9.0 | Image processing without libGL/X11 |
| numpy | ^1.26.0 | Vector and embedding algebra |
| face-recognition | ^1.3.0 | Face embedding computation (dlib) |
| face-recognition-models (git) | ageitgey repository, commit pinned in `poetry.lock` | Model data |
| Pillow | ^13.0.0 | Image I/O |
| python-multipart | ^0.0.32 | Multipart uploads |
| structlog | ^24.1.0 | Structured logging |
| httpx | ^0.28.1 | HTTP client |
| cryptography / certifi | ^50.0.0 / >=2026.10.19 | Cryptography and root certificates |
| pytest / pytest-asyncio / testcontainers[mongodb] (dev) | ^9.0 / ^0.23 / ^4.0 | Async tests and MongoDB in a container |

`setuptools` is no longer a dependency: `face_recognition_models` only needs `pkg_resources.resource_filename`, which the service answers with a minimal module (`pkg_resources.py`) built on `importlib.resources`.

### 07-ms-configuration — Node 24 + TypeScript (`configuration-service:0.1.0`)

Same set as 03-academic: fastify ^5.12.7, pg ^8.11.0, zod ^4.6.7, kafkajs ^2.2.4, pino ^9.14.0, `@faceattend/auth` and `@faceattend/infra`, and the TypeScript/Jest toolchain. Only difference: `@types/jest` ^30.0.0.

### 08-ms-notification — Node 24 + TypeScript (`notification-service`)

Same set as 03-academic plus **nodemailer ^10.2.1** (`@types/nodemailer` ^8.0.2) for outgoing mail (SMTP). It replaced the former Go implementation.

### 09-ms-quality — Node 24 + TypeScript (`quality-service:0.1.0`)

Same set as 03-academic without `@faceattend/infra`: fastify, pg, zod, kafkajs, pino, `@faceattend/auth` and the TypeScript/Jest toolchain.

### 99-api-gateway — Kong 3.6 DB-less (no code dependencies)

Declarative configuration in `kong/kong.yml` (`_format_version: "3.0"`). The "libraries" are Kong plugins:

- `cors`: origins come from the `CORS_ORIGINS` variable (placeholder `__CORS_ORIGINS__`), with credentials.
- `rate-limiting` with `policy: redis`, per IP: 60/min and 600/h on the credential endpoints, 5/min and 20/h on public school sign-up, and the general `KONG_RATE_LIMIT_PER_MINUTE` limit everywhere else.
- `request-size-limiting` and a custom `post-function`.

The associated infrastructure is Kong + Redis 7.

## 2. Front-end (Expo SDK 57 + React 19.2.3 + React Native 0.86.3 + React Navigation 7)

### Web (`front-end/Web`, `FaceAttend-EDU v1.0.0`)

41 dependencies, among them:

- Fingerprint capture: `@digitalpersona/fingerprint` ^1.0.0 and `@digitalpersona/websdk` ^1.1.0. The websdk is copied into the project by `scripts/copy-digitalpersona.mjs`.
- In-browser face recognition: `@vladmandic/face-api` ^1.7.14.
- Excel report export: `exceljs` ^4.4.0.
- Languages: `i18next` ^26.5.10 and `react-i18next` ^17.1.8.
- Themes: `tinycolor2` ^1.6.0.
- UI: `@gorhom/bottom-sheet` 5.2.14 and the Expo 57 packages (constants, document-picker, font, haptics, image, image-picker, linear-gradient, linking, splash-screen, status-bar, symbols, system-ui, web-browser).
- Navigation: bottom-tabs ^7.4.0, elements ^2.6.3, native ^7.2.2 and native-stack ^5.0.5. The last one is an old, deprecated release: see note 3.
- Others: gesture-handler ~2.32.0, reanimated 4.5.1, safe-area-context ~5.7.0, screens ~4.26.0, svg 15.15.4, async-storage 2.2.0, datetimepicker 9.1.0, slider 5.2.0.

Dev: typescript ~6.0.3, eslint ^9.25.0, eslint-config-expo ~57.0.2, jest ^29.7.0, jest-expo ~57.0.5, playwright-core ^1.64.1 and @types/react ~19.2.4.

### Mobile (`front-end/Mobile`, `faceattendedu v1.0.0`)

33 dependencies. The ones only Mobile has:

- `axios` ^1.16.1 and `@react-navigation/stack` ^7.8.10.
- `expo-camera` ~57.0.4 (face capture) and `expo-secure-store` ~57.0.3, which keeps the `sessionId` in the keystore/Keychain and falls back to AsyncStorage on web.
- `expo-notifications` ~57.0.24, `expo-updates` ~57.0.26, `expo-device`, `expo-file-system`, `expo-sharing`, `expo-localization` and `expo-navigation-bar`.
- `i18next` ^26.0.4 and `react-i18next` ^17.0.2.
- `jwt-decode` ^4.0.0, only used when the token looks like a JWT; the backend issues UUID session ids.
- datetimepicker 8.4.4.

It does not include DigitalPersona, face-api or exceljs. Dev: typescript ~5.9.2, jest ^29.7.0, jest-expo ~57.0.5, @react-native/jest-preset ^0.86.3 and @types/react ~19.1.10.

## 3. Cross-cutting notes

- **Java (01/02/04/05):** a homogeneous Spring Boot 4.1.2 / Java 21 stack. Identity and attendance add file storage (MinIO); only identity uses Redis.
- **Node (03/07/08/09):** a common Fastify 5 / pg / kafkajs / zod 4 / pino 9 base on Node 24, with the shared libraries `back-end/shared/auth-node` and `back-end/shared/node-infra`. No Node service uses an ORM.
- **Python (06):** the only service with face recognition and MongoDB.
- **Gateway:** no code of its own. The policy lives in Kong plugins (CORS, Redis-backed limits and request size).
- **Pending upgrades** (input for the improvement plan in folder 08):
  - `@react-navigation/native-stack` ^5.0.5 (Web) is deprecated: the rest of React Navigation is on version 7.
  - `kafkajs` 2.2.4 has had no new releases since 2023.
  - `numpy` ^1.26 and the Kong 3.6, Kafka 3.8, MongoDB 7, Redis 7 and Liquibase 4.29 images have newer major versions.

Splitting the system into small services with their own database follows Newman (2021). The document format follows the American Psychological Association (2020).

# References

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). https://doi.org/10.1037/0000165-000

Newman, S. (2021). *Building microservices* (2nd ed.). O'Reilly Media.

Spring. (2024). *Spring Boot reference documentation*. https://docs.spring.io/spring-boot/
