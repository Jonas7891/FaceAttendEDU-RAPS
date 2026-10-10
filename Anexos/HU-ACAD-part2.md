conference
# HU-ACAD-005: Assign conference/time blocks

## Story

**As** an administrator or supervisor  
**I want** to assign  or time blocks to a registered cohort with a valid responsible instructor  
**So that** the system knows the schedule in which students must attend and register their attendance.

---

## Acceptance criteria

> Format: “Given [context/initial state], when [user action], then [expected and verifiable result].”

- [ ] **AC1:** Given that an administrator or supervisor is authenticated in the system, when the user assigns a time block to a registered cohort with a valid responsible instructor, then the system associates the time block with the cohort and responsible user and shows a confirmation message.

- [ ] **AC2:** Given that the selected responsible user is not valid or is not registered, when the time block assignment is submitted, then the system rejects the operation and shows a validation error.

- [ ] **AC3:** Given that the proposed time block overlaps with another time block already assigned, when the assignment is submitted, then the system rejects the operation and shows a clear conflict message.

- [ ] **AC4:** Given that the cohort is not available for the proposed time block, when the assignment is submitted, then the system rejects the operation and shows a validation error.

- [ ] **AC5:** Given that a jornada/time block is successfully assigned, when attendance registration occurs for that cohort, then the system can identify the expected time in which students must register entry and exit.

- [ ] **AC6:** Given that users need to consult schedules, when an administrator, instructor, or student requests the corresponding schedule, then the system displays the schedules where users must attend and register, according to their role.

- [ ] **AC7:** Given normal network conditions, when the administrator assigns or consults conference/time blocks, then the system responds in less than 5 seconds, according to RNF 1.

- [ ] **AC8:** Given that a jornada/time block is assigned, updated, or rejected, when the operation is completed, then the system records an audit event containing the action, user, cohort, responsible user, time block, date, and time.

---

## Technical notes

- The SRS refers to “asignar conference” and “tiempo bloque de laburo”. For consistency, this story uses “jornada” or “time block” as the period during which a cohort must attend with a responsible instructor.
- The system must validate that:
  - The responsible user is valid.
  - The time block does not overlap with another already assigned block.
  - The cohort has availability for the assigned time block.
- This story is essential for attendance validation because HU-ATT-001 depends on knowing whether entry and exit times are appropriate for the assigned block.
- The schedule/jornada relationship will be used by attendance registration, late detection, absence detection, history, reports, and dashboards.
- The system should prevent duplicate or conflicting schedules for the same cohort, responsible user, environment, or time period.
- The team must define whether overlap validation applies only to the same cohort, to the same responsible instructor, to the same environment, or to a combination of these.
- Role-based access must be enforced:
  - Administrators and supervisors can assign and consult schedules.
  - Instructors can consult schedules assigned to them.
  - Students can consult schedules where they are registered, if applicable.

**Responsible service(s):** Academic Structure service / Scheduling service, proposed.  
**Endpoint(s) implemented:**  
- `POST /api/v1/schedules` — proposed.  
- `GET /api/v1/schedules` — proposed.  
- `GET /api/v1/cohorts/{cohortId}/schedule` — proposed.  
- `GET /api/v1/users/me/schedule` — proposed.  

**Events generated:**  
- `ScheduleAssigned`  
- `ScheduleAssignmentFailed`  
- `ScheduleConflictDetected`  

**Required permissions:**  
- Administrator or Supervisor for assigning conference/time blocks.
- Teacher/Instructor may consult schedules assigned to them.  
- Student may consult allowed schedules if applicable.

---

## Definition of Done (DoD)

> This HU can only be closed when it meets the team's full DoD.

**Additional checks specific to this HU:**

- [ ] Successful jornada/time block assignment tested.
- [ ] Invalid responsible user scenario tested.
- [ ] Time block overlap scenario tested.
- [ ] Cohort unavailable scenario tested.
- [ ] Assigned time block is correctly associated with cohort and responsible user.
- [ ] Schedule consultation tested for administrator, instructor, and student where applicable.
- [ ] Attendance module can use the assigned time block for entry/exit validation.
- [ ] Audit event is generated.
- [ ] Confirmation and error messages are clear.
- [ ] Response time is validated under normal conditions and remains under 5 seconds.
- [ ] Traceability to RF 2.3.2 is documented.

---

## Estimation and priority

| Field | Value |
|---|---|
| Story Points | 8 |
| Priority | High |
| Target sprint | Sprint 1 / MVP |
| Dependencies | HU-IAM-001: User login; HU-ACAD-003: Assign responsible instructors; HU-ACAD-004: Assign cohorts to responsible users |