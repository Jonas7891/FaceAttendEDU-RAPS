---
title: "Data Flow Within the System — FaceAttend EDU"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
date: "October 13, 2026"
abstract: |
  This document describes the flow that data performs within the FaceAttend EDU system: entry points through the web and mobile clients, edge routing and authentication at the Kong gateway, per-microservice data ownership with polyglot persistence (PostgreSQL, MongoDB, Redis), asynchronous communication through Apache Kafka, and exit points (synchronous responses, SMTP mail, alerts and derived reports). It is based on the implementation in FULL and the fae-docs sections 02-domain, 05-architecture, 06-data, 07-api and 09-microservices.
keywords:
  - data flow
  - microservices
  - api gateway
  - polyglot persistence
  - kafka events
  - APA 7th Edition
---

# Introduction

## Document Objective

To describe, in an orderly and verifiable manner, the path that data follows within FaceAttend EDU, from its entry through the clients to its storage and exit, identifying the responsible microservice, the persistence and the events involved at each stage.

## Document Scope

This document covers the data flow at the analysis level (stages, owners, stores and events). It does not replace the design documents (class, sequence and C4 diagrams) nor the code documentation, which detail the implementation.

## Method and Sources

The flow was reconstructed from the implementation in `FULL` (compose files, `kong.yml`, per-service builds) and `fae-docs` (`05-architecture`, `07-api`, `09-microservices`, `06-data`, `02-domain`, `08-uml`).

# Data Flow Within the System

## Stage 0 — Entry: Clients

The only entry points in normal operation are the Expo web application (served through nginx on port 8090) and the native mobile application. Both address exclusively the Kong gateway (`http://<host>:8080`, configured through `EXPO_PUBLIC_API_URL`); there is no direct access to the microservices, whose ports (8081–8089) are bound to loopback for local development only. Each authenticated call carries `Authorization: Bearer <sessionId>`. Payloads are JSON with `?page&limit` pagination, except facial registration from mobile, which uses multipart (`POST face/register`).

## Stage 1 — Edge: Kong Gateway (DB-less)

Kong 3.6 in DB-less mode (`kong.yml`) applies, in order: a Lua pre-function, CORS (`origins *`, credentials enabled) and rate limiting at 300 requests per minute backed by Redis, plus per-upstream limits (e.g., 60/min for identity, 100/min for authorization, 200/min for attendance). Routes are mapped by prefix to the owning microservice (`/persons|users|auth|sessions` to identity on 8081; `/roles|permissions|user-roles` to authorization on 8082; academic, scheduling, attendance, biometric, configuration, notification and quality equivalents). The pre-function allows `OPTIONS`, health checks, login/logout/refresh and password-recovery endpoints without a token; every other request requires a Bearer session identifier validated by introspection against `ms-identity` (`GET /api/v1/sessions/{id}`), returning 401 for unknown or non-`Active` sessions and 503 when identity is unavailable. The client therefore never reaches stored data directly; three Docker networks (`faceattend-edge`, `faceattend-app`, `faceattend-data`) enforce this separation.

## Stage 2 — Authentication and Authorization on Every Call

Login (`POST /api/v1/auth/login` with email or username plus password) verifies the BCrypt hash against `identity.app_user.password_hash` and inserts a row in `identity.user_session` with `Active` status, returning `{sessionId, userId}` with a generic 401 that avoids user enumeration. The client then loads its context (roles, permission evaluation, person profile). Each subsequent request passes two checks: the Kong edge check and the owning microservice guard (token filter in the Java services, auth guard in the TypeScript services, permission requirement in Python, middleware in Go), which revalidates the session and the specific permission. Refresh rotates the session identifier (single use) and logout closes the row.

## Stage 3 — Data Ownership: One Microservice, One Schema

Each microservice owns its data exclusively; there are no foreign keys between bounded contexts, only public identifiers (UUIDs or IDs) validated by the application:

