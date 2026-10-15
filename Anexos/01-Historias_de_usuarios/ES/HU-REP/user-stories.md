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
- Esta historia no cubre los filtros por rango de fechas. Eso se maneja en la HU-REP-003.
- Esta historia no cubre el tablero analítico. Eso se maneja en la HU-REP-002.
- Esta historia no cubre las consultas específicas de retardos/ausencias por parámetros. Eso se maneja en la HU-REP-004.
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
| Dependencias | HU-IAM-001: Inicio de sesión de usuario; HU-IAM-003: Asignación de permisos basada en roles; HU-HIST-001: Consulta de asistencias y ausencias por parámetros |

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
- Esta historia no cubre la exportación de reportes. Eso se maneja en la HU-REP-001.
- Esta historia no cubre los reportes por rango de fechas. Eso se maneja en la HU-REP-003.
- Esta historia no cubre las consultas específicas de retardos/ausencias. Eso se maneja en la HU-REP-004.
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
| Dependencias | HU-IAM-001: Inicio de sesión de usuario; HU-IAM-003: Asignación de permisos basada en roles; HU-HIST-001: Consulta de asistencias y ausencias por parámetros; HU-ATT-001: Registro de asistencia facial |
