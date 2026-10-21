# Guía del Modelo de Datos — FaceAttend-Edu 

## 1. Visión general

FaceAttend-Edu es una plataforma de **gestión de asistencia mediante reconocimiento biométrico** para instituciones educativas. El modelo de datos soporta:

- Gestión de personas, usuarios y credenciales
- Estructura académica (sedes, programas, periodos, cohortes, cursos, actores y matrículas)
- Horarios y sesiones de clase
- Registro de asistencia con múltiples fuentes (facial, manual, IoT, import)
- Justificaciones con soporte documental
- Actualización de plantillas biométricas con flujo de aprobación (`biometric_update_case` unifica FACIAL/FINGERPRINT)
- Plantillas biométricas (facial y dactilar) almacenadas como **colecciones NoSQL en MongoDB**
- Alertas y notificaciones
- Configuración académica y de seguridad
- Evaluación de calidad de los microservicios (ISO/IEC 25010, ISO/IEC 29110, ISTQB) — contexto **Quality**

---

## 2. Bounded Contexts (Contextos delimitados)

2. Bounded Contexts (Contextos delimitados)

El modelo de dominio de FACEATTEND-EDU (v6 — DBML) se organiza en 9 bounded contexts, cada uno responsable de un área funcional del negocio. Esta división permite delimitar responsabilidades, organizar las entidades del modelo y diferenciar los mecanismos de persistencia de cada contexto.

2.1. Vista general de los contextos
flowchart TB
    ROOT(["FACEATTEND-EDU<br/>Modelo de dominio · v6"])

    ROOT --> ID
    ROOT --> AC
    ROOT --> OP
    ROOT --> SYS

    subgraph ID["IDENTIDAD Y CONTROL DE ACCESO"]
        direction LR
        I["IDENTITY · 4 tablas<br/><br/>person<br/>app_user<br/>user_session<br/>password_policy"]
        AU["AUTHORIZATION · 4 tablas<br/><br/>role<br/>permission<br/>role_permission<br/>user_role"]
    end

    subgraph AC["GESTIÓN ACADÉMICA"]
        direction LR
        A["ACADEMIC · 8 tablas<br/><br/>school · program<br/>academic_period · cohort<br/>course<br/>academic_actor_type<br/>academic_actor<br/>enrollment"]
        S["SCHEDULING · 3 tablas<br/><br/>environment<br/>schedule_block<br/>class_session"]
    end

    subgraph OP["ASISTENCIA Y BIOMETRÍA"]
        direction LR
        AT["ATTENDANCE · 5 tablas<br/><br/>attendance_record<br/>justification_type<br/>justification<br/>supporting_document<br/>attendance_report"]
        BIO["BIOMETRIC · NoSQL<br/><br/>facial_emb<br/>fingerprint_emb<br/><br/>MongoDB"]
    end

    subgraph SYS["CONFIGURACIÓN, NOTIFICACIONES Y CALIDAD"]
        direction LR
        C["CONFIGURATION · 3 tablas<br/><br/>academic_config<br/>security_config<br/>biometric_update_case"]
        N["NOTIFICATION · 2 tablas<br/><br/>alert_type<br/>alert"]
        Q["QUALITY · 7 tablas<br/><br/>quality_project<br/>quality_evaluation<br/>quality_evaluation_item<br/>process_assessment<br/>process_assessment_rating<br/>istqb_assessment<br/>istqb_assessment_item"]
    end

    classDef root fill:#172554,color:#ffffff,stroke:#3b82f6,stroke-width:2px
    classDef context fill:#e0f2fe,color:#0c4a6e,stroke:#0284c7,stroke-width:1px
    classDef nosql fill:#fef3c7,color:#78350f,stroke:#d97706,stroke-width:1px
    classDef quality fill:#f3e8ff,color:#581c87,stroke:#9333ea,stroke-width:1px

    class ROOT root
    class I,AU,A,S,AT,C,N context
    class BIO nosql
    class Q quality

