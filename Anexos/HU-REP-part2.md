  - Instructors: records from assigned cohorts/environments/training blocks.
  - Administrators/Supervisors: broader administrative scope.
- The query results should include justification status to help users identify which delays/absences have been justified.
- Pagination should be considered for large result sets.
- The team should consider:
  - Sorting options (by date, user, type).
  - Export option for query results, if applicable.
  - Integration with HU-REP-001 for exporting query results.
- This story does not cover the analytical dashboard. That is handled in HU-REP-002.
- This story does not cover general attendance/absence history queries. That is handled in HU-HIST-001.
- This story focuses specifically on delays and absences, not on all attendance records.
- Attendance data must be protected according to RNF 4, and access must be restricted according to the user's role.

**Responsible service(s):** Report service / Delay-Absence query service, proposed.  
**Endpoint(s) implemented:**  
- `GET /api/v1/reports/delays-absences` — proposed, with query parameters for user, date, cohort, environment, and instructor.  
- `GET /api/v1/reports/delays-absences/me` — proposed, for students querying their own records.  

**Events generated:**  
- `DelayAbsenceQueryExecuted` — recommended for audit purposes.  
- `UnauthorizedDelayAbsenceQueryAttempt`  

**Required permissions:**  
- Authenticated user.  
- Role-based scope enforcement:
  - Student: own records only.
  - Instructor: assigned cohorts, environments, or training blocks.
  - Administrator/Supervisor: administrative scope.

---

## Definition of Done (DoD)

> This HU can only be closed when it meets the team's full DoD.

**Additional checks specific to this HU:**

- [ ] Query by user tested.
- [ ] Query by date tested.
- [ ] Query by cohort/group tested.
- [ ] Query by environment/classroom tested.
- [ ] Query by instructor tested.
- [ ] Combined filters tested.
- [ ] Empty result scenario tested.
- [ ] Role-based filtering tested for student, instructor, administrator, and supervisor.
- [ ] Unauthorized access attempt tested.
- [ ] Returned records include required fields: user, date, type, cohort, environment, instructor, justification status.
- [ ] Pagination tested for large result sets.
- [ ] Audit event is generated.
- [ ] Confirmation and error messages are clear.
- [ ] Response time is validated under normal conditions and remains under 5 seconds.
- [ ] Traceability to RF 7.4 is documented.

---

## Estimation and priority

| Field | Value |
|---|---|
| Story Points | 5 |
| Priority | High |
| Target sprint | Sprint 3 |
| Dependencies | HU-IAM-001: User login; HU-IAM-003: Role-based permission assignment; HU-HIST-001: Attendance and absence query by parameters; HU-CONF-008: Justification parametrization |