---
title: "FaceAttend EDU: an attendance-management system with facial recognition for SENA training environments"
authors: Jonattan Steven Rizo Solano, Diego Andrés Gutiérrez Nuñez, Juan David Arboleda Perdomo
date: "October 22, 2026"
abstract: |
  In many SENA training environments, attendance depends on one instructor taking a roll call of more than 30 learners. This leads to inconsistent records, lost class time and late follow-up on absences and late arrivals. FaceAttend EDU is a web and mobile platform that records entry and exit through facial recognition, falls back to manual or import registration when a scan fails, and centralizes history, justifications, role-based reports and alerts. The system was built with Scrum in two-week sprints, design documentation before code (SDD) and test-driven development (TDD). The architecture has nine hexagonal microservices behind Kong, opaque-session authentication checked at the edge, PostgreSQL with one schema per context, MongoDB for biometric vectors, Kafka for events, and Expo clients for web and Android. The IEEE 829 report of September 21, 2026 records 81 tests with a 96.3% pass rate and 72% code coverage. The ISO/IEC 25010 assessment raised the identity service from an estimated score of 2.6/5 to 4.0/5. The article also describes the privacy measures required by Colombian Law 1581 of 2012, some of which are still in development, and the limits of the available evidence.
keywords:
  - facial recognition
  - attendance management
  - microservices
  - hexagonal architecture
  - personal data protection
  - ISO/IEC 25010
  - IEEE 829
  - SENA
---

# Abstract

In many SENA training environments, attendance depends on one instructor taking a roll call of more than 30 learners [7], [8]. FaceAttend EDU automates that record with facial recognition, offers manual or import registration when a scan fails, and centralizes history, justifications, reports and alerts. This article presents the problem, the objectives, the legal and quality framework, the methodology, the architecture, the security and privacy measures, and the test results recorded up to October 2026. It also discusses what is still to be validated, mainly the pilot with real users.

**Keywords:** facial recognition, attendance management, microservices, hexagonal architecture, personal data protection, ISO/IEC 25010, IEEE 829, SENA.

# Introduction

## Problem

In SENA academic processes, attendance is usually taken with a manual list kept by the instructor. The project's requirements specification states that delegating the registration of more than 30 users to one person, who also has other duties, is inefficient and error-prone [8]. The team identifies four consequences: inconsistent records, administrative overload, impersonation risk, and late follow-up on absences and late arrivals [7].

Current solutions (paper lists, informal spreadsheets, excuses sent by e-mail or message) give no real-time visibility, are hard to consolidate by date, cohort or classroom, and leave no record of who approved or rejected a justification [8]. The team's desk research found that commercial biometric attendance vendors have no justification workflow, and that none of the options reviewed brings cohort, schedule and justification together [8].

## Context

FaceAttend EDU is the training project of team 10 of cohort 3145556 in the Software Analysis and Development technology program (ADSO). The team has three apprentices and no dedicated operations role [7]. Scope is frozen to the Horizon 1 MVP (0 to 3 months) until the product is validated in a pilot [7].

There are three users: the learner, who records attendance contactlessly, checks their history and submits justifications; the instructor, who opens and closes class sessions, sees the cohort's attendance, receives alerts and reviews justifications; and the administrator, who manages users, roles, academic structure and configuration [7].

# Objectives

## General objective

Develop a software solution for attendance recording, history lookup, justification management and report generation that protects data under Law 1581 of 2012 and ensures usability and traceability [7].

## Specific objectives

1. Manage users, roles and permissions, with bulk CSV registration (Identity and Authorization contexts).
2. Manage classrooms, cohorts, programs, courses and academic periods (Academic and Scheduling).
3. Record entry and exit through facial recognition, with manual or import registration as a fallback (Attendance and Biometric).
4. Configure the system and raise alerts for late arrivals and absences (Configuration and Notification).
5. Manage the justification workflow with supporting documents (Attendance).
6. Produce reports and dashboards by date, cohort and classroom (Attendance and Academic).

Each specific objective is traced to functional requirements RF 1 to RF 8 and to the security non-functional requirement RNF 4 of the specification [7].

# Reference framework

## Law 1581 of 2012

