# HU-HIST-001: Consulta de asistencia y ausencias por parámetros

## Historia

**Como** administrador, supervisor, instructor o estudiante  
**Quiero** consultar los registros de asistencia y ausencias utilizando filtros como fecha, cohorte, ambiente, instructor o persona  
**Para que** pueda obtener información precisa de la asistencia según mi rol y tomar decisiones académicas oportunas.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

### Validación de parámetros

- [ ] **AC1:** Dado que un usuario autenticado accede al módulo de historial de asistencia, cuando el usuario selecciona uno o más parámetros de consulta (fecha, cohorte, ambiente, instructor o persona), entonces el sistema valida cada parámetro antes de procesar la consulta.

- [ ] **AC2:** Dado que el usuario selecciona un parámetro de fecha, cuando se ejecuta la consulta, entonces el sistema valida que la fecha sea válida y aplicable al contexto de control de asistencia antes de devolver resultados.

- [ ] **AC3:** Dado que el usuario selecciona una cohorte/ficha como filtro, cuando se ejecuta la consulta, entonces el sistema valida que la cohorte esté registrada en el sistema antes de devolver resultados.

- [ ] **AC4:** Dado que el usuario selecciona un ambiente como filtro, cuando se ejecuta la consulta, entonces el sistema valida que el ambiente esté registrado en el sistema antes de devolver resultados.

- [ ] **AC5:** Dado que el usuario selecciona un instructor como filtro, cuando se ejecuta la consulta, entonces el sistema valida que el instructor/docente esté registrado en el sistema antes de devolver resultados.

- [ ] **AC6:** Dado que el usuario selecciona una persona como filtro, cuando se ejecuta la consulta, entonces el sistema valida que la persona esté registrada en el sistema antes de devolver resultados.

### Ejecución de la consulta

- [ ] **AC7:** Dado que todos los parámetros seleccionados son válidos, cuando se ejecuta la consulta, entonces el sistema devuelve los registros de asistencia y ausencias que coinciden con los filtros solicitados.

- [ ] **AC8:** Dado que los parámetros seleccionados son válidos pero no producen registros coincidentes, cuando se ejecuta la consulta, entonces el sistema muestra un mensaje claro indicando que no se encontraron registros de asistencia para los filtros especificados.

- [ ] **AC9:** Dado que el usuario combina múltiples filtros, cuando se ejecuta la consulta, entonces el sistema aplica todos los filtros simultáneamente y devuelve solo los registros que coinciden con todos los criterios.

### Acceso basado en roles

- [ ] **AC10:** Dado que el usuario es un estudiante, cuando el usuario consulta los registros de asistencia, entonces el sistema solo devuelve los registros pertenecientes a ese estudiante, a menos que se le otorgue explícitamente un permiso más amplio.

- [ ] **AC11:** Dado que el usuario es un instructor, cuando el usuario consulta los registros de asistencia, entonces el sistema solo devuelve los registros asociados con las cohortes, ambientes o bloques de formación asignados a ese instructor.

- [ ] **AC12:** Dado que el usuario es un administrador o supervisor, cuando el usuario consulta los registros de asistencia, entonces el sistema devuelve los registros de acuerdo con el alcance administrativo asignado a ese rol.

- [ ] **AC13:** Dado que un usuario no autorizado intenta consultar los registros de asistencia fuera del alcance de su rol, cuando se procesa la solicitud, entonces el sistema deniega el acceso y registra un intento de acceso no autorizado.

### Visualización de resultados

- [ ] **AC14:** Dado que una consulta devuelve resultados, cuando el sistema los muestra, entonces los registros incluyen al menos la siguiente información: usuario, fecha, hora de entrada, hora de salida, estado de asistencia (presente, ausente, retardado, justificado), cohorte, ambiente e instructor, según corresponda.

- [ ] **AC15:** Dado que una consulta devuelve un gran número de resultados, cuando el sistema los muestra, entonces los resultados se paginan para mantener el rendimiento y la usabilidad.