2.2. Descripción de los contextos
Bounded Context	Tablas / almacenamiento	Responsabilidad
IDENTITY	4 tablas	Gestión de personas, usuarios, sesiones y políticas de contraseñas.
AUTHORIZATION	4 tablas	Administración de roles, permisos y asignaciones de acceso.
ACADEMIC	8 tablas	Gestión de instituciones, programas, periodos académicos, cohortes, cursos, actores académicos y matrículas.
SCHEDULING	3 tablas	Gestión de ambientes, bloques de horarios y sesiones de clase.
ATTENDANCE	5 tablas	Registro de asistencia, justificaciones, documentos de soporte e informes.
BIOMETRIC	NoSQL (MongoDB)	Almacenamiento de representaciones biométricas faciales y dactilares.
CONFIGURATION	3 tablas	Administración de configuraciones académicas, de seguridad y casos de actualización biométrica.
NOTIFICATION	2 tablas	Gestión de tipos de alerta y alertas generadas por el sistema.
QUALITY	7 tablas	Gestión de proyectos, evaluaciones de calidad, valoraciones de procesos y evaluaciones ISTQB.
2.3. Relación entre usuarios y personas

En el contexto IDENTITY, la columna app_user.person_id tiene una restricción UNIQUE, destinada a establecer una relación uno a uno entre app_user y person.

erDiagram
    PERSON o|--o| APP_USER : "person_id UNIQUE"

    PERSON {
        bigint id PK
    }

    APP_USER {
        bigint id PK
        bigint person_id FK, UK
    }


La cardinalidad uno a uno representa que una persona puede estar asociada a un único usuario y que un usuario puede estar asociado a una única persona. Para garantizar que todo usuario tenga una persona asociada, person_id también debe ser NOT NULL; la clave foránea debe estar definida para garantizar la integridad referencial.

2.4. Consideraciones de persistencia e infraestructura
Persistencia relacional: los contextos IDENTITY, AUTHORIZATION, ACADEMIC, SCHEDULING, ATTENDANCE, CONFIGURATION, NOTIFICATION y QUALITY se representan mediante tablas relacionales.
Persistencia documental: BIOMETRIC utiliza MongoDB para almacenar las colecciones facial_emb y fingerprint_emb.
Ubicación física de QUALITY: aunque constituye un bounded context independiente desde la perspectiva del dominio, sus 7 tablas están alojadas en el esquema quality de la base de datos 07-ms-configuration-db.
Gestión de migraciones: no existe un servicio quality-liquibase independiente; las tablas de QUALITY se encuentran físicamente dentro de la base de datos asociada a CONFIGURATION.

En consecuencia, el modelo contempla 9 bounded contexts de negocio, aunque QUALITY comparte infraestructura de persistencia con CONFIGURATION. Esta ubicación física no implica que ambos contextos deban considerarse un único contexto delimitado desde el punto de vista conceptual.
---

## 3. Contexto: Identity

**Responsabilidad:** Identidad de personas, credenciales de acceso, sesiones y políticas de contraseña.

| Tabla | Descripción | PK |
|---|---|---|
| `person` | Identidad base: documento, nombre, contacto | `person_id` (UUID) |
| `app_user` | Credenciales de acceso; 1:1 con `person` (`person_id` UNIQUE) | `user_id` (UUID) |
| `user_session` | Sesiones activas/cerradas | `session_id` (UUID) |
| `password_policy` | Reglas de complejidad de contraseña | `policy_id` (INT) |

**Relaciones internas (FK real):**
- `app_user.person_id` → `person.person_id`
- `user_session.user_id` → `app_user.user_id`

**Diseño clave:** `person` **no tiene** `school_id`. La identidad es agnóstica de sede.

> **Nota histórica:** la tabla `city` existió en este contexto pero fue **eliminada** (changeset `008-drop-city-table.yaml`). La referencia geográfica a ciudad se resuelve hoy mediante un catálogo externo (API countriesnow), sin FK ni tabla propia.

---

## 4. Contexto: Authorization

**Responsabilidad:** Control de acceso basado en roles (RBAC).

| Tabla | Descripción | PK |
|---|---|---|
| `role` | Catálogo de roles del sistema | `role_id` (INT) |
| `permission` | Catálogo de permisos atómicos | `permission_id` (INT) |
| `role_permission` | Asignación N:N roles ↔ permisos | `(role_id, permission_id)` |
| `user_role` | Asignación N:N usuarios ↔ roles | `(user_id, role_id)` |