Statutory Law 1581 of 2012 sets the general regime for personal data protection in Colombia [1]. It classifies biometric data as sensitive data (art. 5) and allows its processing only with the data subject's explicit authorization, except for legal exceptions (art. 6). For FaceAttend EDU this means that facial enrollment requires prior consent, that biometric vectors must be treated as protected personal information, and that the learner must be able to see, update and revoke their authorization. The project scope records this as an assumption: the system supports educational management, it is not a legal attendance-enforcement mechanism, and biometric processing requires consent and encryption at rest [7].

## ISO/IEC 25010

ISO/IEC 25010:2011 defines a product quality model with eight characteristics: functional suitability, performance efficiency, compatibility, usability, reliability, security, maintainability and portability [2]. The team turned the model into a 24-item instrument on a 1-to-5 Likert scale, served by the `ms-quality` service, which computes a weighted global score: functional suitability 20%; performance efficiency, reliability and security 15% each; compatibility, usability and maintainability 10% each; and portability 5% [13].

## IEEE 829

IEEE Std 829-1998 defines software test documentation: plan, test case specification, incident report and summary report [3]. The project produced all four documents for the back-end [12]. The test plan sets as acceptance criteria a pass rate of at least 95%, coverage of at least 70%, a P95 of at most 200 ms and a 5xx error rate of at most 1% [11].

# Methodology

## Scrum with two-week sprints

The team works with Scrum [4] in two-week sprints, from Monday to Friday of the second week, with an estimated capacity of 20 story points per sprint [10]. The first sprint ran from September 16 to 27, 2026 [10]. The ceremonies are planning (2 hours at most), a 15-minute daily stand-up, mid-sprint refinement, review and retrospective; every retrospective must produce at least one improvement action with an owner and a due date [10]. The technical instructors act as Product Owner, Diego Andrés Gutiérrez Nuñez as Scrum Master, and Juan David Arboleda Perdomo and Jonattan Steven Rizo Solano as developers [16].

## SDD and TDD

The work plan combines two practices [16]. The first is SDD (Software Design Documentation): the design is documented before code is written, and the documentation is updated after implementation. That documentation lives in the `fae-docs` repository and includes 20 architecture decision records (ADRs) [9]. The second is TDD [6]: each change starts with a failing test, continues with the minimum code that makes it pass, and ends with a refactor. Hexagonal architecture makes this cycle cheap because the domain imports no framework and is tested with in-memory doubles [11]. IEEE 829 test case IDs (`TC-xx-xxx`) appear in test names so each test traces back to a requirement [11].

Continuous integration runs on GitHub Actions: the `ci.yml` workflow runs the tests of the nine services and both clients on every push or pull request to `main` [9].

# Architecture

## Hexagonal microservices

The system is split into eight bounded contexts (Identity, Authorization, Academic, Scheduling, Attendance, Biometric, Configuration and Notification) plus one quality-tooling service. Each runs in its own container, on ports 8081 to 8089 [7]. Each service follows the hexagonal (ports and adapters) architecture [5], with `domain/`, `application/` and `infrastructure/` folders [9].

| # | Service | Responsibility | Technology |
|---|---|---|---|
| 01 | `ms-identity` | People, users, sessions, password policy | Java 21 + Spring Boot 4.1 |
| 02 | `ms-authorization` | Roles, permissions and assignments (RBAC) | Java 21 + Spring Boot 4.1 |
| 03 | `ms-academic` | Schools, programs, periods, cohorts, courses, enrollments | TypeScript + Fastify |
| 04 | `ms-scheduling` | Classrooms, schedule blocks, class sessions without double booking | Java 21 + Spring Boot 4.1 |
| 05 | `ms-attendance` | Attendance records, justifications, supporting documents, reports | Java 21 + Spring Boot 4.1 |
| 06 | `ms-biometric` | Facial and fingerprint vectors | Python 3.12 + FastAPI |
| 07 | `ms-configuration` | Academic and security configuration | TypeScript + Fastify |
| 08 | `ms-notification` | Alert types and alerts | TypeScript + Fastify |
| 09 | `ms-quality` | ISO/IEC 25010, ISTQB and ISO/IEC 29110 instruments | TypeScript + Fastify |

