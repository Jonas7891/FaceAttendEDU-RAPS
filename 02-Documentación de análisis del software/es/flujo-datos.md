---
title: "Flujo de los datos dentro del sistema — FaceAttend EDU"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
date: "13 de octubre de 2026"
abstract: |
  Este documento describe el flujo que realizan los datos dentro del sistema FaceAttend EDU: puntos de entrada a través de los clientes web y móvil, enrutamiento y autenticación en el gateway Kong, propiedad de los datos por microservicio con persistencia políglota (PostgreSQL, MongoDB, Redis), comunicación asíncrona mediante Apache Kafka y puntos de salida (respuestas síncronas, correo SMTP, alertas y reportes derivados). Se basa en la implementación de FULL y en las secciones 02-domain, 05-architecture, 06-data, 07-api y 09-microservices de fae-docs.
keywords:
  - flujo de datos
  - microservicios
  - api gateway
  - persistencia políglota
  - eventos kafka
  - Normas APA 7ma edición
---

# Introducción

## Objetivo del documento

Describir de forma ordenada y verificable el recorrido que realizan los datos dentro de FaceAttend EDU, desde su entrada por los clientes hasta su almacenamiento y salida, identificando el microservicio responsable, la persistencia y los eventos involucrados en cada etapa.

## Alcance del documento

Este documento cubre el flujo de datos a nivel de análisis (etapas, propietarios, almacenes y eventos). No reemplaza los documentos de diseño (diagramas de clases, secuencia y C4) ni la documentación de código, que detallan la implementación.

## Método y fuentes

El flujo se reconstruyó a partir de la implementación en `FULL` (archivos compose, `kong.yml`, builds por servicio) y de `fae-docs` (`05-architecture`, `07-api`, `09-microservices`, `06-data`, `02-domain`, `08-uml`).

# Flujo de los datos dentro del sistema

## Etapa 0 — Entrada: clientes

Los únicos puntos de entrada en operación normal son la aplicación web Expo (servida vía nginx en el puerto 8090) y la aplicación móvil nativa. Ambas apuntan exclusivamente al gateway Kong (`http://<host>:8080`, configurado vía `EXPO_PUBLIC_API_URL`); no existe acceso directo a los microservicios, cuyos puertos (8081–8089) están ligados a loopback solo para desarrollo local. Cada llamada autenticada porta `Authorization: Bearer <sessionId>`. Las cargas son JSON con paginación `?page&limit`, excepto el registro facial desde el móvil, que usa multipart (`POST face/register`).

## Etapa 1 — Borde: gateway Kong (DB-less)

Kong 3.6 en modo DB-less (`kong.yml`) aplica, en orden: una pre-función Lua, CORS (orígenes `*`, con credenciales) y limitación de tasa de 300 peticiones por minuto respaldada en Redis, más límites por upstream (p. ej. 60/min identity, 100/min authorization, 200/min attendance). Las rutas se mapean por prefijo al microservicio dueño (`/persons|users|auth|sessions` hacia identity en 8081; `/roles|permissions|user-roles` hacia authorization en 8082; equivalentes para academic, scheduling, attendance, biometric, configuration, notification y quality). La pre-función permite sin token `OPTIONS`, health checks, login/logout/refresh y recuperación de contraseña; toda otra petición exige un identificador de sesión Bearer validado por introspección contra `ms-identity` (`GET /api/v1/sessions/{id}`), con 401 para sesiones desconocidas o no `Active` y 503 si identity no responde. El cliente nunca alcanza los datos almacenados directamente; tres redes Docker (`faceattend-edge`, `faceattend-app`, `faceattend-data`) imponen esta separación.

## Etapa 2 — Autenticación y autorización en cada llamada

El login (`POST /api/v1/auth/login` con correo o username más contraseña) verifica el hash BCrypt contra `identity.app_user.password_hash` e inserta una fila en `identity.user_session` en estado `Active`, devolviendo `{sessionId, userId}` con un 401 genérico que evita la enumeración de usuarios. El cliente carga luego su contexto (roles, evaluación de permisos, perfil de persona). Cada petición posterior pasa dos verificaciones: el chequeo de borde en Kong y la guarda del microservicio dueño (filtro de token en los servicios Java, guarda de autenticación en los TypeScript, requerimiento de permiso en Python, middleware en Go), que revalida la sesión y el permiso específico. La renovación rota el identificador de sesión (un solo uso) y el logout cierra la fila.

