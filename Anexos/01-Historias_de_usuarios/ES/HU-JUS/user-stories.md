# HU-JUS-001: Carga del soporte de justificación

## Historia

**Como** estudiante/aprendiz  
**Quiero** cargar una justificación con su correspondiente documento de soporte para una ausencia o un retardo  
**Para que** mi caso pueda ser revisado y evaluado formalmente por un superior, en lugar de permanecer como una ausencia o un retardo sin justificar.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que el estudiante está autenticado y tiene al menos una ausencia o un retardo registrado pendiente de justificación, cuando el estudiante accede al módulo de justificaciones, entonces el sistema muestra la lista de ausencias o retardos elegibles para justificación.

- [ ] **AC2:** Dado que el estudiante selecciona una ausencia o un retardo elegible, cuando el estudiante completa el motivo de la justificación y carga un documento de soporte válido, entonces el sistema valida los datos, almacena la justificación con estado "Pendiente", la asocia con el registro de asistencia correspondiente y muestra un mensaje de confirmación.

- [ ] **AC3:** Dado que el estudiante intenta enviar una justificación sin cargar un documento de soporte, cuando se procesa el envío, entonces el sistema rechaza la operación y muestra un mensaje de validación claro indicando que se requiere un documento de soporte.

- [ ] **AC4:** Dado que el documento de soporte cargado excede el tamaño máximo de archivo permitido o tiene un formato no soportado, cuando se intenta la carga, entonces el sistema rechaza el archivo y muestra un mensaje de error claro con los formatos permitidos y el tamaño máximo.

- [ ] **AC5:** Dado que ya existe una justificación para la ausencia o el retardo seleccionado, cuando el estudiante intenta enviar una nueva justificación para el mismo registro, entonces el sistema evita la justificación duplicada y muestra un mensaje claro indicando que el registro ya tiene una justificación en curso o resuelta.

- [ ] **AC6:** Dado que el estudiante no tiene ausencias ni retardos registrados elegibles para justificación, cuando el estudiante accede al módulo de justificaciones, entonces el sistema muestra un mensaje claro indicando que no hay registros pendientes de justificación.

- [ ] **AC7:** Dado que la justificación se envía correctamente, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el identificador del estudiante, el registro de asistencia asociado, el identificador de la justificación, la referencia del documento de soporte, la fecha y hora de envío y el estado inicial como "Pendiente".

- [ ] **AC8:** Dadas condiciones normales de red, cuando el estudiante envía una justificación con su documento de soporte, entonces el sistema responde en menos de 10 segundos, incluido el tiempo de carga del archivo.

- [ ] **AC9:** Dado que la justificación se envía correctamente, cuando el sistema procesa el envío, entonces el documento de soporte se almacena de forma segura y su contenido no se expone en logs ni en mensajes de la interfaz de usuario más allá del nombre o la referencia del archivo.

---

## Notas Técnicas

- El Caso de Uso 3 del SRS describe el flujo de justificación de la siguiente manera:
  1. El usuario registra su asistencia.
  2. El sistema informa al usuario las ausencias registradas durante el período establecido.
  3. El sistema solicita al usuario cargar una justificación válida para la ausencia.
  4. Si el usuario no carga una justificación, la ausencia permanece marcada como no justificada.
- El RF 6.1 indica que el documento de soporte debe cargarse correctamente antes de continuar con el proceso de evaluación.
- El sistema debe validar:
  - Que el estudiante esté autenticado y activo.
  - Que el registro de asistencia exista y corresponda a una ausencia o un retardo.
  - Que el registro de asistencia no tenga ya una justificación activa o resuelta.
  - Que el documento de soporte esté presente, dentro del tamaño de archivo permitido y en un formato soportado.
  - Que el motivo de la justificación no esté vacío.
- Los formatos de archivo soportados y el tamaño máximo deben ser definidos por el equipo. Valores iniciales propuestos:
  - Formatos: PDF, JPG, JPEG, PNG.
  - Tamaño máximo: 5 MB.