Table sources: [7] and the code on the `develop` branch [17]. The notification service was designed in Go 1.22 with Gin, and that is how it appears in the September 2026 reports [12], [14]. It was later rewritten in TypeScript with Fastify to share the Node runtime with Academic and Configuration; the TypeScript version is the current one [17].

## Kong and opaque sessions

Kong OSS 3.6 is the single entry point. It runs without a database, with declarative configuration in `kong.yml`, and routes by path to the nine services [9]. Authentication does not use JWT: on login, `ms-identity` issues an opaque session identifier (a UUID) stored in the `identity.user_session` table [9]. On every non-public route, a Kong pre-function requires the `Authorization: Bearer <uuid>` header and asks `ms-identity` whether the session is still active. It answers 401 when the session is missing, malformed or expired, and 503 when the identity service does not respond. Positive answers are cached for 10 seconds and invalidated on logout [9]. In addition, every service repeats the session and permission check with its own guard, as defense in depth [9]. Passwords are stored with bcrypt at cost 12 [9], [17].

## Data: PostgreSQL per schema and MongoDB for embeddings

Relational data lives in one logical database, `faceattend_db`, on PostgreSQL 17, with one schema per context, one database role per schema and no foreign keys across schemas. References between contexts are public IDs resolved through the API or events [9]. Migrations run with Liquibase in eight chained jobs, one per context [9].

Biometric vectors live in MongoDB 7 because they are accessed by similarity search, not relational queries [9]. The biometric service detects the face with OpenCV and extracts a 128-dimensional vector with `face_recognition` (based on dlib) [9]. In the current code, the match threshold is a cosine similarity of 0.85. During the liveness check, the test that every frame shows the same person uses a looser threshold, 0.75, because frames taken mid-gesture shift the vector [17].

## Events with Kafka

Synchronous communication goes over REST through Kong. Asynchronous communication uses domain events on Apache Kafka in KRaft mode, published with `KafkaTemplate` in Java and `kafkajs` in TypeScript [9]. The attendance record is written in a local transaction in the `attendance` schema; decoupled reactions, such as alerts, are triggered by events after commit [9]. The outbox pattern, dead-letter queues and compensation handlers are not implemented yet: this is technical debt item AT-006 [9].

## Expo clients for web and mobile

Both clients use Expo SDK 57 [9], [17]. The web client is an Expo Web static export served by nginx; it bundles the DigitalPersona SDK for fingerprints. The Android app is built and signed on EAS [9]. Both use the MVVM pattern and access the camera through `expo-camera`, with a liveness challenge before the face is captured [9].

## Deployment

According to ADR-020, the back-end is deployed with Docker Compose on an AWS EC2 `t3.xlarge` instance running 13 containers (nine services, Kong, MongoDB, Redis and Kafka). PostgreSQL runs on Neon, in the `sa-east-1` region, and the web client is published on Vercel [9].

# Security and privacy

## What is implemented

- Opaque-session authentication checked in Kong and in every service, with session rotation (test `RefreshSessionUseCaseImplTest`) [9], [15].
- Account lockout after 5 failed login attempts, for 30 minutes, the `AuthProperties` defaults (test `shouldLockAccountAfterFiveFailedAttempts`) [15], [17].
- Role-based access control (administrator, instructor, learner), tested in `CheckPermissionUseCaseImplTest` [15].
- Passwords hashed with bcrypt at cost 12 [9].
- Liveness check against presentation attacks: a photo or a screen is rejected with HTTP 400 [15].
- Traceability: every request carries a correlation ID, and errors use one uniform format with no stack traces [13], [14].

## What is in development

As of October 22, 2026, three controls required by Law 1581 and by the accepted ADRs are not yet on the `develop` branch [17]:

1. **Informed consent.** Biometric enrollment must record the learner's explicit authorization before the face is captured, as article 6 of Law 1581 requires [1], [7].
2. **Encryption of biometric and personal data.** ADR-016 accepts encryption at rest for personal data and biometric vectors, but its October 17, 2026 note confirms it is not implemented; today encryption at rest relies on the managed providers [9]. The work in progress encrypts those fields in the application with AES-256-GCM.
3. **Supporting documents in MinIO.** ADR-009 decides to store justification documents in MinIO, a self-hosted, S3-compatible object store with server-side encryption [9]. Uploading supporting documents to MinIO is in development.