## Etapa 3 — Propiedad de los datos: un microservicio, un schema

Cada microservicio es dueño exclusivo de sus datos; no existen llaves foráneas entre bounded contexts, solo identificadores públicos (UUID o ID) validados por la aplicación:

- **Identity (Java, 8081):** `identity.person`, `app_user` (1:1 con persona), `user_session`, `password_policy`.
- **Authorization (Java, 8082):** `role`, `permission`, `role_permission` (clave compuesta), `user_role` (identificador de usuario sin llave foránea).
- **Academic (TypeScript, 8083):** `school`, `program`, `academic_period`, `cohort`, `course`, `academic_actor_type`, `academic_actor` (enlaza persona y sede), `enrollment` (actor–cohorte, par único).
- **Scheduling (Java, 8084):** `environment`, `schedule_block` (cohorte, curso e instructor como referencias cross-contexto; restricciones únicas de sala–slot e instructor–slot; día ISO 1–7), `class_session` (materialización de un bloque en una fecha; única por bloque–fecha; `Open → Closed | Cancelled`).
- **Attendance (Java, 8085):** `attendance_record` (sesión más actor, par único, `Present | Absent | Late | Justified`, método `FACIAL | FINGERPRINT | MANUAL | IMPORT`), `justification` (1:1 con el registro; `Pending → Approved | Rejected`), `justification_type` (global cuando `school_id` es nulo, por sede en otro caso), `supporting_document` (metadatos del archivo; el contenido vive en MinIO), `attendance_report` (proyección derivada JSONB sin llaves foráneas).
- **Biometric (Python, 8086):** colecciones MongoDB de embeddings faciales y de huella (`person_id`, versión de plantilla, encoding, versión del modelo, fecha de enrolamiento, bandera activa) más la tabla SQL canónica de casos hospedada en el schema de configuración.
- **Configuration (TypeScript, 8087):** `academic_configuration` (por sede), `security_configuration` (global), `biometric_update_case` (`Pending → In_Review → Approved | Rejected`).
- **Notification (TypeScript, 8088):** `alert_type`, `alert` (referencia al actor, marcas de raised/resolved).

Toda tabla SQL porta el bloque de auditoría (`created_at`, `updated_at`, `deleted_at`, `created_by`, `updated_by`, `deleted_by`, `row_version`) con borrado lógico y bloqueo optimista.

## Etapa 4 — Flujos de negocio

**De académico a scheduling.** Sedes, programas, períodos, cohortes, cursos, actores académicos y matrículas se crean en el servicio académico; luego ambientes, bloques horarios y sesiones de clase (apertura/cierre) se crean en scheduling. No hay llamadas entre ambos microservicios; la aplicación suministra los identificadores.

**Asistencia biométrica y manual.** El enrolamiento (`POST /biometric/facial/enroll`) inserta el embedding en MongoDB desactivando el anterior, solo si la persona aceptó los términos y, si es menor de edad, tiene acudiente con autorización firmada. El match ocurre en el servidor: el kiosco de Web envía la imagen o la muestra de huella a biometric (`POST /biometric/facial/identify-image` o `/biometric/fingerprint/identify-sample`, también por WebSocket), que compara contra las plantillas activas de la sede de quien llama y devuelve el `person_id` o "sin coincidencia". Con ese resultado el kiosco escribe el registro (`POST /attendance-records`), y quien no usa biometría se registra por la vía `MANUAL` desde la lista de la clase. La escritura es transaccional con pre-chequeo del par único sesión–actor (409 si duplica) y publica un mensaje `attendance-events` en modo best-effort. El estado de llegada lo decide attendance, no el cliente: fija `captured_at` con su propio reloj y lo compara con la fecha de la sesión y la hora de inicio del bloque (scheduling) más la tolerancia `tardy_tolerance_minutes` de la sede (configuration); pasada la tolerancia el registro queda `Late`, si no `Present`. Si scheduling o configuration no responden, el registro no se guarda (503). Attendance no llama a biometric.

**Cierre de clase.** Al cerrar una sesión (`Open → Closed`), scheduling publica `scheduling.class-session.closed`. Notification lo consume, registra como `Absent` (vía `POST /attendance-records`, con su cuenta de servicio) a los aprendices de la ficha sin registro y envía al instructor el correo con las ausencias. Las alertas de riesgo por baja asistencia salen de un pase diario programado de notification, no de este evento.

