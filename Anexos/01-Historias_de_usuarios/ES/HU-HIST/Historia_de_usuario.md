# HU-HIST-001: Consulta de asistencias y ausencias por parámetros

## Historia

**Como** administrador, supervisor, instructor o estudiante  
**Quiero** consultar los registros de asistencia y ausencias utilizando filtros como fecha, cohorte, ambiente, instructor o persona  
**Para que** pueda obtener información precisa de la asistencia según mi rol y tomar decisiones académicas oportunas.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

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
- Esta historia no cubre la exportación de reportes. Eso se maneja en la [HU-REP-001](../HU-REP/user-stories.md#hu-rep-001-exportación-de-reportes).
- Esta historia no cubre los tableros (dashboards). Eso se maneja en la [HU-REP-002](../HU-REP/user-stories.md#hu-rep-002-tablero-analítico-basado-en-roles).
- Esta historia no cubre consultas específicas de retardos/ausencias por parámetros. Eso se maneja en la [HU-REP-004](../HU-REP/user-stories.md#hu-rep-004-consulta-de-retardos-y-ausencias-por-parámetros).
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
| Dependencias | [HU-IAM-001](../HU-IAM/user-stories.md#hu-iam-001-inicio-de-sesión-de-usuario): Inicio de sesión de usuario; [HU-IAM-003](../HU-IAM/user-stories.md#hu-iam-003-asignación-de-permisos-basada-en-roles): Asignación de permisos basada en roles; [HU-ATT-001](../HU-ATT/user-stories.md#hu-att-001-registro-de-asistencia-facial): Registro de asistencia facial; [HU-ACAD-005](../HU-ACAD/user-stories.md#hu-acad-005-asignar-bloques-de-tiempojornadas): Asignar bloques de tiempo/jornadas |

---

# HU-HIST-002: Historial de cambios del usuario

## Historia

**Como** administrador o supervisor  
**Quiero** consultar el historial de cambios de un usuario específico  
**Para que** pueda auditar las modificaciones realizadas a los datos de ese usuario a lo largo del tiempo y mantener la trazabilidad de las acciones administrativas.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

### Validación de la consulta

- [ ] **AC1:** Dado que un administrador o supervisor autenticado accede al módulo de historial de cambios del usuario, cuando el usuario proporciona los parámetros para identificar al usuario objetivo, entonces el sistema valida que el usuario objetivo exista y esté registrado en la aplicación.

- [ ] **AC2:** Dado que el usuario objetivo no existe en el sistema, cuando el administrador consulta el historial de cambios, entonces el sistema rechaza la operación y muestra un mensaje de validación claro.

- [ ] **AC3:** Dado que el usuario objetivo existe pero no tiene cambios registrados, cuando el administrador consulta el historial de cambios, entonces el sistema muestra un mensaje claro indicando que no se han registrado cambios para ese usuario.

### Visualización del historial

- [ ] **AC4:** Dado que el usuario objetivo tiene cambios registrados, cuando se muestra el historial, entonces el sistema muestra cada cambio con al menos la siguiente información: fecha y hora del cambio, tipo de cambio, el usuario que realizó el cambio, valor anterior (si aplica) y valor nuevo (si aplica).

- [ ] **AC5:** Dado que el historial de cambios contiene datos sensibles, cuando se muestra el historial, entonces el sistema protege los campos sensibles (como contraseñas o parámetros biométricos) y no expone sus valores reales en la interfaz o los logs.

- [ ] **AC6:** Dado que se muestra el historial de cambios, cuando el administrador interactúa con los resultados, entonces el sistema soporta el filtrado por usuario objetivo, rango de fechas, tipo de cambio y usuario que realizó el cambio.

### Control de acceso

- [ ] **AC7:** Dado que el usuario que realiza la consulta no tiene permisos de administrador o supervisor, cuando el usuario intenta acceder al módulo de historial de cambios del usuario, entonces el sistema deniega el acceso y registra un intento de acceso no autorizado.

- [ ] **AC8:** Dado que un usuario regular, instructor o estudiante intenta acceder al historial de cambios de otro usuario, cuando se procesa la solicitud, entonces el sistema deniega el acceso y aplica las restricciones basadas en roles.

### Integración de auditoría

- [ ] **AC9:** Dado que los datos de un usuario se modifican en cualquier otro módulo (registro de usuario, cambio de rol, activación/desactivación, actualización de parámetros faciales, etc.), cuando se guarda la modificación, entonces el sistema registra el cambio en el historial de cambios del usuario automáticamente.

- [ ] **AC10:** Dado que se consulta el historial de cambios, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el usuario que consulta, el usuario objetivo, la fecha y la hora.

### Rendimiento

- [ ] **AC11:** Dadas condiciones normales de red, cuando el administrador consulta el historial de cambios del usuario, entonces el sistema responde en menos de 5 segundos, de acuerdo con el RNF 1.

---

## Notas Técnicas

- El RF 5.2 indica que el sistema debe validar:
  - Que el usuario que realiza la consulta esté registrado en el sistema.
  - Que el usuario objetivo/filtrado exista.
- Esta historia depende de que se generen eventos de auditoría por otros módulos:
  - [HU-IAM-001](../HU-IAM/user-stories.md#hu-iam-001-inicio-de-sesión-de-usuario): Eventos de inicio de sesión.
  - [HU-IAM-002](../HU-IAM/user-stories.md#hu-iam-002-registro-de-usuario-con-asignación-de-rol): Eventos de registro de usuario y asignación de roles.
  - [HU-IAM-007](../Index_HU.md#historias-planificadas-pendientes-de-documentar): Eventos de activación/desactivación de usuarios.
  - [HU-IAM-010](../Index_HU.md#historias-planificadas-pendientes-de-documentar): Eventos de cambio de rol.
  - [HU-ATT-002](../HU-ATT/user-stories.md#hu-att-002-inscripción-inicial-de-parámetros-faciales): Eventos de inscripción de parámetros faciales.
  - [HU-CONF-006](../HU-CONF/user-stories.md#hu-conf-006-actualizar-parámetros-faciales-del-usuario): Eventos de actualización de parámetros faciales.
- El historial de cambios debe ser inmutable: una vez registrado, las entradas del historial no deben modificarse ni eliminarse.
- Los campos sensibles como contraseñas o parámetros biométricos no deben exponer sus valores reales en el historial de cambios. Solo debe registrarse el hecho de que ocurrió un cambio.
- El historial de cambios debe soportar el filtrado por:
  - Usuario objetivo.
  - Rango de fechas.
  - Tipo de cambio.
  - Usuario que realizó el cambio.
- Se debe considerar la paginación para usuarios con muchos cambios.
- Esta historia no cubre el historial de asistencia. Eso se maneja en la [HU-HIST-001](#hu-hist-001-consulta-de-asistencias-y-ausencias-por-parámetros).
- Esta historia no cubre la auditoría general de todas las acciones del sistema. Se enfoca específicamente en los cambios de datos de usuario.
- El equipo debe definir el período de retención de los registros del historial de cambios, considerando las políticas institucionales y las regulaciones de protección de datos.

**Servicio(s) responsable(s):** servicio-asistencia (HIST-001) / servicio-identidad a través del registro de la plataforma (HIST-002).  
**Endpoint(s) implementados:**  
- `GET /api/v1/users/{userId}/change-history` — propuesto.  
- `GET /api/v1/users/{userId}/change-history` con parámetros de consulta para rango de fechas, tipo de cambio y autor — propuesto.  

**Eventos generados:**  
- `UserChangeHistoryQueried` — recomendado para fines de auditoría.  
- `UnauthorizedChangeHistoryAccessAttempt`  

**Permisos requeridos:**  
- Administrador o Supervisor.
- Los usuarios regulares no deben poder acceder al historial de cambios de otros usuarios.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Consulta exitosa del historial de cambios probada.
- [ ] Escenario de usuario objetivo inexistente probado.
- [ ] Escenario de usuario objetivo sin cambios probado.
- [ ] El historial de cambios incluye los campos requeridos: fecha, hora, tipo, autor, valor anterior, valor nuevo.
- [ ] El historial de cambios se registra automáticamente cuando se modifican los datos del usuario en otros módulos.
- [ ] Intento de acceso no autorizado probado.
- [ ] Los campos sensibles están protegidos y no se exponen.
- [ ] El historial de cambios es inmutable.
- [ ] Filtrado por rango de fechas, tipo de cambio y autor probado.
- [ ] Se genera un evento de auditoría para las consultas del historial.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se ha validado bajo condiciones normales y se mantiene por debajo de los 5 segundos.
- [ ] La trazabilidad al RF 5.2 está documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Media |
| Sprint objetivo | Sprint 3 |
| Dependencias | [HU-IAM-001](../HU-IAM/user-stories.md#hu-iam-001-inicio-de-sesión-de-usuario): Inicio de sesión de usuario; [HU-IAM-003](../HU-IAM/user-stories.md#hu-iam-003-asignación-de-permisos-basada-en-roles): Asignación de permisos basada en roles; [HU-IAM-002](../HU-IAM/user-stories.md#hu-iam-002-registro-de-usuario-con-asignación-de-rol): Registro de usuario con asignación de rol; [HU-IAM-007](../Index_HU.md#historias-planificadas-pendientes-de-documentar): Activar/desactivar usuarios; [HU-IAM-010](../Index_HU.md#historias-planificadas-pendientes-de-documentar): Cambio de rol |