### Rendimiento y auditoría

- [ ] **AC16:** Dadas condiciones normales de red, cuando el usuario ejecuta una consulta de asistencia, entonces el sistema responde en menos de 5 segundos, de acuerdo con el RNF 1.

- [ ] **AC17:** Dado que la consulta se ejecuta, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el usuario, los parámetros aplicados, la fecha y la hora.

---

## Notas Técnicas

- El RF 5.1 indica que el sistema debe validar los siguientes parámetros antes de devolver resultados:
  - El usuario está registrado en el sistema.
  - La cohorte/ficha está registrada en el sistema.
  - El ambiente está registrado en el sistema.
  - El instructor/docente está registrado en el sistema.
  - La fecha es válida y aplicable.
- Los parámetros de consulta definidos por el SRS son: fecha, cohorte/ficha, ambiente, instructor y persona.
- El filtrado basado en roles debe aplicarse en el lado del servidor:
  - Estudiantes: solo sus propios registros.
  - Instructores: registros de cohortes/ambientes/bloques de formación asignados.
  - Administradores/Supervisores: alcance administrativo más amplio.
- Los valores del estado de asistencia deben alinearse con el modelo de dominio definido en `entities-and-rules.md`:
  - Presente.
  - Ausente.
  - Retardado.
  - Justificado.
- Se debe considerar la paginación para conjuntos de resultados grandes.
- El sistema debe considerar estrategias de almacenamiento en caché para reportes accedidos frecuentemente, si es aplicable.
- Esta historia no cubre la exportación de reportes. Eso se maneja en la HU-REP-001.
- Esta historia no cubre los tableros (dashboards). Eso se maneja en la HU-REP-002.
- Esta historia no cubre consultas específicas de retardos/ausencias por parámetros. Eso se maneja en la HU-REP-004.
- Los datos de asistencia deben protegerse de acuerdo con el RNF 4, y el acceso debe restringirse según el rol del usuario.

**Servicio(s) responsable(s):** Servicio de Historial / Servicio de Consulta de Asistencia, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/attendance/history` — propuesto, con parámetros de consulta para fecha, cohorte, ambiente, instructor y persona.  
- `GET /api/v1/attendance/history/me` — propuesto, para que los estudiantes consulten sus propios registros.  

**Eventos generados:**  
- `AttendanceHistoryQueried` — recomendado para fines de auditoría.  
- `UnauthorizedAttendanceQueryAttempt`  

**Permisos requeridos:**  
- Usuario autenticado.  
- Aplicación del alcance basado en roles:
  - Estudiante: solo sus propios registros.
  - Instructor: cohortes, ambientes o bloques de formación asignados.
  - Administrador/Supervisor: alcance administrativo.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Consulta por fecha probada.
- [ ] Consulta por cohorte probada.
- [ ] Consulta por ambiente probada.
- [ ] Consulta por instructor probada.
- [ ] Consulta por persona probada.
- [ ] Filtros combinados probados.
- [ ] Escenario de resultados vacíos probado.
- [ ] Escenario de fecha inválida probado.
- [ ] Escenario de entidad inexistente probado.
- [ ] Filtrado basado en roles probado para estudiante, instructor, administrador y supervisor.
- [ ] Intento de acceso no autorizado probado.
- [ ] Los registros devueltos incluyen los campos requeridos.
- [ ] Paginación probada para conjuntos de resultados grandes.
- [ ] Se genera un evento de auditoría.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se ha validado bajo condiciones normales y se mantiene por debajo de los 5 segundos.
- [ ] La trazabilidad al RF 5.1 está documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 8 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 2 |
| Dependencias | HU-IAM-001: Inicio de sesión de usuario; HU-IAM-003: Asignación de permisos basados en roles; HU-ATT-001: Registro de asistencia facial; HU-ACAD-005: Asignación de jornadas/bloques de tiempo |