**Cross-context (sin FK):**
- `user_role.user_id` → `Identity.app_user.user_id`

**Diseño clave:** `role` **no tiene** `school_id`. Los roles son globales.

---

## 5. Contexto: Academic

**Responsabilidad:** Estructura académica: sedes, programas, periodos, cohortes, cursos, actores y matrículas.

| Tabla | Descripción | PK |
|---|---|---|
| `school` | Sede educativa | `school_id` (INT) |
| `program` | Oferta curricular de una sede | `program_id` (INT) |
| `academic_period` | Ventana temporal (trimestre/semestre) | `academic_period_id` (INT) |
| `cohort` | Grupo de estudiantes en programa + periodo | `cohort_id` (BIGINT) |
| `course` | Materia/asignatura de un programa | `course_id` (INT) |
| `academic_actor_type` | Catálogo: STUDENT / INSTRUCTOR / ADMIN (administrador de sede) | `actor_type_id` (SMALLINT) |
| `academic_actor` | Rol que una persona cumple en una sede | `academic_actor_id` (BIGINT) |
| `enrollment` | Matrícula de actor en una cohorte | `enrollment_id` (BIGINT) |

**Relaciones internas (FK real):**
school → program → course school → program → cohort ← academic_period academic_actor → academic_actor_type academic_actor → school enrollment → academic_actor enrollment → cohort


**Cross-context (sin FK):**
- `academic_actor.person_id` → `Identity.person.person_id`

**Diferencias clave:**
- **`program`**: Contenedor curricular de más alto nivel ("Tecnología en Análisis de Datos")
- **`course`**: Unidad de contenido dentro de un programa ("Bases de Datos I")
- **`cohort`**: Grupo concreto de estudiantes que avanza junto ("Ficha 2691234 — ADSI — Trimestre 1 de 2026")

---

## 6. Contexto: Scheduling

**Responsabilidad:** Traduce la estructura académica en horarios y sesiones concretas.

| Tabla | Descripción | PK |
|---|---|---|
| `environment` | Espacio físico (aula, laboratorio) | `environment_id` (INT) |
| `schedule_block` | Bloque recurrente semanal | `schedule_block_id` (BIGINT) |
| `class_session` | Materialización en una fecha concreta | `class_session_id` (BIGINT) |

**Cross-context (sin FK):**
- `environment.school_id` → `Academic.school.school_id`
- `schedule_block.cohort_id` → `Academic.cohort.cohort_id`
- `schedule_block.course_id` → `Academic.course.course_id`
- `schedule_block.instructor_actor_id` → `Academic.academic_actor.academic_actor_id`
- `class_session.opened_by` / `class_session.closed_by` → `Academic.academic_actor.academic_actor_id`

**Índices de negocio (únicos):**
- `(environment_id, day_of_week, starts_at)` → evita doble reserva del mismo ambiente.
- `(instructor_actor_id, day_of_week, starts_at)` → evita doble asignación del mismo instructor.

**Puente hacia Attendance:** `class_session` es la entidad que recibe registros de asistencia.

---

## 7. Contexto: Attendance

**Responsabilidad:** Registro de asistencia, justificaciones, soportes documentales y reportes.

| Tabla | Descripción | PK |
|---|---|---|
| `attendance_record` | Un registro por actor y sesión | `attendance_record_id` (BIGINT) |
| `justification_type` | Catálogo de tipos de justificación | `justification_type_id` (INT) |
| `justification` | Justificación de inasistencia/tardanza | `justification_id` (BIGINT) |
| `supporting_document` | Soportes documentales adjuntos | `supporting_document_id` (BIGINT) |
| `attendance_report` | Reporte de asistencia generado (snapshot JSON) | `report_id` (UUID) |

**Relaciones internas (FK real):**
- `justification.attendance_record_id` → `attendance_record.attendance_record_id`
- `justification.justification_type_id` → `justification_type.justification_type_id`
- `supporting_document.justification_id` → `justification.justification_id`

**Cross-context (sin FK):**
- `attendance_record.class_session_id` → `Scheduling.class_session.class_session_id`
- `attendance_record.academic_actor_id` → `Academic.academic_actor.academic_actor_id`
- `justification.reviewed_by` → `Identity.app_user.user_id`
- `attendance_report.school_id` / `cohort_id` → `Academic`
- `attendance_report.generated_by` → `Identity.app_user.user_id`

