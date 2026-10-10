# HU-ACAD-001: Registrar ambientes/salones

## Historia

**Como** administrador o supervisor  
**Quiero** registrar ambientes o salones  
**Para que** los registros de asistencia puedan asociarse con el espacio de formación correcto donde los estudiantes asisten a clase.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

- [ ] **AC1:** Dado que un administrador o supervisor está autenticado en el sistema, cuando el usuario envía identificadores de ambiente válidos, entonces el sistema valida los datos, registra el ambiente/salón, almacena la información de forma segura y muestra un mensaje de confirmación.

- [ ] **AC2:** Dado que los datos del ambiente están incompletos o son inválidos, cuando se envía el registro, entonces el sistema rechaza la operación y muestra mensajes de validación específicos.

- [ ] **AC3:** Dado que ya existe un ambiente con el mismo identificador único, cuando el administrador intenta registrarlo nuevamente, entonces el sistema evita el registro duplicado y muestra un mensaje de error claro.

- [ ] **AC4:** Dado que un ambiente se registró exitosamente, cuando el sistema requiera un ambiente para el registro de asistencia o reportes, entonces el ambiente registrado está disponible para selección.

- [ ] **AC5:** Dadas condiciones normales de red, cuando el administrador registra un ambiente, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

- [ ] **AC6:** Dado que un ambiente es registrado, cuando la operación se completa, entonces el sistema registra un evento de auditoría que contiene la acción, usuario, fecha y hora.

---

## Notas Técnicas

- El SRS se refiere a los ambientes como “ambientes/salones”, que son los espacios físicos o virtuales donde se registra la asistencia.
- Los datos del ambiente deben ser validados antes de la persistencia, según los criterios de aceptación del RF 2.1.
- La información debe almacenarse de forma segura, según el RF 2.1 y el RNF 4.
- Esta historia no incluye la asignación de horarios, cohortes o instructores. Esas relaciones se manejan en otras historias de ACAD.
- La entidad de ambiente será utilizada posteriormente por el registro de asistencia, reportes y dashboards.
- La interfaz de usuario debe permitir la consulta de ambientes registrados después de su creación.
- Se recomienda la auditabilidad para los cambios administrativos.

**Servicio(s) responsable(s):** Servicio de Estructura Académica, propuesto.  
**Endpoint(s) implementados:**  
- `POST /api/v1/environments` — propuesto.  
- `GET /api/v1/environments` — propuesto.  

**Eventos generados:**  
- `EnvironmentRegistered`  
- `EnvironmentRegistrationFailed`  

**Permisos requeridos:**  
- Administrador o Supervisor.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Registro de ambiente exitoso probado.
- [ ] Datos de ambiente inválidos o incompletos probados.
- [ ] Escenario de ambiente duplicado probado.
- [ ] Ambiente registrado disponible para selección en flujos académicos.
- [ ] Mensajes de confirmación y error son claros.
- [ ] Evento de auditoría generado.
- [ ] Tiempo de respuesta validado bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Datos almacenados de forma segura.
- [ ] Trazabilidad al RF 2.1 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 3 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 1 / MVP |
| Dependencias | HU-IAM-001: Inicio de sesión; HU-IAM-003: Asignación de permisos basada en roles |

---

# HU-ACAD-002: Registrar cohortes/cursos

## Historia

**Como** administrador o supervisor  
**Quiero** registrar cohortes o cursos, conocidos como “fichas” en el contexto del SENA  
**Para que** los grupos de estudiantes puedan asociarse con el control de asistencia, instructores responsables y horarios de formación.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

- [ ] **AC1:** Dado que un administrador o supervisor está autenticado en el sistema, cuando el usuario envía identificadores de cohorte/curso válidos, entonces el sistema valida los datos, registra la cohorte/curso asociada con el control de asistencia, almacena la información de forma segura y muestra un mensaje de confirmación.

- [ ] **AC2:** Dado que los datos de la cohorte/curso están incompletos o son inválidos, cuando se envía el registro, entonces el sistema rechaza la operación y muestra mensajes de validación específicos.

