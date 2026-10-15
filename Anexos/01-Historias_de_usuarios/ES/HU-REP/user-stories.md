# HU-REP-001: Exportación de reportes

## Historia

**Como** administrador, supervisor, instructor o usuario autorizado  
**Quiero** exportar reportes de asistencia que incluyan asistencias, ausencias, retardos, cohortes/grupos, instructores y ambientes  
**Para que** pueda obtener archivos estructurados para el análisis institucional, el seguimiento académico y el registro administrativo según mi rol.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que un usuario autenticado accede al módulo de exportación de reportes, cuando el sistema muestra los tipos de reporte disponibles, entonces el usuario puede ver las siguientes opciones: asistencias, ausencias, retardos, cohortes/grupos, instructores/profesores y ambientes/salones.

- [ ] **AC2:** Dado que el usuario selecciona un tipo de reporte, cuando se envía la solicitud de exportación, entonces el sistema valida que el rol del usuario esté autorizado para exportar ese tipo de reporte específico antes de procesar la solicitud.

- [ ] **AC3:** Dado que el rol del usuario está autorizado para el tipo de reporte seleccionado, cuando se procesa la exportación, entonces el sistema genera el archivo del reporte y lo pone a disposición del usuario para su descarga.

- [ ] **AC4:** Dado que el rol del usuario no está autorizado para el tipo de reporte seleccionado, cuando se solicita la exportación, entonces el sistema deniega la solicitud, muestra un mensaje de error de permisos claro y registra un intento de acceso no autorizado.

- [ ] **AC5:** Dado que la generación del reporte se completa correctamente, cuando se descarga el archivo, entonces los datos exportados coinciden con el alcance autorizado actual del usuario e incluyen el tipo de reporte, la fecha de generación y el usuario que solicitó la exportación.

- [ ] **AC6:** Dado que el reporte no contiene datos que coincidan con el alcance del usuario, cuando se solicita la exportación, entonces el sistema muestra un mensaje claro indicando que no hay datos disponibles para exportar bajo los criterios actuales.

- [ ] **AC7:** Dado que un estudiante intenta exportar reportes fuera de su alcance personal, cuando se procesa la solicitud, entonces el sistema deniega la solicitud y aplica las restricciones basadas en roles.

- [ ] **AC8:** Dadas condiciones normales de red, cuando el usuario envía una solicitud de exportación estándar, entonces el sistema responde en menos de 10 segundos para volúmenes de datos típicos.

- [ ] **AC9:** Dado que la solicitud de exportación es procesada, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el usuario, el tipo de reporte, la fecha, la hora y el resultado.

---

## Notas Técnicas

- El RF 7.1 indica que el sistema debe validar el rol del usuario y asociarlo con los tipos de exportación que está autorizado a solicitar.
- Los tipos de reporte definidos por el SRS son:
  - Asistencias.
  - Faltas.
  - Retardos.
  - Fichas/Grupos.
  - Instructores/Profesores.
  - Ambientes/Salón.
- La matriz de acceso por rol para los reportes debe ser definida por el equipo. Matriz inicial propuesta:
  - Estudiante: solo su historial personal de asistencia.
  - Instructor: asistencias, retardos y ausencias de las cohortes asignadas.
  - Administrador/Supervisor: todos los tipos de reporte.
- Los formatos de exportación deben ser definidos por el equipo. Formatos iniciales propuestos:
  - CSV para análisis de datos.
  - PDF para documentos institucionales.
- Para conjuntos de datos grandes, el sistema debe considerar:
  - Paginación o generación por streaming.
  - Exportación asíncrona con notificación cuando esté lista.
  - Un límite máximo de tamaño de exportación.
- El archivo exportado debe incluir:
  - Tipo de reporte.
  - Fecha y hora de generación.
  - Usuario que solicitó la exportación.
  - Filtros aplicados, si existen.