**Regla de negocio (índice único):** `(class_session_id, academic_actor_id)` es **único** → solo puede existir **un registro de asistencia por actor por sesión de clase**.

**ENUMs:**
- `attendance_status`: Present, Absent, Late, Justified
- `capture_method`: FACIAL, MANUAL, IOT, IMPORT
- `review_status`: Pending, Approved, Rejected

---

## 8. Contexto: Biometric

**Responsabilidad:** Plantillas biométricas (facial y dactilar).

**No forma parte del esquema relacional SQL.** Se modela como colecciones NoSQL en **MongoDB** (fuera del modelo relacional DBML):

| Colección | Campos clave |
|---|---|
| `facial_embedding` | `person_id`, `template_version`, `encoding`, `model_version`, `enrolled_at`, `is_active` |
| `fingerprint_embedding` | `person_id`, `finger_number`, `template_version`, `encoding`, `model_version`, `enrolled_at`, `is_active` |

**¿Por qué NoSQL?** Los embeddings biométricos son documentos de estructura flexible y de alto volumen de lectura/escritura por reconocimiento en tiempo real.

**En PostgreSQL solo existe el schema `biometric` vacío** (`06-ms-biometric-db` solo crea el schema, sin tablas). La referencia lógica desde `configuration.biometric_update_case.current_embedding_ref` apunta al `_id` del documento en MongoDB (cross-paradigm, sin FK).

---

## 9. Contexto: Configuration

**Responsabilidad:** Parámetros configurables y casos de actualización biométrica.

| Tabla | Descripción | PK |
|---|---|---|
| `academic_configuration` | Parámetros por sede | `configuration_id` (INT) |
| `security_configuration` | Parámetros globales de seguridad | `configuration_id` (INT) |
| `biometric_update_case` | Solicitud de actualización biométrica | `case_id` (UUID) |

**`biometric_update_case`** unifica los casos de actualización facial y dactilar:
- `biometric_type`: FACIAL / FINGERPRINT (ENUM)
- `finger_number`: Solo para FINGERPRINT (1..10), nulo para FACIAL
- `current_embedding_ref`: Referencia lógica al documento activo en NoSQL

---

## 9.5 Contexto: Quality

**Responsabilidad:** Instrumento de evaluación de calidad (ISO/IEC 25010, ISO/IEC 29110, ISTQB) sobre los demás microservicios. Dueño lógico: `09-ms-quality`; hospedaje físico transitorio en el schema `quality` dentro de `07-ms-configuration-db`, porque no existe un servicio `quality-liquibase` independiente — deuda documentada, no mover sin migración compatibilizada.

| Tabla | Descripción | PK |
|---|---|---|
| `quality_project` | Proyecto de software evaluado (ISO/IEC 29110) | `project_id` (BIGINT) |
| `quality_evaluation` | Evaluación ISO/IEC 25010 de un microservicio | `evaluation_id` (BIGINT) |
| `quality_evaluation_item` | Puntaje 1-5 por subcaracterística 25010 | `evaluation_item_id` (BIGINT) |
| `process_assessment` | Valoración de proceso ISO/IEC 29110 (PM/SI) | `assessment_id` (BIGINT) |
| `process_assessment_rating` | Calificación N/P/L/F por objetivo 29110 | `process_rating_id` (BIGINT) |
| `istqb_assessment` | Evaluación ISTQB CTFL v4.0 de un microservicio | `istqb_assessment_id` (BIGINT) |
| `istqb_assessment_item` | Puntaje 1-5 por ítem ISTQB | `istqb_assessment_item_id` (BIGINT) |

**Relaciones internas (FK real):**
- `quality_evaluation.project_id` → `quality_project.project_id`
- `quality_evaluation_item.evaluation_id` → `quality_evaluation.evaluation_id`
- `process_assessment.project_id` → `quality_project.project_id`
- `process_assessment_rating.assessment_id` → `process_assessment.assessment_id`
- `istqb_assessment.project_id` → `quality_project.project_id`
- `istqb_assessment_item.istqb_assessment_id` → `istqb_assessment.istqb_assessment_id`