In parallel, the demo seed accounts are being removed and the deployment hardened. For example, ADR-020 records the Kong admin port open on EC2 as pending hardening [9].

# Results

## Delivered functionality

The Horizon 1 MVP has the Identity, Academic and Scheduling contexts and the Attendance core validated, plus facial enrollment, the justification workflow and in-app notifications [7]. Fingerprint enrollment and verification in the web client shipped ahead of the roadmap, which had placed them in Horizon 3 [8].

## Testing under IEEE 829

The IEEE 829 summary report of September 21, 2026 covers eight back-end services [12]. At that time notification was written in Go, and `ms-quality` was not part of the suite.

| Type | Run | Passed | Failed | Pass rate |
|---|---:|---:|---:|---:|
| Unit | 3 | 3 | 0 | 100% |
| CRUD | 55 | 53 | 2 | 96.4% |
| API | 23 | 22 | 1 | 95.7% |
| **Total** | **81** | **78** | **3** | **96.3%** |

Source: [12].

Coverage was 72% for code (target ≥ 70%), 68% for branches (target ≥ 65%), 100% for CRUD operations and 95% for validation (target ≥ 90%) [12]. Two services, `ms-scheduling` and `ms-configuration`, were left pending [12]. P95 response times of the five endpoints measured were between 30 and 85 ms, and the highest P99 was 150 ms, all below the 200 ms target [12]. Under concurrency, 50 simultaneous reads and 20 simultaneous writes had a 100% success rate, and a mixed load of 100 requests had 99% [12]. SQL injection, XSS, CSRF and authentication-bypass tests found no vulnerabilities [12]. Two critical incidents were recorded and are now closed (duplicates in bulk attendance creation, and a 500 error in notification when the database was down), and two minor ones remain open [12].

The unit testing documentation of October 10, 2026 covers the identity, authorization, scheduling, attendance and biometric services. It reports 88% line coverage and 82% branch coverage, and the five tests traced to requirements pass [15]. This figure does not replace the IEEE 829 one: it is later and measures only unit tests of five services.

For end-to-end tests, the team loads a data set with 2 schools, 1,600 learners, 80 instructors, about 1,210 sessions and about 81,000 attendance records [11].

## Quality under ISO/IEC 25010

The first application of the instrument found eight issues, four of high severity and four of medium. They included wrong HTTP status codes, missing input validation, 500 errors that exposed stack traces, unpaginated listings and different error formats across services [13]. Before fixing them, the team estimated an average of 2.6/5 [13]. After the fixes, the assessment of the `ms-identity` cities CRUD scored 4.0/5 (80%), level "Good" [13]. It was a single assessment of a single service, done by the team itself.

# Discussion

The results show that the architecture meets the technical goals that can be measured without users: the test pass rate and response times exceed the IEEE 829 plan targets, and the ISO/IEC 25010 instrument helped find and fix concrete defects. The evidence has four limits.

1. **There is no pilot yet.** Product targets such as 80% of attendance captured automatically, 10 seconds or less per facial record, and 85% or more facial success under normal light are still hypotheses. Interviews with 5 instructors and 30 learners and the observation of 3 sessions were planned for October 2026 [8]. The baselines are also estimates still to be validated, such as 45 seconds per learner for manual registration and 30 minutes to consolidate a cohort report [8].
2. **The quality figures come from different measurements.** The IEEE 829 report is from September and describes the notification service in Go. The 88% coverage is from October and covers only unit tests. The ISO/IEC 25010 assessment covers one CRUD. No figure describes the whole system in its current version.
3. **There is documented technical debt.** The outbox pattern and dead-letter queues are missing (AT-006), observability is partial (structured logs exist, but no Prometheus, Grafana or Jaeger), and each stack keeps its own session guard, so the implementations can drift apart [9].
4. **Privacy is incomplete.** Until consent and field encryption are in production, the system does not yet meet the privacy-by-design principle it set for itself [8], [9].