- Los datos exportados deben respetar el alcance del rol del usuario y no deben incluir registros no autorizados.
- Esta historia no cubre los filtros por rango de fechas. Eso se maneja en la [HU-REP-003](#hu-rep-003-reportes-por-rango-de-fechas).
- Esta historia no cubre el tablero analítico. Eso se maneja en la [HU-REP-002](#hu-rep-002-tablero-analítico-basado-en-roles).
- Esta historia no cubre las consultas específicas de retardos/ausencias por parámetros. Eso se maneja en la [HU-REP-004](#hu-rep-004-consulta-de-retardos-y-ausencias-por-parámetros).
- El rendimiento de exportaciones grandes puede superar el tiempo de respuesta estándar de 5 segundos. El equipo debe definir si se requiere exportación asíncrona.
- Los datos sensibles incluidos en las exportaciones deben protegerse según el RNF 4.

**Servicio(s) responsable(s):** Servicio de Reportes / Servicio de Exportación, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/reports/types` — propuesto, devuelve los tipos de reporte disponibles para el rol del usuario.  
- `POST /api/v1/reports/export` — propuesto, genera y devuelve el archivo del reporte.  
- `GET /api/v1/reports/export/{exportId}` — propuesto, recupera el resultado de una exportación asíncrona.  

**Eventos generados:**  
- `ReportExportRequested`  
- `ReportExportCompleted`  
- `ReportExportFailed`  
- `UnauthorizedReportAccessAttempt`  

**Permisos requeridos:**  
- Usuario autenticado.  
- Aplicación del alcance según el rol:
  - Estudiante: solo registros personales.
  - Instructor: cohortes/ambientes asignados.
  - Administrador/Supervisor: alcance institucional completo.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Los seis tipos de reporte están disponibles según los permisos del rol.
- [ ] Validación basada en roles probada para cada tipo de reporte.
- [ ] Exportación exitosa probada para los roles autorizados.
- [ ] Intento de exportación no autorizado probado y denegado.
- [ ] Escenario de resultado vacío probado.
- [ ] Restricción de alcance del estudiante probada.
- [ ] El archivo exportado incluye el tipo de reporte, la fecha de generación y el usuario solicitante.
- [ ] Se genera un evento de auditoría para las solicitudes de exportación.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se valida para volúmenes de datos típicos.
- [ ] El comportamiento con conjuntos de datos grandes está definido y probado.
- [ ] Trazabilidad al RF 7.1 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 8 |
| Prioridad | Media |
| Sprint objetivo | Sprint 3 |
| Dependencias | [HU-IAM-001](../HU-IAM/user-stories.md#hu-iam-001-inicio-de-sesión-de-usuario): Inicio de sesión de usuario; [HU-IAM-003](../HU-IAM/user-stories.md#hu-iam-003-asignación-de-permisos-basada-en-roles): Asignación de permisos basada en roles; [HU-HIST-001](../HU-HIST/user-stories.md#hu-hist-001-consulta-de-asistencias-y-ausencias-por-parámetros): Consulta de asistencias y ausencias por parámetros |

---

# HU-REP-002: Tablero analítico basado en roles

## Historia

**Como** administrador, supervisor, instructor o estudiante  
**Quiero** acceder a un tablero analítico correspondiente a mi rol  
**Para que** pueda visualizar los indicadores de asistencia relevantes y tomar decisiones oportunas según mis responsabilidades.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que un usuario autenticado accede al módulo del tablero, cuando el sistema procesa la solicitud, entonces el sistema valida el rol del usuario y muestra el tablero correspondiente a ese rol.

- [ ] **AC2:** Dado que el usuario es un estudiante, cuando se muestra el tablero, entonces el sistema muestra información personal de asistencia como el porcentaje de asistencia, las ausencias, los retardos y el estado de las justificaciones.

- [ ] **AC3:** Dado que el usuario es un instructor, cuando se muestra el tablero, entonces el sistema muestra información de asistencia de las cohortes, ambientes o bloques de formación asignados a ese instructor.

- [ ] **AC4:** Dado que el usuario es un administrador o supervisor, cuando se muestra el tablero, entonces el sistema muestra indicadores de asistencia de alcance institucional o administrativo.

- [ ] **AC5:** Dado que el rol del usuario no tiene un tablero asociado, cuando el usuario accede al módulo del tablero, entonces el sistema muestra un mensaje claro indicando que no hay un tablero disponible para su rol.

- [ ] **AC6:** Dado que se muestra el tablero, cuando el usuario interactúa con los componentes visuales, entonces la información se actualiza según el alcance del usuario sin exponer datos no autorizados.

- [ ] **AC7:** Dado que un usuario no autorizado intenta acceder a un tablero fuera de su rol, cuando se procesa la solicitud, entonces el sistema deniega el acceso y registra un intento de acceso no autorizado.

- [ ] **AC8:** Dadas condiciones normales de red, cuando el usuario accede al tablero, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

- [ ] **AC9:** Dado que se accede al tablero, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el usuario, el rol, la fecha y la hora.

---

## Notas Técnicas

- El RF 7.2 indica que el sistema debe validar el rol del usuario y asociarlo con el tablero correspondiente.
- El contenido del tablero debe definirse por rol. Contenido inicial propuesto:
  - **Tablero del estudiante:**
    - Porcentaje de asistencia.
    - Total de ausencias.
    - Total de retardos.
    - Estado de las justificaciones.
    - Registros de asistencia recientes.
  - **Tablero del instructor:**
    - Resumen de asistencia de las cohortes asignadas.
    - Estudiantes con más ausencias o retardos.
    - Registros de asistencia recientes.
    - Justificaciones pendientes, si aplica.
  - **Tablero del administrador/supervisor:**
    - Resumen institucional de asistencia.
    - Asistencia por cohorte.
    - Asistencia por ambiente.
    - Usuarios con más ausencias o retardos.
    - Alertas del sistema, si aplica.
- El tablero debe ser responsivo y compatible con las vistas web y móvil, según el RNF 3.
- El tablero debe considerar:
  - Actualizaciones en tiempo real o casi real, si es factible.
  - Indicadores visuales claros (gráficos, contadores, insignias de estado).
  - Buenas prácticas de accesibilidad, según el RNF 2.
- El filtrado basado en roles debe aplicarse en el lado del servidor. El filtrado únicamente en el cliente no es suficiente.
- El tablero debe usar datos en caché o agregados cuando sea posible para mejorar el rendimiento.
- Esta historia no cubre la exportación de reportes. Eso se maneja en la [HU-REP-001](#hu-rep-001-exportación-de-reportes).
- Esta historia no cubre los reportes por rango de fechas. Eso se maneja en la [HU-REP-003](#hu-rep-003-reportes-por-rango-de-fechas).
- Esta historia no cubre las consultas específicas de retardos/ausencias. Eso se maneja en la [HU-REP-004](#hu-rep-004-consulta-de-retardos-y-ausencias-por-parámetros).
- El equipo debe definir los KPIs específicos y los componentes visuales para cada rol.

**Servicio(s) responsable(s):** Servicio de Reportes / Servicio de Tableros, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/dashboard` — propuesto, devuelve los datos del tablero según el rol del usuario.  
- `GET /api/v1/dashboard/student` — propuesto.  
- `GET /api/v1/dashboard/instructor` — propuesto.  
- `GET /api/v1/dashboard/admin` — propuesto.  

**Eventos generados:**  
- `DashboardAccessed`  
- `UnauthorizedDashboardAccessAttempt`  

**Permisos requeridos:**  
- Usuario autenticado.  
- Aplicación del alcance según el rol:
  - Estudiante: tablero personal.
  - Instructor: tablero de cohortes/ambientes asignados.
  - Administrador/Supervisor: tablero institucional.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] El tablero se muestra correctamente para cada rol.
- [ ] El tablero del estudiante muestra la información personal de asistencia.
- [ ] El tablero del instructor muestra la información de las cohortes asignadas.
- [ ] El tablero del administrador/supervisor muestra los indicadores institucionales.
- [ ] Escenario de rol sin tablero probado.
- [ ] Intento de acceso no autorizado al tablero probado.
- [ ] Los datos del tablero respetan el alcance del usuario.
- [ ] El tablero es responsivo en las vistas web y móvil.
- [ ] Se genera un evento de auditoría para el acceso al tablero.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se valida bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Trazabilidad al RF 7.2 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 8 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 3 |
| Dependencias | [HU-IAM-001](../HU-IAM/user-stories.md#hu-iam-001-inicio-de-sesión-de-usuario): Inicio de sesión de usuario; [HU-IAM-003](../HU-IAM/user-stories.md#hu-iam-003-asignación-de-permisos-basada-en-roles): Asignación de permisos basada en roles; [HU-HIST-001](../HU-HIST/user-stories.md#hu-hist-001-consulta-de-asistencias-y-ausencias-por-parámetros): Consulta de asistencias y ausencias por parámetros; [HU-ATT-001](../HU-ATT/user-stories.md#hu-att-001-registro-de-asistencia-facial): Registro de asistencia facial |

---

# HU-REP-003: Reportes por rango de fechas

## Historia

**Como** administrador, supervisor, instructor o usuario autorizado  
**Quiero** exportar reportes filtrados por un rango de fechas  
**Para que** pueda analizar la información de asistencia de períodos específicos, como semanas, meses o períodos académicos, según mi rol.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que un usuario autenticado accede al módulo de reportes por rango de fechas, cuando el usuario selecciona la fecha de inicio, la fecha de fin y el tipo de reporte, entonces el sistema valida los parámetros de entrada antes de procesar la solicitud.

- [ ] **AC2:** Dado que el rango de fechas seleccionado es válido y el rol del usuario está autorizado, cuando se envía la solicitud de exportación, entonces el sistema genera el reporte filtrado por el rango de fechas especificado y lo pone a disposición para su descarga.

- [ ] **AC3:** Dado que la fecha de inicio es posterior a la fecha de fin, cuando se envía la solicitud, entonces el sistema rechaza la operación y muestra un mensaje de validación claro.

- [ ] **AC4:** Dado que el rango de fechas seleccionado excede el período máximo permitido, cuando se envía la solicitud, entonces el sistema rechaza la operación y muestra un mensaje claro indicando el período máximo permitido.

- [ ] **AC5:** Dado que el rol del usuario no está autorizado para el tipo de reporte seleccionado, cuando se solicita la exportación, entonces el sistema deniega la solicitud, muestra un error de permisos y registra un intento de acceso no autorizado.

- [ ] **AC6:** Dado que no existen datos dentro del rango de fechas seleccionado, cuando se solicita la exportación, entonces el sistema muestra un mensaje claro indicando que no hay datos disponibles para el período especificado.

- [ ] **AC7:** Dado que el reporte por rango de fechas es generado, cuando se descarga el archivo, entonces los datos exportados respetan el alcance del rol del usuario e incluyen el rango de fechas aplicado, el tipo de reporte, la fecha de generación y el usuario solicitante.

- [ ] **AC8:** Dadas condiciones normales de red, cuando el usuario envía una solicitud estándar de exportación por rango de fechas, entonces el sistema responde en menos de 10 segundos para volúmenes de datos típicos.

- [ ] **AC9:** Dado que la exportación por rango de fechas es procesada, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el usuario, el tipo de reporte, el rango de fechas, la fecha, la hora y el resultado.

---

## Notas Técnicas

- El RF 7.3 indica que el sistema debe validar el rol del usuario y asociarlo con los tipos de exportación que está autorizado a solicitar, de forma similar al RF 7.1 pero con filtrado por rango de fechas.
- Los tipos de reporte disponibles para la exportación por rango de fechas deben alinearse con la [HU-REP-001](#hu-rep-001-exportación-de-reportes):
  - Asistencias.
  - Faltas.
  - Retardos.
  - Fichas/Grupos.
  - Instructores/Profesores.
  - Ambientes/Salón.
- El equipo debe definir:
  - El rango de fechas máximo permitido (por ejemplo, 1 año).
  - Si la exportación por rango de fechas soporta filtros combinados (cohorte, ambiente, instructor).
  - Si se requiere exportación asíncrona para rangos de fechas amplios.
- El sistema debe validar:
  - Que la fecha de inicio no sea posterior a la fecha de fin.
  - Que el rango de fechas no exceda el período máximo permitido.
  - Que las fechas sean válidas y aplicables al contexto de control de asistencia.
- El archivo exportado debe incluir:
  - El rango de fechas aplicado.
  - El tipo de reporte.
  - La fecha y hora de generación.
  - El usuario que solicitó la exportación.
- Esta historia se basa en la [HU-REP-001](#hu-rep-001-exportación-de-reportes). El equipo debe considerar reutilizar la lógica de generación de exportaciones.
- Esta historia no cubre el tablero analítico. Eso se maneja en la [HU-REP-002](#hu-rep-002-tablero-analítico-basado-en-roles).
- Esta historia no cubre las consultas específicas de retardos/ausencias por parámetros. Eso se maneja en la [HU-REP-004](#hu-rep-004-consulta-de-retardos-y-ausencias-por-parámetros).
- El rendimiento para rangos de fechas amplios puede requerir una exportación asíncrona con notificación cuando esté lista.
- Los datos sensibles incluidos en las exportaciones deben protegerse según el RNF 4.

**Servicio(s) responsable(s):** Servicio de Reportes / Servicio de Exportación, propuesto.  
**Endpoint(s) implementados:**  
- `POST /api/v1/reports/export/by-date-range` — propuesto.  
- `GET /api/v1/reports/export/{exportId}` — propuesto, recupera el resultado de una exportación asíncrona.  

**Eventos generados:**  
- `DateRangeReportRequested`  
- `DateRangeReportCompleted`  
- `DateRangeReportFailed`  
- `UnauthorizedDateRangeReportAccessAttempt`  

**Permisos requeridos:**  
- Usuario autenticado.  
- Aplicación del alcance según el rol:
  - Estudiante: solo registros personales.
  - Instructor: cohortes/ambientes asignados.
  - Administrador/Supervisor: alcance institucional completo.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Exportación exitosa por rango de fechas probada.
- [ ] Escenario de fecha de inicio posterior a la fecha de fin probado.
- [ ] Escenario de rango de fechas máximo excedido probado.
- [ ] Escenario de tipo de reporte no autorizado probado.
- [ ] Escenario de resultado vacío en el rango de fechas probado.
- [ ] Aplicación del alcance basado en roles probada.
- [ ] El archivo exportado incluye el rango de fechas aplicado, el tipo de reporte y el usuario solicitante.
- [ ] Se genera un evento de auditoría para las solicitudes de exportación por rango de fechas.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se valida para volúmenes de datos típicos.
- [ ] El comportamiento con rangos de fechas amplios está definido y probado.
- [ ] Trazabilidad al RF 7.3 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Media |
| Sprint objetivo | Sprint 3 |
| Dependencias | [HU-REP-001](#hu-rep-001-exportación-de-reportes): Exportación de reportes; [HU-IAM-003](../HU-IAM/user-stories.md#hu-iam-003-asignación-de-permisos-basada-en-roles): Asignación de permisos basada en roles; [HU-HIST-001](../HU-HIST/user-stories.md#hu-hist-001-consulta-de-asistencias-y-ausencias-por-parámetros): Consulta de asistencias y ausencias por parámetros |

---

# HU-REP-004: Consulta de retardos y ausencias por parámetros

## Historia

**Como** administrador, supervisor, instructor o usuario autorizado  
**Quiero** consultar retardos y ausencias utilizando parámetros como usuario, fecha, cohorte/grupo, ambiente/salón e instructor  
**Para que** pueda identificar problemas de asistencia y tomar acciones correctivas oportunas según mi rol.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que un usuario autenticado accede al módulo de consulta de retardos/ausencias, cuando el usuario selecciona uno o más parámetros (usuario, fecha, cohorte/grupo, ambiente/salón, instructor), entonces el sistema valida los parámetros antes de procesar la consulta.

- [ ] **AC2:** Dado que los parámetros seleccionados son válidos y el rol del usuario está autorizado, cuando se ejecuta la consulta, entonces el sistema devuelve los retardos y ausencias que coinciden con los filtros solicitados.

- [ ] **AC3:** Dado que el rol del usuario no está autorizado para el alcance de consulta solicitado, cuando se ejecuta la consulta, entonces el sistema deniega el acceso, muestra un error de permisos y registra un intento de acceso no autorizado.

- [ ] **AC4:** Dado que los parámetros seleccionados no producen registros coincidentes, cuando se ejecuta la consulta, entonces el sistema muestra un mensaje claro indicando que no se encontraron retardos ni ausencias para los filtros especificados.

- [ ] **AC5:** Dado que un estudiante consulta retardos y ausencias, cuando se procesa la solicitud, entonces el sistema solo devuelve los registros pertenecientes a ese estudiante.

- [ ] **AC6:** Dado que un instructor consulta retardos y ausencias, cuando se procesa la solicitud, entonces el sistema solo devuelve los registros asociados con las cohortes, ambientes o bloques de formación asignados a ese instructor.

- [ ] **AC7:** Dado que la consulta devuelve resultados, cuando el sistema los muestra, entonces cada registro incluye como mínimo la siguiente información: usuario, fecha, tipo (retardo o ausencia), cohorte, ambiente, instructor y estado de la justificación cuando aplique.

- [ ] **AC8:** Dadas condiciones normales de red, cuando el usuario ejecuta una consulta de retardos/ausencias, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

- [ ] **AC9:** Dado que la consulta es ejecutada, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el usuario, los parámetros aplicados, la fecha y la hora.

---

## Notas Técnicas

- El RF 7.4 indica que el sistema debe validar el rol del usuario y asociarlo con las consultas de retardos/ausencias que está autorizado a solicitar.
- Los parámetros de consulta definidos por el SRS son:
  - Referencias de usuario.
  - Fechas.
  - Fichas/grupos.
  - Ambientes/salones.
  - Instructores/profesores.
- La consulta debe soportar filtros combinados para obtener resultados más precisos.
- El filtrado basado en roles debe aplicarse en el lado del servidor:
  - Estudiantes: solo sus propios registros.
  - Instructores: registros de las cohortes/ambientes/bloques de formación asignados.
  - Administradores/Supervisores: alcance administrativo más amplio.
- Los resultados de la consulta deben incluir el estado de la justificación para ayudar a los usuarios a identificar qué retardos/ausencias han sido justificados.
- Se debe considerar la paginación para conjuntos de resultados grandes.
- El equipo debe considerar:
  - Opciones de ordenamiento (por fecha, usuario, tipo).
  - Opción de exportar los resultados de la consulta, si aplica.
  - Integración con la [HU-REP-001](#hu-rep-001-exportación-de-reportes) para exportar los resultados de la consulta.
- Esta historia no cubre el tablero analítico. Eso se maneja en la [HU-REP-002](#hu-rep-002-tablero-analítico-basado-en-roles).
- Esta historia no cubre las consultas generales del historial de asistencias/ausencias. Eso se maneja en la [HU-HIST-001](../HU-HIST/user-stories.md#hu-hist-001-consulta-de-asistencias-y-ausencias-por-parámetros).
- Esta historia se enfoca específicamente en retardos y ausencias, no en todos los registros de asistencia.
- Los datos de asistencia deben protegerse según el RNF 4, y el acceso debe restringirse según el rol del usuario.

**Servicio(s) responsable(s):** Servicio de Reportes / Servicio de Consulta de Retardos y Ausencias, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/reports/delays-absences` — propuesto, con parámetros de consulta para usuario, fecha, cohorte, ambiente e instructor.  
- `GET /api/v1/reports/delays-absences/me` — propuesto, para estudiantes que consultan sus propios registros.  

**Eventos generados:**  
- `DelayAbsenceQueryExecuted` — recomendado para fines de auditoría.  
- `UnauthorizedDelayAbsenceQueryAttempt`  

**Permisos requeridos:**  
- Usuario autenticado.  
- Aplicación del alcance según el rol:
  - Estudiante: solo sus propios registros.
  - Instructor: cohortes, ambientes o bloques de formación asignados.
  - Administrador/Supervisor: alcance administrativo.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Consulta por usuario probada.
- [ ] Consulta por fecha probada.
- [ ] Consulta por cohorte/grupo probada.
- [ ] Consulta por ambiente/salón probada.
- [ ] Consulta por instructor probada.
- [ ] Filtros combinados probados.
- [ ] Escenario de resultado vacío probado.
- [ ] Filtrado basado en roles probado para estudiante, instructor, administrador y supervisor.
- [ ] Intento de acceso no autorizado probado.
- [ ] Los registros devueltos incluyen los campos requeridos: usuario, fecha, tipo, cohorte, ambiente, instructor, estado de la justificación.
- [ ] Paginación probada para conjuntos de resultados grandes.
- [ ] Se genera un evento de auditoría.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se valida bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Trazabilidad al RF 7.4 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 3 |
| Dependencias | [HU-IAM-001](../HU-IAM/user-stories.md#hu-iam-001-inicio-de-sesión-de-usuario): Inicio de sesión de usuario; [HU-IAM-003](../HU-IAM/user-stories.md#hu-iam-003-asignación-de-permisos-basada-en-roles): Asignación de permisos basada en roles; [HU-HIST-001](../HU-HIST/user-stories.md#hu-hist-001-consulta-de-asistencias-y-ausencias-por-parámetros): Consulta de asistencias y ausencias por parámetros; [HU-CONF-008](../HU-CONF/user-stories.md#hu-conf-008-parametrización-de-retardos-y-justificaciones): Parametrización de retardos y justificaciones |