- [ ] **AC3:** Dado que ya existe una cohorte/curso con el mismo identificador único, cuando el administrador intenta registrarla nuevamente, entonces el sistema evita el registro duplicado y muestra un mensaje de error claro.

- [ ] **AC4:** Dado que una cohorte/curso se registró exitosamente, cuando se realiza el registro de asistencia o la asignación académica, entonces el sistema puede mostrar o utilizar la cohorte/curso asociada con el usuario o grupo de formación correspondiente, según el RF 2.2.

- [ ] **AC5:** Dado que una cohorte/curso se registró exitosamente, cuando el administrador asigna instructores responsables u horarios, entonces la cohorte/curso está disponible para esas relaciones.

- [ ] **AC6:** Dadas condiciones normales de red, cuando el administrador registra una cohorte/curso, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

- [ ] **AC7:** Dado que una cohorte/curso es registrada, cuando la operación se completa, entonces el sistema registra un evento de auditoría que contiene la acción, usuario, fecha y hora.

---

## Notas Técnicas

- El SRS utiliza los términos “fichas/cursos”. El equipo debe definir si una “ficha” se trata como una cohorte, grupo o curso en el modelo de dominio final.
- Esta historia cubre el registro del grupo/cohorte, no la asignación de estudiantes, instructores u horarios.
- La cohorte/curso registrada debe estar disponible para la posterior asignación de usuarios responsables, según el RF 2.3.1.
- La entidad de cohorte/curso será utilizada por el registro de asistencia, consultas de historial, justificaciones y reportes.
- Las reglas de validación deben incluir identificadores requeridos y contexto institucional.
- No se esperan datos sensibles en esta entidad, pero los cambios administrativos deben ser auditables.
- El sistema debe almacenar los datos de forma segura, según el RF 2.2 y el RNF 4.

**Servicio(s) responsable(s):** Servicio de Estructura Académica, propuesto.  
**Endpoint(s) implementados:**  
- `POST /api/v1/cohorts` — propuesto, representando las “fichas” del SENA.  
- `GET /api/v1/cohorts` — propuesto.  

**Eventos generados:**  
- `CohortRegistered`  
- `CohortRegistrationFailed`  

**Permisos requeridos:**  
- Administrador o Supervisor.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Registro de cohorte/curso exitoso probado.
- [ ] Datos de cohorte/curso inválidos o incompletos probados.
- [ ] Escenario de cohorte/curso duplicado probado.
- [ ] Cohorte/curso registrada disponible para asignación de instructores.
- [ ] Cohorte/curso registrada disponible para flujos relacionados con la asistencia.
- [ ] Mensajes de confirmación y error son claros.
- [ ] Evento de auditoría generado.
- [ ] Tiempo de respuesta validado bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Datos almacenados de forma segura.
- [ ] Trazabilidad al RF 2.2 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 3 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 1 / MVP |
| Dependencias | HU-IAM-001: Inicio de sesión; HU-IAM-003: Asignación de permisos basada en roles; HU-ACAD-001 recomendado para contexto de ambiente |

---

# HU-ACAD-003: Asignar instructores responsables

## Historia

**Como** administrador o supervisor  
**Quiero** asignar a un profesor/instructor como el usuario responsable de una cohorte o bloque de formación  
**Para que** los registros de asistencia estén vinculados al instructor correcto y los instructores puedan ver los grupos asignados a ellos.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

- [ ] **AC1:** Dado que un administrador o supervisor ha iniciado sesión en FaceAttend EDU y los usuarios están registrados en el sistema, cuando el usuario accede a la función de asignación de responsables, entonces el sistema muestra la lista de usuarios asociados con el administrador y la lista de usuarios de tipo profesor/instructor, según el RF 2.3.

- [ ] **AC2:** Dado que se selecciona un profesor/instructor válido y una cohorte/curso registrada, cuando el administrador asigna el usuario responsable, entonces el sistema asocia al instructor con la cohorte/curso o bloque de formación y muestra un mensaje de confirmación.

- [ ] **AC3:** Dado que el usuario seleccionado no es un usuario válido o no es de tipo profesor/instructor, cuando el administrador intenta la asignación, entonces el sistema rechaza la operación y muestra un error de validación.