Some decisions changed during implementation. ADR-002 proposed a modular monolith, and the system runs as nine containers, although it keeps the per-schema data boundaries. ADR-011 proposed Jenkins and was replaced by GitHub Actions. ADR-013 ruled out Expo and was replaced by ADR-019 [9]. Because every change is recorded as an implementation note or a new ADR, the gap between what was planned and what was built can be audited.

# Conclusions

1. FaceAttend EDU addresses the stated problem with a web and mobile platform that records attendance through facial recognition, offers manual and import fallbacks, and centralizes history, justifications, reports and alerts.
2. Splitting the system into nine hexagonal services with one schema per context kept each domain's model isolated and let each service use the most suitable stack, such as Python for computer vision.
3. The opaque session checked in Kong lets the server revoke access: Kong invalidates its cache on logout and each service's cache lasts 10 seconds. A stateless JWT cannot do this without extra infrastructure.
4. The recorded tests exceed the IEEE 829 plan targets for pass rate, coverage and response times, and the ISO/IEC 25010 instrument improved quality in a measurable way.
5. Before a deployment with real data, consent, AES-256-GCM encryption of biometric and personal data, and storage of supporting documents in MinIO must be finished, and the product targets must be validated in a pilot with a real cohort.

# References

[1] Congress of the Republic of Colombia. (2012). *Ley Estatutaria 1581 de 2012, por la cual se dictan disposiciones generales para la protección de datos personales*. Diario Oficial 48.587.

[2] International Organization for Standardization. (2011). *ISO/IEC 25010:2011. Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — System and software quality models*.

[3] IEEE. (1998). *IEEE Std 829-1998. IEEE Standard for Software Test Documentation*.

[4] Schwaber, K., & Sutherland, J. (2020). *The Scrum Guide*. https://scrumguides.org

[5] Cockburn, A. (2005). *Hexagonal architecture*. https://alistair.cockburn.us/hexagonal-architecture/

[6] Beck, K. (2002). *Test-Driven Development: By Example*. Addison-Wesley.

[7] FaceAttend EDU team. (2026). *fae-docs: `01-context/overview.md` and `01-context/scope.md`*. Branch `main`.

[8] FaceAttend EDU team. (2026). *fae-docs: `03-product/problem-framing.md` and `03-product/vision.md`*. Branch `main`.

[9] FaceAttend EDU team. (2026). *fae-docs: `05-architecture/overview.md` and records ADR-001 to ADR-020*. Branch `main`.

[10] FaceAttend EDU team. (2026). *fae-docs: `00-governance/agile-conventions.md`*. Branch `main`.

[11] FaceAttend EDU team. (2026). *fae-docs: `11-quality/README.md`, `testing-strategy.md` and `tdd-guide.md`*. Branch `main`.

[12] FaceAttend EDU team. (2026, September 21). *IEEE 829 — Test summary report*. ProyectoFaceAttendEDU, `back-end/IEEE829_TEST_SUMMARY_REPORT.md`, branch `develop`.

[13] FaceAttend EDU team. (2026). *ISO/IEC 25010 quality report*. ProyectoFaceAttendEDU, `back-end/ISO25010_QUALITY_REPORT.md`, branch `develop`.

[14] FaceAttend EDU team. (2026, September 21). *ISO 9001:2015 instrument implementation report*. ProyectoFaceAttendEDU, `back-end/QUALITY_REPORT.md`, branch `develop`.

[15] FaceAttend EDU team. (2026, October 10). *Unit testing documentation — FaceAttendEDU back-end*. FaceAttendEDU-RAPS, `06-Documentación de pruebas/en/Unit_test_documentation.md`.

[16] FaceAttend EDU team. (2025). *Technical report* and *Activity plan*. FaceAttendEDU-RAPS, `03-Documentación de propuesta técnica del software/en/`.

[17] FaceAttend EDU team. (2026, October 22). *ProyectoFaceAttendEDU source code*, branch `develop`, commit `2f8bf2d`: `back-end/08-ms-notification/STACK.md`, `back-end/06-ms-biometric/infrastructure/config/settings.py`, `back-end/01-ms-identity/.../SecurityConfig.java` and `AuthProperties.java`, `back-end/05-ms-attendance/pom.xml`, `front-end/*/package.json`.