**Justificación.** La justificación se crea en estado `Pending` (1:1 con un registro `Absent` o `Late`). Los soportes se suben a attendance, que guarda el archivo en MinIO y los metadatos en `supporting_document`. La revisión la transiciona a `Approved` o `Rejected`, exigiendo adjunto cuando el tipo lo requiere. Aprobarla deja el registro de asistencia en `Justified` en la misma transacción y publica `attendance.justification.events`, que notification consume para crear la alerta que el aprendiz ve en Novedades.

**Caso biométrico.** Las solicitudes de actualización siguen un flujo de auditoría (`Pending → In_Review → Approved | Rejected`) con solicitante obligatorio y número de dedo restringido a 1–10 para huellas (nulo para facial).

**Recuperación de contraseña.** La solicitud siempre devuelve 202 para evitar enumeración (cooldown de 60 segundos, código de 6 dígitos SHA-256 en memoria con TTL de 10 minutos y 5 intentos); es la única llamada de negocio entre servicios, de identity a notification por la red interna para enviar el correo; la verificación y el reseteo validan la política de contraseñas, aplican hash BCrypt, cierran sesiones activas y publican el evento `PasswordChanged`.

## Etapa 5 — Persistencia

Los datos relacionales viven en PostgreSQL 17 (`faceattend_db`), un schema por bounded context, migrados con Liquibase por microservicio. Los vectores biométricos viven en MongoDB 7 (`faceattend_biometric`). Redis 7 solo guarda el estado del rate-limit de Kong; las sesiones persisten en PostgreSQL y los códigos de recuperación en memoria o Redis. Los archivos (soportes de justificaciones y autorizaciones firmadas de acudientes) viven en MinIO (ADR-009), con cifrado en reposo; PostgreSQL guarda solo sus metadatos.

## Etapa 6 — Eventos Kafka

Los servicios publican eventos de dominio en modo fire-and-forget best-effort (errores solo logueados, sin outbox, cola de muertos ni reintentos): eventos de identity, ciudad y política de contraseñas; de rol y permiso; académicos, de sesión de clase, ambiente, bloque, asistencia, justificación y configuración. En runtime hay dos consumidores, ambos en notification: `scheduling.class-session.closed` (registra los ausentes de la clase y envía al instructor el correo de ausencias) y `attendance.justification.events` (crea la alerta con el resultado de la revisión que el aprendiz ve en Novedades). Ambos son de entrega al menos una vez: reintentan con espera creciente y descartan con aviso los mensajes inválidos. Los demás eventos no tienen consumidor: `PasswordChanged` no invalida nada remoto y los eventos `attendance-events` no crean alertas por sí solos. El catálogo de eventos describe la arquitectura objetivo, no solo el runtime actual.

## Etapa 7 — Salidas

Respuestas JSON síncronas vía Kong (2xx/4xx/429/503). El correo sale exclusivamente por el servicio de notificaciones (SMTP desde el entorno; host vacío produce `202 {skipped}`). Las alertas persisten como filas y opcionalmente se propagan a un webhook configurado firmado con HMAC, o no-op. Los reportes son proyecciones derivadas JSONB leídas bajo demanda, sin pipeline ETL.

# Discusión y conclusión

El flujo confirma el principio rector de que cada microservicio es dueño de sus datos: la entrada se centraliza en Kong, la propiedad se particiona por schema y la consistencia entre contextos es eventual y validada por la aplicación, no impuesta referencialmente. Las brechas documentadas (sin validación cross-contexto, productores sin consumidores, documentos solo como metadatos) son hallazgos de análisis que las fases de diseño e implementación deben atender explícitamente.

# Referencias

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). https://doi.org/10.1037/0000165-000

Apache Software Foundation. (2024). *Documentación de Apache Kafka*. https://kafka.apache.org/documentation/

Fielding, R. T. (2000). *Architectural styles and the design of network-based software architectures* (Tesis doctoral, University of California, Irvine).

Kong Inc. (2024). *Documentación de Kong Gateway (modo DB-less)*. https://docs.konghq.com/gateway/

Newman, S. (2021). *Building microservices* (2da ed.). O'Reilly Media.