- La justificación debe estar vinculada a un único registro de asistencia, según la regla de dominio RN-32 definida en `entities-and-rules.md`.
- Una justificación no puede aprobarse sin al menos un documento de soporte válido, según la regla de dominio RN-33.
- El documento de soporte debe almacenarse de forma segura y no debe exponer su contenido en logs ni en la interfaz.
- La parametrización de justificaciones, como los tipos de justificación válidos, se gestiona en la [HU-CONF-008](../HU-CONF/user-stories.md#hu-conf-008-parametrización-de-retardos-y-justificaciones) y el RF 4.7.
- Esta historia no cubre el flujo de aprobación/rechazo. Eso se maneja en la [HU-JUS-002](#hu-jus-002-aprobación-y-rechazo-de-justificaciones).
- Esta historia no cubre la notificación automática del resultado. Eso se maneja en la [HU-JUS-003](#hu-jus-003-notificación-automática-del-resultado-de-la-justificación).

**Servicio(s) responsable(s):** Servicio de Justificaciones / Servicio de Almacenamiento de Archivos, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/attendance/unjustified` — propuesto, devuelve las ausencias y retardos elegibles para justificación.  
- `POST /api/v1/justifications` — propuesto, crea la justificación con el documento de soporte.  
- `GET /api/v1/justifications/me` — propuesto, devuelve las justificaciones propias del estudiante.  

**Eventos generados:**  
- `JustificationSubmitted`  
- `JustificationSubmissionFailed`  
- `SupportDocumentUploaded`  

**Permisos requeridos:**  
- Estudiante/aprendiz autenticado con al menos una ausencia o un retardo sin justificar.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] La lista de ausencias y retardos elegibles se muestra correctamente.
- [ ] Envío exitoso de justificación con un documento de soporte válido probado.
- [ ] Escenario de documento de soporte faltante probado.
- [ ] Escenario de formato de archivo inválido probado.
- [ ] Escenario de archivo demasiado grande probado.
- [ ] Prevención de justificación duplicada probada.
- [ ] Escenario sin registros elegibles probado.
- [ ] La justificación se vincula correctamente con el registro de asistencia.
- [ ] La justificación se almacena con el estado inicial "Pendiente".
- [ ] El documento de soporte se almacena de forma segura.
- [ ] El contenido sensible del documento no se expone en logs ni en la UI.
- [ ] Se genera un evento de auditoría.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se valida bajo condiciones normales.
- [ ] Trazabilidad al RF 6.1 y al Caso de Uso 3 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 8 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 2 |
| Dependencias | [HU-IAM-001](../HU-IAM/user-stories.md#hu-iam-001-inicio-de-sesión-de-usuario): Inicio de sesión de usuario; [HU-ATT-001](../HU-ATT/user-stories.md#hu-att-001-registro-de-asistencia-facial): Registro de asistencia facial; [HU-HIST-001](../HU-HIST/user-stories.md#hu-hist-001-consulta-de-asistencias-y-ausencias-por-parámetros): Consulta de asistencias y ausencias por parámetros; [HU-CONF-008](../HU-CONF/user-stories.md#hu-conf-008-parametrización-de-retardos-y-justificaciones): Parametrización de retardos y justificaciones |

---

# HU-JUS-002: Aprobación y rechazo de justificaciones

## Historia

**Como** instructor, administrador o supervisor  
**Quiero** revisar la justificación de un estudiante, evaluar el documento de soporte cargado y aprobar o rechazar la justificación  
**Para que** el registro de asistencia del estudiante se actualice de manera justa y transparente según las reglas institucionales.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que un usuario autorizado con un rol superior está autenticado, cuando el usuario accede al módulo de revisión de justificaciones, entonces el sistema muestra la lista de justificaciones con estado "Pendiente" que corresponden a sus cohortes, ambientes o alcance institucional asignados según su rol.

- [ ] **AC2:** Dado que el revisor selecciona una justificación pendiente, cuando el sistema muestra el detalle de la justificación, entonces el revisor puede ver el nombre del estudiante, la fecha del registro de asistencia, el tipo de ausencia o retardo, el motivo de la justificación y el documento de soporte cargado.

- [ ] **AC3:** Dado que el revisor revisa el documento de soporte y determina que es válido según los parámetros de justificación configurados, cuando el revisor aprueba la justificación, entonces el sistema actualiza el estado de la justificación a "Aprobada", actualiza el estado del registro de asistencia correspondiente a "Justificado", almacena el identificador del revisor y la fecha de evaluación, y muestra un mensaje de confirmación.

- [ ] **AC4:** Dado que el revisor revisa el documento de soporte y determina que no es válido, cuando el revisor rechaza la justificación, entonces el sistema actualiza el estado de la justificación a "Rechazada", mantiene el registro de asistencia en su estado original, almacena el identificador del revisor y la fecha de evaluación, y muestra un mensaje de confirmación.

- [ ] **AC5:** Dado que el documento de soporte no existe o no puede recuperarse al momento de la revisión, cuando el revisor intenta evaluar la justificación, entonces el sistema impide la evaluación y muestra un mensaje de error claro indicando que el documento de soporte no está disponible.

- [ ] **AC6:** Dado que una justificación ya ha sido aprobada o rechazada, cuando otro revisor o el mismo revisor intenta evaluarla nuevamente, entonces el sistema impide la reevaluación y muestra un mensaje claro indicando que la justificación ya fue resuelta.

- [ ] **AC7:** Dado que el usuario que intenta evaluar la justificación no tiene un rol superior o no tiene permiso para revisar justificaciones de esa cohorte o estudiante, cuando el usuario intenta la evaluación, entonces el sistema deniega el acceso y registra un intento de acceso no autorizado.

- [ ] **AC8:** Dado que la justificación es aprobada o rechazada, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el identificador de la justificación, el registro de asistencia, el identificador del revisor, la decisión (aprobada o rechazada) y la fecha y hora de evaluación.

- [ ] **AC9:** Dadas condiciones normales de red, cuando el revisor envía la aprobación o el rechazo, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

---

## Notas Técnicas

- El RF 6.2 indica que el soporte debe validarse según lo definido por el superior.
- El RF 4.7 define la parametrización de los tipos de justificación válidos. La evaluación debe considerar si la justificación enviada coincide con los tipos válidos configurados.
- La regla de dominio RN-35 establece que una justificación solo puede ser evaluada por un usuario con un rol superior al rol del estudiante.
- La regla de dominio RN-34 establece que una justificación solo puede aprobarse si cumple los criterios parametrizados por el administrador.
- La regla de dominio RN-33 establece que una justificación no puede aprobarse si no tiene al menos un documento de soporte válido.
- El sistema debe validar:
  - Que el revisor esté autenticado y tenga un rol superior.
  - Que el revisor tenga visibilidad de la justificación según sus cohortes o alcance asignados.
  - Que la justificación exista y esté en estado "Pendiente".
  - Que el documento de soporte exista y sea accesible.
  - Que la justificación no haya sido resuelta previamente.
- Cuando una justificación se aprueba, el registro de asistencia correspondiente debe actualizarse a "Justificado".
- Cuando una justificación se rechaza, el registro de asistencia debe permanecer en su estado original ("Ausente" o "Retardo").
- La identidad del revisor y su decisión deben almacenarse para fines de trazabilidad.
- El proceso de revisión no debe permitir que la misma justificación sea evaluada más de una vez.
- Esta historia no cubre la notificación automática del resultado. Eso se maneja en la [HU-JUS-003](#hu-jus-003-notificación-automática-del-resultado-de-la-justificación).

**Servicio(s) responsable(s):** Servicio de Justificaciones / Servicio de Asistencia, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/justifications?status=pending` — propuesto, devuelve las justificaciones pendientes filtradas por el alcance del revisor.  
- `GET /api/v1/justifications/{justificationId}` — propuesto, devuelve el detalle de la justificación con el documento de soporte.  
- `POST /api/v1/justifications/{justificationId}/approve` — propuesto.  
- `POST /api/v1/justifications/{justificationId}/reject` — propuesto.  

**Eventos generados:**  
- `JustificationApproved`  
- `JustificationRejected`  
- `AttendanceRecordUpdatedToJustified`  
- `JustificationEvaluationFailed`  
- `UnauthorizedJustificationAccessAttempt`  

**Permisos requeridos:**  
- Instructor, Administrador o Supervisor, según la política institucional.  
- El revisor debe tener visibilidad de la cohorte o el alcance del estudiante.  
- Los estudiantes no deben poder evaluar sus propias justificaciones.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] La lista de justificaciones pendientes se muestra correctamente para el alcance del revisor.
- [ ] El detalle de la justificación con el documento de soporte se muestra correctamente.
- [ ] Aprobación exitosa probada.
- [ ] Rechazo exitoso probado.
- [ ] El registro de asistencia se actualiza a "Justificado" tras la aprobación.
- [ ] El registro de asistencia permanece sin cambios tras el rechazo.
- [ ] Escenario de documento de soporte faltante o inaccesible probado.
- [ ] Escenario de justificación ya resuelta probado.
- [ ] Escenario de revisor no autorizado probado.
- [ ] Prevención de autoevaluación del estudiante probada.
- [ ] La identidad del revisor y la fecha de decisión se almacenan.
- [ ] Se genera un evento de auditoría para la aprobación y el rechazo.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se valida bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Trazabilidad al RF 6.2, RF 4.7 y Caso de Uso 3 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 8 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 2 |
| Dependencias | [HU-JUS-001](#hu-jus-001-carga-del-soporte-de-justificación): Carga del soporte de justificación; [HU-CONF-008](../HU-CONF/user-stories.md#hu-conf-008-parametrización-de-retardos-y-justificaciones): Parametrización de retardos y justificaciones; [HU-IAM-003](../HU-IAM/user-stories.md#hu-iam-003-asignación-de-permisos-basada-en-roles): Asignación de permisos basada en roles |

---

# HU-JUS-003: Notificación automática del resultado de la justificación

## Historia

**Como** estudiante/aprendiz que envió una justificación  
**Quiero** recibir una notificación automática con el resultado de mi justificación (aprobada o rechazada)  
**Para que** conozca el resultado sin tener que consultar el sistema manualmente y pueda tomar acciones adicionales si es necesario.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que un revisor ha aprobado una justificación, cuando se procesa la aprobación, entonces el sistema envía automáticamente una notificación al estudiante que envió la justificación, incluyendo el identificador de la justificación, la fecha del registro de asistencia y el resultado como "Aprobada".

- [ ] **AC2:** Dado que un revisor ha rechazado una justificación, cuando se procesa el rechazo, entonces el sistema envía automáticamente una notificación al estudiante que envió la justificación, incluyendo el identificador de la justificación, la fecha del registro de asistencia y el resultado como "Rechazada".

- [ ] **AC3:** Dado que la notificación es enviada, cuando el estudiante accede al módulo de notificaciones o a su bandeja de notificaciones, entonces el estudiante puede ver la notificación con el resultado de la justificación, la fecha de la evaluación y el registro de asistencia asociado.

- [ ] **AC4:** Dado que la notificación no puede entregarse debido a un error del sistema, cuando falla la entrega, entonces el sistema registra la falla en el log de la plataforma y reintenta la entrega según la política de reintentos configurada, o marca la notificación como fallida para revisión administrativa.

- [ ] **AC5:** Dado que se evalúa una justificación, cuando se genera la notificación del resultado, entonces la notificación no incluye detalles sensibles como el contenido del documento de soporte, los datos personales del revisor o cualquier información biométrica. Solo se incluyen el resultado, el identificador de la justificación y la fecha de asistencia.

- [ ] **AC6:** Dado que el estudiante tiene las notificaciones habilitadas, cuando se evalúan múltiples justificaciones, entonces el sistema envía notificaciones individuales por cada resultado de justificación.

- [ ] **AC7:** Dado que la notificación es generada, cuando se completa la operación, entonces el sistema emite un evento de auditoría de la plataforma que contiene el tipo de notificación, el identificador del estudiante, el identificador de la justificación, el resultado y el estado de entrega de la notificación.

- [ ] **AC8:** Dadas condiciones normales del sistema, cuando se procesa el resultado de la justificación, entonces la notificación se genera y se encola para su entrega dentro de los 60 segundos posteriores a la evaluación.

---

## Notas Técnicas

- El RF 6.3 indica que el superior proporciona la respuesta de la justificación de forma automática y que el soporte debe existir al momento de la respuesta.
- El mecanismo de notificación debe ser definido por el equipo. Los posibles canales incluyen:
  - Notificación dentro de la aplicación (in-app), como canal principal.
  - Notificación por correo electrónico, si está configurada y aprobada.
  - Notificación push, si la aplicación móvil la soporta.
  - Los canales SMS o de mensajería externa están fuera del alcance, a menos que se aprueben explícitamente.
- La notificación debe generarse como reacción a los eventos `JustificationApproved` o `JustificationRejected` de la [HU-JUS-002](#hu-jus-002-aprobación-y-rechazo-de-justificaciones).
- La notificación no debe incluir:
  - El contenido del documento de soporte.
  - Los datos personales del revisor más allá de lo que esté permitido institucionalmente.
  - Información biométrica o sensible.
- Si la entrega de la notificación falla, el sistema debe:
  - Registrar la falla en el log.
  - Reintentar según una política configurada, o marcarla para revisión administrativa.
  - No bloquear el proceso de evaluación de la justificación.
- El estudiante debe poder consultar sus notificaciones aun si perdió la entrega inicial.
- Las preferencias de notificación pueden ser gestionadas por el estudiante mediante la [HU-CONF-001](../HU-CONF/user-stories.md#hu-conf-001-configurar-alertas-del-sistema) y la [HU-CONF-002](../HU-CONF/user-stories.md#hu-conf-002-habilitar-o-deshabilitar-alertas), si esas historias se implementan.
- La regla de dominio RN-36 establece que el resultado de una justificación debe notificarse automáticamente al usuario solicitante.
- El equipo debe definir el retraso máximo aceptable entre la evaluación y la entrega de la notificación. Valor inicial propuesto: 60 segundos.

**Servicio(s) responsable(s):** Servicio de Justificaciones / Servicio de Notificaciones, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/notifications/me` — propuesto, devuelve las notificaciones del usuario autenticado.  
- `GET /api/v1/notifications/{notificationId}` — propuesto, devuelve el detalle de la notificación.  
- `PATCH /api/v1/notifications/{notificationId}/read` — propuesto, marca una notificación como leída.  

**Eventos generados:**  
- `JustificationResultNotificationSent`  
- `JustificationResultNotificationFailed`  
- `NotificationRead`  

**Permisos requeridos:**  
- Notificación generada por el sistema, no activada por el usuario.  
- El estudiante/aprendiz autenticado puede consultar y leer sus propias notificaciones.  
- Los Administradores o Supervisores pueden revisar las fallas de entrega de notificaciones.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Se genera la notificación automática tras la aprobación de la justificación.
- [ ] Se genera la notificación automática tras el rechazo de la justificación.
- [ ] El contenido de la notificación incluye el identificador de la justificación, la fecha de asistencia y el resultado.
- [ ] La notificación no incluye datos sensibles como el contenido del soporte o los datos personales del revisor.
- [ ] El estudiante puede consultar las notificaciones en la bandeja de notificaciones.
- [ ] Se registra la falla de entrega de la notificación.
- [ ] Mecanismo de reintento o revisión administrativa probado.
- [ ] Múltiples evaluaciones de justificaciones generan notificaciones individuales.
- [ ] La notificación se genera dentro del retraso aceptable tras la evaluación.
- [ ] Se genera un evento de auditoría para la entrega de la notificación.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] Trazabilidad al RF 6.3 y al Caso de Uso 3 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 2 |
| Dependencias | [HU-JUS-002](#hu-jus-002-aprobación-y-rechazo-de-justificaciones): Aprobación y rechazo de justificaciones; [HU-CONF-001](../HU-CONF/user-stories.md#hu-conf-001-configurar-alertas-del-sistema): Configurar alertas del sistema, si aplica; [HU-CONF-002](../HU-CONF/user-stories.md#hu-conf-002-habilitar-o-deshabilitar-alertas): Habilitar o deshabilitar alertas, si aplica |