- [ ] **AC4:** Dado que la cohorte/curso no existe en el sistema, cuando el administrador intenta asignar un usuario responsable, entonces el sistema rechaza la operación y muestra un error de validación.

- [ ] **AC5:** Dado que un instructor responsable fue asignado exitosamente, cuando se registra la asistencia para la cohorte o bloque de formación correspondiente, entonces el sistema puede identificar al instructor responsable de ese registro de asistencia.

- [ ] **AC6:** Dadas condiciones normales de red, cuando el administrador asigna un instructor responsable, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

- [ ] **AC7:** Dado que un instructor responsable es asignado o actualizado, cuando la operación se completa, entonces el sistema registra un evento de auditoría que contiene la acción, administrador, instructor, cohorte/curso, fecha y hora.

---

## Notas Técnicas

- El SRS describe este requisito como la obtención del profesor/instructor responsable del bloque de formación correspondiente al registro de asistencia.
- La asignación debe requerir inicio de sesión previo y registro previo en FaceAttend EDU, según los criterios de aceptación del RF 2.3.
- El sistema debe mostrar ambos:
  - Usuarios asociados con el administrador.
  - Usuarios de tipo profesor/instructor.
- Esta historia aún no incluye la asignación de bloques de tiempo u horarios. Eso se cubre en la HU-ACAD-005.
- La relación de instructor responsable será utilizada por la asistencia, historial, reportes y dashboards.
- El sistema debe evitar asignar responsabilidad a usuarios sin un rol de instructor/profesor.
- Se recomienda la auditabilidad porque esta asignación afecta la visibilidad académica y los permisos de reportes.
- El equipo debe definir si una cohorte/curso puede tener uno o múltiples instructores responsables.