Los catálogos de subcaracterísticas/objetivos/ítems (ej. `tech-bva`, `PM.O1`) viven en código (`09-ms-quality/src/domain/{iso25010,iso29110,istqb}.ts`), no en tablas: el instrumento es inmutable, así que no llevan FK.

---

## 10. Contexto: Notification

**Responsabilidad:** Tipos de alerta y alertas generadas sobre actores académicos.

| Tabla | Descripción | PK |
|---|---|---|
| `alert_type` | Catálogo de tipos de alerta | `alert_type_id` (SMALLINT) |
| `alert` | Alerta generada sobre un actor | `alert_id` (BIGINT) |

**Relación interna (FK real):**
- `alert.alert_type_id` → `alert_type.alert_type_id`

**Cross-context (sin FK):**
- `alert.academic_actor_id` → `Academic.academic_actor.academic_actor_id`

---

## 11. Regla más importante: Sin FK entre contextos

> **Toda referencia entre contextos distintos se documenta como comentario, nunca como `Ref:` activa.**

Esto significa que en el código SQL/Liquibase:

```yaml
# ✅ CORRECTO - sin FK real
- column:
    name: person_id
    type: UUID
    remarks: "Cross-context reference to Identity.person (NO FK)"

# ❌ INCORRECTO - FK real entre contextos
- addForeignKeyConstraint:
    baseColumnNames: person_id
    referencedTableName: person
    referencedTableSchemaName: identity
Excepción: Las referencias dentro del mismo contexto SÍ usan FK reales (ej: enrollment.cohort_id → cohort.cohort_id, ambas en Academic).
```

## 12. Diagrama de relaciones (simplificado)

**El siguiente diagrama resume las principales relaciones entre los contextos del modelo de datos. Las referencias entre contextos son lógicas y no representan claves foráneas físicas.**

```yaml
identity.person ─────────────────────────────────────────┐
    │ 1:1                                                │
    ▼                                                    │
identity.app_user                              academic.academic_actor
    │                                                  │    │
    │ (cross-context)                                  │    │
    ▼                                                  │    │
authorization.user_role                        enrollment   │
                                                   │        │
                                       academic.cohort  ◄───┘
                                           │
                                           │
                               scheduling.schedule_block
                                           │
                                           ▼
                               scheduling.class_session
                                           │
                                           ▼
                               attendance.attendance_record
                                           │
                                           ▼
                               attendance.justification
                                           │
                                           ▼
                               attendance.supporting_document
```

## 13. ENUMs del modelo

Los **ENUMs** se implementan como tipos nativos de PostgreSQL (CREATE TYPE ... AS ENUM), no como columnas VARCHAR con validación textual. Aunque el DBML los documenta como varchar con la nota ENUM, la implementación física utiliza tipos nativos.

A continuación, se presentan los 14 tipos ENUM reales, con sus valores exactos según los changesets de Liquibase.

```yaml
Contexto	Tipo ENUM (PostgreSQL)	Valores exactos
Identity	user_session_status	Active, Closed
Identity	authentication_type	Local, Windows, External
Academic	enrollment_status	Active, Withdrawn, Completed
Scheduling	class_session_status	Open, Closed, Cancelled
Attendance	attendance_status	Present, Absent, Late, Justified
Attendance	capture_method	FACIAL, MANUAL, IOT, IMPORT
Attendance	review_status	Pending, Approved, Rejected
Configuration	biometric_type	FACIAL, FINGERPRINT
Configuration	update_status	Pending, In_Review, Approved, Rejected
Quality	project_status	Planned, Active, Closed
Quality	evaluation_status	Draft, Completed
Quality	iso29110_process	PM, SI
Quality	objective_rating	N, P, L, F
Quality	quality_level	Deficiente, En proceso, Aceptable, Bueno, Excelente
```

## 14. Auditoría y buenas prácticas transversales

Toda tabla del modelo incluye un bloque estándar de auditoría y control de concurrencia.

### 14.1. Campos de auditoría

```yaml
Columna	Tipo	Propósito
created_at	datetime	Fecha de creación del registro.
updated_at	datetime	Fecha de la última modificación.
deleted_at	datetime (nullable)	Borrado lógico: NULL indica que la fila está activa.
created_by	UUID	Usuario que creó el registro.
updated_by	UUID	Usuario que modificó el registro.
deleted_by	UUID	Usuario que eliminó el registro lógicamente.
row_version	BIGINT (default 1)	Control optimista de concurrencia (optimistic locking).
```

