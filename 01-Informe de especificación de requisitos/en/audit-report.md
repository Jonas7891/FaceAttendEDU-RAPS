# Software Requirements Audit Report - FaceAttendEDU

## 1. General Project Status Summary

The project shows significant progress in the base architecture and the identity core, but currently exists in a state of **partial disconnection between documentation and actual implementation**.

- **Documentation:** Extremely well-structured, utilizing a modern approach with User Stories, clear Acceptance Criteria (AC), and a measurable Non-Functional Requirements (NFR) matrix. However, some User Stories describe functionalities as "proposed" or "pending" that should already be part of the MVP.
- **Implementation:** The technology stack (Java/Spring) is correctly implemented in the Identity and Authorization microservices. The hexagonal architecture is visible and consistent. Nevertheless, critical services for **Attendance (Facial), Biometrics, and Configuration** are practically empty or exist only as skeletons, meaning the business core (facial recognition) lacks functional code evidence.
- **Global Status:** The system is currently a robust user and permission management system, but it is not yet a functional facial attendance control system.

---

## 2. Detailed Requirement Compliance

### A. Functional Requirements (RF / HU)

| ID | Requirement | Status | Technical Evidence | Observations |
|:---|:---|:---|:---|:---|
| **HU-IAM-001** | User Login | **Implemented** | `AuthController.java` (`/login`, `/logout`, `/me`) | Complies with opaque sessions and credential validation. |
| **HU-IAM-002** | User Registration | **Implemented** | `UserController.java` (`POST /users`) | User creation and activation flow is implemented. |
| **HU-IAM-003** | Role-Based Permissions (RBAC) | **Implemented** | `RoleController.java`, `PermissionController.java`, `UserRoleController.java` | Role assignment and permission evaluation (`/auth/evaluate`) are implemented. |
| **HU-ATT-001** | Facial Registration | **Not Implemented** | N/A | No functional endpoints for facial capture or validation were found. |
| **HU-ATT-002** | Facial Enrollment | **Not Implemented** | N/A | No evidence of biometric template storage services. |
| **HU-ATT-003** | Alternative Registration | **Not Implemented** | N/A | No logic for survey or personal data validation exists. |
| **HU-JUS-001** | Justification Upload | **Partial** | `JustificationController.java` | Controller exists, but integration with the file storage service is missing. |
| **HU-JUS-002** | Approval/Rejection | **Not Implemented** | N/A | No state transition logic for justifications. |
| **HU-ACAD-001** | Environments/Classrooms | **Not Implemented** | N/A | `AcademicController` or similar was not found. |
| **HU-HIST-001** | Attendance Query | **Partial** | `AttendanceReportController.java` | Report structure exists, but advanced filtering logic is missing. |
| **HU-CONF-001** | Alert Configuration | **Not Implemented** | N/A | No evidence of user preference persistence. |

### B. Non-Functional Requirements (NFR)

| ID | Category | Status | Technical Evidence | Observations |
|:---|:---|:---|:---|:---|
| **NFR-004** | Security | **Implemented** | `SecurityConfig.java` (BCrypt cost 12), `AuthTokenFilter` | Password hashing and endpoint protection are complied with. |
| **NFR-002** | Availability | **Implemented** | `/health` and `/health/ready` endpoints | Liveness and readiness checks are implemented. |
| **NFR-008** | Maintainability | **Implemented** | `RoleControllerTest.java`, Hexagonal Structure | High code quality, use of unit tests and layer separation. |
| **NFR-009** | Portability | **Implemented** | `docker-compose.yml`, `Dockerfile` | The system is fully containerized. |
| **NFR-001** | Performance | **Not Verifiable** | N/A | No load test reports or P95 latency data exist. |

---

## 3. Proposed Changes

### Formal Changes (Wording and Structure)
| Req. | Current Text | Proposed Text | Justification |
|:---|:---|:---|:---|
| **HU-IAM-001** | "Sistemas deny access with 401 and generic message `Invalid email or password`" | "The system denies access with HTTP 401 and the generic message: `Invalid credentials`" | Standardize the language and terminology for consistency. |
| **HU-CONF-007** | (Note about RF 4.5 inconsistency) | Remove the inconsistency note and rename HU to `HU-CONF-007: IoT Alert Management` | The inconsistency has already been detected; it is preferable to correct the SRS index than to keep the note in the HU. |

### Substantive Changes (Alignment with Technical Reality)
| Req. | Current Text | Proposed Text | Justification |
|:---|:---|:---|:---|
| **HU-IAM-001 (AC4)** | "Status: not implemented... no service reads them yet." | "Given that a user exceeds failed attempts, the system temporarily locks the account based on `max_login_attempts`." | Convert the "not implemented" note into an imperative requirement for the current Sprint. |
| **HU-ATT-001** | "Responsible service(s): Attendance service... proposed" | "Responsible service(s): `ms-attendance` and `ms-biometric`" | Remove the word "proposed". The service already exists in the repository (`05-ms-attendance`), only the logic is missing. |
| **HU-IAM-001 (AC7)** | "Implemented as platform logging + last_access update" | "The system records an audit event in the database and updates the user's `last_access` field." | Formalize the technical implementation in the acceptance criteria. |

---

## 4. Recommended Actions (Prioritized)

1. **CRITICAL PRIORITY: Implement Biometric Core.**
   The project is a "FaceAttend", but there is no facial recognition code. It is urgent to develop the integration with the biometric SDK (mentioned in ADR-014) and the `ms-biometric` endpoints.
2. **HIGH PRIORITY: Implement Justification Flow.**
   The justification controller already exists, but the logic for uploading files and the approval flow by the instructor is missing.
3. **MEDIUM PRIORITY: Complete Academic Module.**
   Implement the creation of "Cohorts" and "Environments", as without these, attendance has no context for registration.
4. **LOW PRIORITY: Performance Testing.**
   Execute load tests to validate that critical endpoints respond in $< 2$ seconds, thus complying with **NFR-001**.
