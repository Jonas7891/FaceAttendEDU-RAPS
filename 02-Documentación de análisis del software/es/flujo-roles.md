---
title: "Flujo de cada rol dentro del aplicativo — FaceAttend EDU"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
date: "13 de octubre de 2026"
abstract: |
  Este documento describe el flujo que tiene cada rol dentro del aplicativo FaceAttend EDU: SUPER_ADMIN, SCHOOL_ADMIN, INSTRUCTOR y STUDENT. Para cada rol presenta el objetivo, las pantallas y funciones a las que accede, la secuencia típica de pasos y los permisos clave, con base en el mapa de navegación validado contra el código frontend, las historias de usuario y los seeds de autorización en FULL/database.
keywords:
  - roles de usuario
  - control de acceso por roles
  - flujos de usuario
  - gestión de asistencia
  - Normas APA 7ma edición
---

# Introducción

## Objetivo del documento

Describir el flujo de extremo a extremo de cada rol dentro del aplicativo, de modo que el análisis establezca sin ambigüedad quién hace qué, en qué orden y con qué permisos.

## Alcance del documento

Este documento cubre los cuatro roles del catálogo (SUPER_ADMIN, SCHOOL_ADMIN, INSTRUCTOR, STUDENT) en los clientes web y móvil. No define la implementación de la matriz de permisos, que pertenece a los artefactos de diseño y código.

## Método y fuentes

Los flujos se reconstruyeron a partir del mapa de navegación validado contra el frontend (`useRolePermissions.js`, pestañas visibles, menús de acciones por rol), las historias de usuario (`04-requirements`), las reglas de dominio (`02-domain`) y los seeds de autorización (`FULL/database/02-ms-authorization-db`). El catálogo seed define 12 permisos en formato `contexto.recurso:acción`; el build móvil unifica `SCHOOL_ADMIN` en `Administrador` y reintroduce `Super Administrador` como rol global.

## Flujo transversal: autenticación

Todos los roles ingresan con un identificador (correo o username) más contraseña (BCrypt, costo 12), reciben un 401 genérico que evita la enumeración de usuarios ante fallos y obtienen una sesión opaca validada tanto en Kong como en cada microservicio. El bloqueo por intentos (`max_login_attempts=5`, `lockout=30min`) está especificado pero pendiente de implementación. Toda tabla aplica borrado lógico y bloqueo optimista.

# Flujos por rol

## 1. SUPER_ADMIN — Administrador global

**Objetivo.** Control total del sistema y bootstrap (usuario por defecto `admin.faceattend`); único rol con alcance multi-sede.

**Pantallas y funciones.** Dashboard web con seis pestañas: dashboard institucional (KPIs, estudiantes en riesgo), Usuarios (CRUD, importación CSV, enrolar rostros a terceros), Cursos, Ambientes (gestión exclusiva), Reportes de alcance total con exportación y notificar-a-todos, y Configuración (seguridad, paletas, parametrización de justificaciones, idioma y tema). Móvil: gestión de usuarios, gestión de ambientes, configuración de sede y reportes.

**Secuencia típica.**

1. Ingresar y llegar al dashboard institucional.
2. Crear la estructura de sede, programa, cohorte, curso y ambiente.
3. Crear personas, credenciales y asignar roles (`POST /persons`, `POST /users`, `POST /users/{id}/roles`).
4. Definir la matriz rol×permiso y verificarla mediante evaluación de permisos.
5. Parametrizar tipos de justificación, tolerancia de tardanzas, alertas y políticas de seguridad.
6. Auditar el historial de cambios de cualquier usuario y exportar cualquier reporte.

**Permisos clave.** Los 12 del catálogo (asignación total).

## 2. SCHOOL_ADMIN — Administrador de sede

**Objetivo.** Operar una sede: sus usuarios, estructura académica, horarios, justificaciones y reportes.

**Pantallas y funciones.** Las mismas seis pestañas web que el administrador global pero con alcance de sede; pantallas móviles equivalentes salvo funciones globales. Gestiona los usuarios de su sede, crea ambientes y bloques, aprueba justificaciones, revisa casos biométricos, configura la sede y exporta sus reportes.

**Secuencia típica.**

1. Ingresar y llegar al dashboard de la sede.
2. Registrar ambientes, cohortes y cursos.
3. Asignar el instructor responsable a cada cohorte y bloque, validando solapes de sala–slot e instructor–slot.
4. Importar o crear estudiantes, matricularlos y registrar sus rostros.
5. Resolver las justificaciones pendientes de su sede y los casos de actualización biométrica (`Pending → In_Review → Approved | Rejected`).
6. Generar y exportar reportes por cohorte y fecha, y notificar a la población de la sede.