### 14.2. Reglas asociadas
**Borrado lógico:** el valor deleted_at IS NULL identifica las filas activas.
Índices únicos parciales: los índices únicos sensibles al borrado lógico utilizan cláusulas WHERE deleted_at IS NULL; por ejemplo, uq_person_email.
**Tipos ENUM:** se crean como tipos nativos de PostgreSQL durante la fase 01-ddl/02-types/ de Liquibase.
**Integridad referencial:** las claves foráneas reales se utilizan únicamente dentro de cada contexto, de acuerdo con la regla descrita en la sección 11.

## 15. Capa física (implementación)
### 15.1. Topología de bases de datos

***PostgreSQL***

**Base de datos:** faceattend_db.
Desarrollo: una base de datos con ocho esquemas principales (identity, authorization, academic, scheduling, attendance, biometric, configuration y notification), además del esquema quality, alojado en la misma base de datos.
Producción: PostgreSQL serverless en Neon. Para más detalles, consultar DESPLIEGUE.md y database/docker-compose.neon.yml.

***MongoDB***

Almacena las colecciones de embeddings biométricos:
facial_embedding
fingerprint_embedding

**Alternativa de despliegue:** existe el archivo database/scripts/init-multidb.sql, que permite una configuración con ocho bases de datos independientes. Sin embargo, el entorno de desarrollo utiliza una sola base de datos con múltiples esquemas.

### 15.2. Mapeo de carpeta → esquema → contexto

La siguiente tabla relaciona las carpetas de Liquibase con los esquemas, contextos y cantidades de tablas correspondientes.

```yaml
Carpeta Liquibase	Esquema	Contexto	Tablas
01-ms-identity-db	identity	Identity	4
02-ms-authorization-db	authorization	Authorization	4
03-ms-academic-db	academic	Academic	8
04-ms-scheduling-db	scheduling	Scheduling	3
05-ms-attendance-db	attendance	Attendance	5
06-ms-biometric-db	biometric	Biometric	0 (solo esquema; colecciones en MongoDB)
07-ms-configuration-db	configuration	Configuration	3
08-ms-notification-db	notification	Notification	2
Hospedado en 07-ms-configuration-db	quality	Quality	7
Total			36 tablas SQL + 2 colecciones NoSQL
```

### 15.3. Ciclo de migración Liquibase

Cada dominio ejecuta sus cambios siguiendo el siguiente orden:

01-ddl → 02-dml → 03-dcl → 04-tcl


Las fases tienen las siguientes responsabilidades:

```yaml
Fase	Responsabilidad
01-ddl	Creación de esquemas, tipos, tablas y triggers.
02-dml	Inserción de datos iniciales (seeds).
03-dcl	Configuración de roles y permisos (GRANT).
04-tcl	Funciones y triggers implementados en PL/pgSQL.
```

Estructura por dominio:

```yaml
changelog/changelog-master.yaml: archivo maestro de migraciones.
01-ddl/: cambios de definición de datos, organizados en subdirectorios (01-schemas a 08-triggers).
02-dml/: datos iniciales con context: seed.
03-dcl/: control de acceso y permisos.
04-tcl/: funciones y lógica transaccional.
05-rollbacks/: actualmente vacío; contiene databaseChangeLog: [].
```

Versión de la imagen de Liquibase: liquibase/liquibase: 4.29.0.

### 15.4. Deudas y limitaciones físicas documentadas

Quality sin servicio Liquibase propio: el contexto Quality se aloja temporalmente en el esquema quality de 07-ms-configuration-db. 
No debe trasladarse sin una migración compatibilizada.

Eliminación de city: la referencia geográfica school.city_id apunta a un catálogo externo (API countriesnow), 
sin una tabla propia ni una clave foránea local.

Referencia entre SQL y MongoDB: biometric_update_case.current_embedding_ref apunta al _id de un documento de MongoDB.
Al tratarse de una referencia entre paradigmas de persistencia, no existe integridad referencial física entre ambos sistemas.