**Servicio(s) responsable(s):** Servicio de Estructura Académica, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/users?role=instructor` — propuesto.  
- `POST /api/v1/cohorts/{cohortId}/responsible` — propuesto.  
- `GET /api/v1/cohorts/{cohortId}/responsible` — propuesto.  

**Eventos generados:**  
- `ResponsibleAssigned`  
- `ResponsibleAssignmentFailed`  

**Permisos requeridos:**  
- Administrador o Supervisor.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Inicio de sesión del administrador probado antes de la asignación.
- [ ] Visualización de la lista de usuarios instructores probada.
- [ ] Visualización de usuarios asociados con el administrador probada.
- [ ] Asignación exitosa de instructor responsable probada.
- [ ] Escenario de instructor inválido probado.
- [ ] Escenario de cohorte/curso inexistente probado.
- [ ] Instructor asignador vinculado correctamente a la cohorte/curso o bloque de formación.
- [ ] Evento de auditoría generado.
- [ ] Mensajes de confirmación y error son claros.
- [ ] Tiempo de respuesta validado bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Trazabilidad al RF 2.3 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 1 / MVP |
| Dependencias | HU-IAM-001: Inicio de sesión; HU-IAM-002: Registro de usuario con asignación de rol; HU-ACAD-002: Registrar cohortes/cursos |

---

# HU-ACAD-004: Asignar cohortes a usuarios responsables

## Historia

**Como** administrador o supervisor  
**Quiero** asignar cohortes/cursos registrados, conocidos como “fichas”, a un usuario responsable válido  
**Para que** los usuarios administrativos, instructores y usuarios responsables puedan ver y gestionar las cohortes asignadas a su responsabilidad académica.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

- [ ] **AC1:** Dado que un administrador o supervisor está autenticado en el sistema, cuando el usuario selecciona un usuario responsable válido y una cohorte/curso existente, entonces el sistema asocia la cohorte/curso con el usuario responsable y muestra un mensaje de confirmación.

- [ ] **AC2:** Dado que el usuario responsable seleccionado no es un usuario registrado válido, cuando se envía la asignación, entonces el sistema rechaza la operación y muestra un error de validación.

- [ ] **AC3:** Dado que la cohorte/curso seleccionada no existe en el sistema, cuando se envía la asignación, entonces el sistema rechaza la operación y muestra un error de validación.

- [ ] **AC4:** Dado que un usuario responsable tiene cohortes/cursos asignados, cuando el usuario solicita la lista de cohortes bajo su responsabilidad, entonces el sistema muestra las cohortes correspondientes a su responsabilidad asignada, según el RF 2.3.1.

- [ ] **AC5:** Dado que un usuario administrativo, supervisor, administrador o profesor/instructor accede al módulo de asignación de cohortes, cuando el sistema carga la información, entonces el usuario ve solo las cohortes permitidas por su rol.

- [ ] **AC6:** Dadas condiciones normales de red, cuando el administrador asigna o consulta cohortes para un usuario responsable, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

- [ ] **AC7:** Dado que una cohorte/curso es asignada a un usuario responsable, cuando la operación se completa, entonces el sistema registra un evento de auditoría que contiene la acción, usuario, usuario responsable, cohorte/curso, fecha y hora.

---

## Notas Técnicas

- El SRS indica que los usuarios administrativos (Supervisor, Administrador y Profesor/Instructor) deben poder obtener la lista de cohortes correspondientes a su trabajo.
- El sistema debe validar que el usuario responsable sea válido y que la cohorte/curso exista, según los criterios de aceptación del RF 2.3.1.
- Esta historia complementa la HU-ACAD-003 al formalizar la visibilidad y asociación de cohortes asignadas a un usuario responsable.
- La relación responsable-cohorte será utilizada por el registro de asistencia, historial, reportes, dashboards y permisos de instructor.
- El sistema debe definir si un usuario responsable puede tener una o múltiples cohortes asignadas.
- El sistema debe evitar asignar usuarios inactivos como usuarios responsables.
- El acceso basado en roles debe aplicarse en el lado del servidor.
- Se recomienda la auditabilidad porque esta asignación afecta la visibilidad académica y los permisos de reportes.

**Servicio(s) responsable(s):** Servicio de Estructura Académica, propuesto.  
**Endpoint(s) implementados:**  
- `POST /api/v1/responsibles/{responsibleId}/cohorts` — propuesto.  
- `GET /api/v1/responsibles/{responsibleId}/cohorts` — propuesto.  
- `GET /api/v1/users/me/cohorts` — propuesto.  

**Eventos generados:**  
- `CohortAssignedToResponsible`  
- `CohortAssignmentFailed`  

**Permisos requeridos:**  
- Administrador o Supervisor para asignar cohortes a usuarios responsables.  
- Profesor/Instructor puede consultar sus propias cohortes asignadas.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Asignación exitosa de cohorte a un usuario responsable probada.
- [ ] Escenario de usuario responsable inválido probado.
- [ ] Escenario de cohorte/curso inexistente probado.
- [ ] Consulta de cohortes asignadas probada.
- [ ] Visibilidad basada en roles probada para administrador, supervisor e instructor.
- [ ] Evento de auditoría generado.
- [ ] Mensajes de confirmación y error son claros.
- [ ] Tiempo de respuesta validado bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Trazabilidad al RF 2.3.1 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 1 / MVP |
| Dependencias | HU-IAM-001: Inicio de sesión; HU-ACAD-002: Registrar cohortes/cursos; HU-ACAD-003: Asignar instructores responsables |

---

# HU-ACAD-005: Asignar bloques de tiempo/jornadas

## Historia

**Como** administrador o supervisor  
**Quiero** asignar bloques de tiempo o jornadas a una cohorte registrada con un instructor responsable válido  
**Para que** el sistema conozca el horario en el cual los estudiantes deben asistir y registrar su asistencia.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

- [ ] **AC1:** Dado que un administrador o supervisor está autenticado en el sistema, cuando el usuario asigna un bloque de tiempo a una cohorte registrada con un instructor responsable válido, entonces el sistema asocia el bloque de tiempo con la cohorte y el usuario responsable y muestra un mensaje de confirmación.

- [ ] **AC2:** Dado que el usuario responsable seleccionado no es válido o no está registrado, cuando se envía la asignación del bloque de tiempo, entonces el sistema rechaza la operación y muestra un error de validación.

- [ ] **AC3:** Dado que el bloque de tiempo propuesto se solapa con otro bloque de tiempo ya asignado, cuando se envía la asignación, entonces el sistema rechaza la operación y muestra un mensaje de conflicto claro.

- [ ] **AC4:** Dado que la cohorte no está disponible para el bloque de tiempo propuesto, cuando se envía la asignación, entonces el sistema rechaza la operación y muestra un error de validación.

- [ ] **AC5:** Dado que una jornada/bloque de tiempo fue asignada exitosamente, cuando ocurre el registro de asistencia para esa cohorte, entonces el sistema puede identificar el tiempo esperado en el cual los estudiantes deben registrar entrada y salida.

- [ ] **AC6:** Dado que los usuarios necesitan consultar horarios, cuando un administrador, instructor o estudiante solicita el horario correspondiente, entonces el sistema muestra los horarios donde los usuarios deben asistir y registrarse, según su rol.

- [ ] **AC7:** Dadas condiciones normales de red, cuando el administrador asigna o consulta bloques de tiempo/jornadas, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

- [ ] **AC8:** Dado que una jornada/bloque de tiempo es asignada, actualizada o rechazada, cuando la operación se completa, entonces el sistema registra un evento de auditoría que contiene la acción, usuario, cohorte, usuario responsable, bloque de tiempo, fecha y hora.

---

## Notas Técnicas

- El SRS se refiere a “asignar conference” y “tiempo bloque de laburo”. Por consistencia, esta historia utiliza “jornada” o “bloque de tiempo” como el período durante el cual una cohorte debe asistir con un instructor responsable.
- El sistema debe validar que:
  - El usuario responsable sea válido.
  - El bloque de tiempo no se solape con otro bloque ya asignado.
  - La cohorte tenga disponibilidad para el bloque de tiempo asignada.
- Esta historia es esencial para la validación de la asistencia porque la HU-ATT-001 depende de saber si los tiempos de entrada y salida son apropiados para el bloque asignada.
- La relación horario/jornada será utilizada por el registro de asistencia, detección de retardos, detección de inasistencias, historial, reportes y dashboards.
- El sistema debe evitar horarios duplicados o conflictivos para la misma cohorte, usuario responsable, ambiente o período de tiempo.
- El equipo debe definir si la validación de solapamiento aplica solo a la misma cohorte, al mismo instructor responsable, al mismo ambiente, o a una combinación de estos.
- El acceso basado en roles debe aplicarse:
  - Administradores y supervisores pueden asignar y consultar horarios.
  - Instructores pueden consultar horarios asignados a ellos.
  - Estudiantes pueden consultar horarios donde están registrados, si aplica.

**Servicio(s) responsable(s):** Servicio de Estructura Académica / Servicio de Programación, propuesto.  
**Endpoint(s) implementados:**  
- `POST /api/v1/schedules` — propuesto.  
- `GET /api/v1/schedules` — propuesto.  
- `GET /api/v1/cohorts/{cohortId}/schedule` — propuesto.  
- `GET /api/v1/users/me/schedule` — propuesto.  

**Eventos generados:**  
- `ScheduleAssigned`  
- `ScheduleAssignmentFailed`  
- `ScheduleConflictDetected`  

**Permisos requeridos:**  
- Administrador o Supervisor para asignar bloques de tiempo/jornadas.
- Profesor/Instructor puede consultar horarios asignados a ellos.
- Estudiante puede consultar horarios permitidos si aplica.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Asignación exitosa de jornada/bloque de tiempo probada.
- [ ] Escenario de usuario responsable inválido probado.
- [ ] Escenario de solapamiento de bloque de tiempo probado.
- [ ] Escenario de cohorte no disponible probado.
- [ ] Bloque de tiempo asignado asociado correctamente con cohorte y usuario responsable.
- [ ] Consulta de horario probada para administrador, instructor y estudiante donde aplique.
- [ ] Módulo de asistencia puede utilizar el bloque de tiempo asignado para validación de entrada/salida.
- [ ] Evento de auditoría generado.
- [ ] Mensajes de confirmación y error son claros.
- [ ] Tiempo de respuesta validado bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Trazabilidad al RF 2.3.2 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 8 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 1 / MVP |
| Dependencias | HU-IAM-001: Inicio de sesión; HU-ACAD-003: Asignar instructores responsables; HU-ACAD-004: Asignar cohortes a usuarios responsables |