**Permisos clave.** Los 12 dentro de su sede (escritura de personas, gestión de sede, gestión de matrículas, gestión de sesiones, escritura de asistencia, aprobación de justificaciones, revisión biométrica, gestión de configuración, más lectura y alertas).

## 3. INSTRUCTOR — Docente

**Objetivo.** Pasar de la lista manual a la supervisión: abrir y cerrar sesiones, registrar asistencia manual cuando falla el rostro, validar justificaciones y hacer seguimiento a las cohortes.

**Pantallas y funciones.** Web con cinco pestañas (dashboard, Alumnos solo lectura con registro de rostro, gestión de Cursos, Reportes propios con exportación, Configuración de docente; Ambientes visibles pero no gestionables). Móvil: dashboard docente, cola de justificaciones pendientes con aprobar/rechazar, visualización de asistencia, actualización de foto, consulta de justificaciones y novedades.

**Secuencia típica.**

1. Ingresar y ver las cohortes y el horario asignados.
2. Abrir la sesión de clase (única por bloque y fecha, estado `Open`).
3. Supervisar la captura facial automática (match en cliente contra el servicio biométrico, luego creación del registro con método `FACIAL`; `Present` o `Late` según la tolerancia; exactamente un registro por actor y sesión, solo en sesiones `Open`).
4. Ante fallo facial, registrar manualmente (`MANUAL`/`IMPORT`) o derivar a justificación.
5. Cerrar la sesión (`Closed` con autor y marca temporal, o `Cancelled`).
6. Validar justificaciones: revisar la cola de pendientes con motivo y soportes, aprobar (el registro pasa a `Justified`) o rechazar (conserva `Absent`/`Late`), disparando notificación automática al estudiante.
7. Consultar historial y reportes de sus cohortes y monitorear alertas de ausentismo, tardanzas, baja asistencia y justificaciones pendientes.

**Permisos clave (8).** Lectura de personas, gestión de sesiones, lectura y escritura de asistencia, aprobación de justificaciones, solicitud y revisión de casos biométricos, lectura de alertas. Excluidos explícitamente: gestión de configuración, gestión de sede, escritura de personas y notificar-a-todos.

## 4. STUDENT — Aprendiz

**Objetivo.** Registrar asistencia rápido sin depender del llamado a lista, justificar ausencias y consultar su historial personal.

**Pantallas y funciones.** Web con tres pestañas (dashboard personal, cursos solo lectura, configuración de estudiante; compañeros solo lectura; sin reportes ni ambientes). El móvil lleva el flujo principal: registro facial con confirmación de éxito (un solo embedding activo), visualización de asistencia propia, menú de justificaciones (fecha, docente, ambiente, motivo, adjuntos PDF/JPG/PNG hasta 5 MB), consulta de estado (`Pending/Approved/Rejected`), actualización de foto (verificar, escanear, validar), novedades, perfil e idioma/apariencia.

**Secuencia típica.**

1. Ingresar y completar el enrolamiento facial inicial.
2. Día a día: escaneo facial dentro de la ventana del bloque, registro `FACIAL` con confirmación; anti-duplicado por la restricción única sesión–actor; un segundo escaneo registra la salida sin duplicar la entrada.
3. Ante fallo de reconocimiento, reintentar o acudir a justificación manual.
4. Ante `Absent`/`Late`, subir justificación con soportes (1:1 con el registro, inicia en `Pending`; adjunto obligatorio cuando el tipo lo exige).
5. Recibir la notificación del resultado (in-app, correo o push) y revisar historial y reportes en alcance personal.
6. Ajustar configuración personal (alertas, tono, idioma); solicitar actualizaciones de rostro, que requieren aprobación administrativa.

**Permisos clave (4).** Lectura de personas, lectura de asistencia, solicitud de casos biométricos, lectura de alertas. Excluidos explícitamente: exportar reportes de terceros, aprobar justificaciones, gestionar usuarios, cursos o ambientes, y gestión de configuración.

# Discusión y conclusión

Los cuatro flujos forman una cadena coherente: los administradores preparan la estructura y las personas, los instructores operan las sesiones y validan excepciones, y los estudiantes producen la evidencia de asistencia y justifican ausencias. El análisis confirma la segregación de funciones mediante el catálogo de 12 permisos, con el administrador global como único rol multi-sede y el estudiante como el más restringido.

# Referencias

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7ma ed.). https://doi.org/10.1037/0000165-000

Ferreira, M., y Smith, J. (2022). Patrones de control de acceso por roles en plataformas educativas. *Journal of Software Engineering Practice, 14*(3), 45–62.

Nielsen, J. (1994). *Usability engineering*. Morgan Kaufmann.

Shneiderman, B., Plaisant, C., Cohen, M., Jacobs, S., y Elmqvist, N. (2016). *Designing the user interface: Strategies for effective human-computer interaction* (6ta ed.). Pearson.