- **Identity (Java, 8081):** `identity.person`, `app_user` (1:1 with person), `user_session`, `password_policy`.
- **Authorization (Java, 8082):** `role`, `permission`, `role_permission` (composite key), `user_role` (user identifier without foreign key).
- **Academic (TypeScript, 8083):** `school`, `program`, `academic_period`, `cohort`, `course`, `academic_actor_type`, `academic_actor` (links person and school), `enrollment` (actor–cohort, unique pair).
- **Scheduling (Java, 8084):** `environment`, `schedule_block` (cohort, course and instructor as cross-context references; unique room–slot and instructor–slot constraints; ISO day 1–7), `class_session` (materialization of a block on a date; unique block–date; `Open → Closed | Cancelled`).
- **Attendance (Java, 8085):** `attendance_record` (session plus actor, unique pair, `Present | Absent | Late | Justified`, capture method `FACIAL | MANUAL | IOT | IMPORT`), `justification` (1:1 with the record; `Pending → Approved | Rejected`), `justification_type` (global when `school_id` is null, per-school otherwise), `supporting_document` (file metadata only), `attendance_report` (derived JSONB projection without foreign keys).
- **Biometric (Python, 8086):** MongoDB collections for facial and fingerprint embeddings (`person_id`, template version, encoding, model version, enrollment timestamp, active flag) plus the canonical SQL case table hosted in the configuration schema.
- **Configuration (TypeScript, 8087):** `academic_configuration` (per school), `security_configuration` (global), `biometric_update_case` (`Pending → In_Review → Approved | Rejected`).
- **Notification (Go, 8088):** `alert_type`, `alert` (actor reference, raised/resolved timestamps).

Every SQL table carries the audit block (`created_at`, `updated_at`, `deleted_at`, `created_by`, `updated_by`, `deleted_by`, `row_version`) with soft delete and optimistic locking.

## Stage 4 — Business Flows

**Academic to scheduling.** Schools, programs, periods, cohorts, courses, academic actors and enrollments are created in the academic service; environments, schedule blocks and class sessions (open/close) are then created in scheduling. No calls occur between the two microservices; the application supplies the identifiers.

**Facial attendance.** Enrollment (`POST /biometric/facial/enroll`) upserts the embedding in MongoDB, deactivating the previous one. Identification compares the captured encoding with up to 10,000 stored templates using cosine similarity with a 0.85 threshold and logs the match. The write (`POST /attendance-records`) is transactional with a pre-check on the unique session–actor pair (409 on duplicate) and publishes an `attendance-events` message on a best-effort basis. The match runs on the client side; attendance never calls biometric directly.

**Justification.** The justification is created in `Pending` state (1:1 with the attendance record), supporting documents are attached as metadata rows (the storage URI is produced by the client; no object storage is implemented), and review transitions it to `Approved` or `Rejected`, requiring an attachment when the type mandates it.

**Biometric update case.** Update requests follow an audit workflow (`Pending → In_Review → Approved | Rejected`) with the requester mandatory and fingerprint number restricted to values 1–10 for fingerprint type (null for facial).

**Password recovery.** The request always returns 202 to avoid enumeration (60-second cooldown, 6-digit SHA-256 code held in memory with 10-minute TTL and 5 attempts); this is the only business-level inter-service call, from identity to notification over the internal network to send the mail; verification and reset then validate the password policy, hash the new password with BCrypt, close active sessions and publish a `PasswordChanged` event.

## Stage 5 — Persistence

Relational data lives in PostgreSQL 17 (`faceattend_db`), one schema per bounded context, migrated with Liquibase per microservice. Biometric vectors live in MongoDB 7 (`faceattend_biometric`). Redis 7 holds only the Kong rate-limit state; sessions persist in PostgreSQL and recovery codes in memory or Redis. Documented object storage (S3/MinIO) is not implemented: supporting documents persist only as metadata rows.

## Stage 6 — Kafka Events

Services publish domain events on a best-effort fire-and-forget basis (errors only logged, no outbox, dead-letter queue or retry): identity, city and password-policy events; role and permission events; academic, class-session, environment, schedule-block, attendance, justification and configuration events. No consumer is implemented at runtime: attendance events do not create alerts automatically and `PasswordChanged` invalidates nothing remotely. The event catalog describes the target architecture, not the current runtime.

## Stage 7 — Exits

Synchronous JSON responses return through Kong (2xx/4xx/429/503). Mail exits exclusively through the notification service (SMTP settings from the environment; empty host yields a `202 {skipped}` response). Alerts persist as rows and optionally fan out to a configured HMAC-signed webhook, otherwise a no-op. Reports are derived JSONB projections read on demand, with no ETL pipeline.

# Discussion and Conclusion

The data flow confirms the governing principle that each microservice owns its data: entry is centralized at Kong, ownership is partitioned by schema, and consistency between contexts is eventual and application-validated rather than referentially enforced. The documented gaps (no cross-context validation, producers without consumers, metadata-only documents) are analysis-level findings that the design and implementation phases must address explicitly.

# References

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). https://doi.org/10.1037/0000165-000

Apache Software Foundation. (2024). *Apache Kafka documentation*. https://kafka.apache.org/documentation/

Fielding, R. T. (2000). *Architectural styles and the design of network-based software architectures* (Doctoral dissertation, University of California, Irvine).

Kong Inc. (2024). *Kong Gateway documentation (DB-less mode)*. https://docs.konghq.com/gateway/

Newman, S. (2021). *Building microservices* (2nd ed.). O'Reilly Media.
