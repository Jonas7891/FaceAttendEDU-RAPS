# Dependencias y librerías — FaceAttend EDU

Fuente de verdad: archivos de build en `FULL/back-end` y `FULL/front-end` (relevado 2026-10-13). Las versiones son las declaradas; los starters de Spring Boot heredan su versión del BOM padre salvo indicación.

## 1. Back-end

### 01-ms-identity — Java 21 / Spring Boot 4.1.1 (Maven, `identity-service:0.0.1-SNAPSHOT`)

| Dependencia | Versión | Uso |
|---|---|---|
| spring-boot-starter-webmvc | BOM 4.1.1 | API REST (`/persons`, `/users`, `/auth`, `/sessions`) |
| spring-boot-starter-data-jpa | BOM | Persistencia JPA/Hibernate (schema `identity`) |
| spring-boot-starter-data-redis | BOM | Caché/sesiones en Redis (solo identity lo tiene) |
| spring-boot-starter-security | BOM | Filtro de autenticación, hash de contraseña |
| spring-boot-starter-validation | BOM | Validación Bean de DTOs |
| spring-boot-starter-kafka | BOM | Eventos entre MS |
| spring-boot-starter-liquibase | BOM | Migraciones versionadas del schema |
| spring-boot-starter-actuator | BOM | Salud/métricas |
| springdoc-openapi-starter-webmvc-ui | 3.1.0 | OpenAPI/Swagger UI |
| postgresql (runtime) | BOM | Driver PostgreSQL 17 |
| lombok (optional) | BOM | Reduce boilerplate |
| slices *-test + h2 (test) | BOM / 2.4.240 | Pruebas con BD embebida |

### 02-ms-authorization — Java 21 / Spring Boot 4.1.1 (`authorization-service`)

Mismo conjunto que identity **menos Redis**: webmvc, data-jpa, security, validation, kafka, liquibase, actuator, springdoc 3.1.0, postgresql, lombok, slices de test + h2. Endpoints RBAC (`/roles`, `/permissions`, `/user-roles`, `/auth/evaluate`).

### 03-ms-academic — Node + TypeScript (`academic-service:0.1.0`)

| Dependencia | Versión | Uso |
|---|---|---|
| fastify | ^4.26.0 | Framework HTTP |
| drizzle-orm | ^0.31.0 | ORM SQL (único MS Node con Drizzle) |
| pg | ^8.11.0 | Driver PostgreSQL |
| zod | ^3.22.0 | Validación de esquemas DTO |
| kafkajs | ^2.2.4 | Cliente Kafka |
| pino | ^8.16.0 | Logging estructurado |
| @opentelemetry/sdk-node | ^0.45.0 | Trazas OTel (único Node con OTel) |
| typescript / ts-node / ts-node-dev / @types/node | ^5.4/^10.9/^2.0/^20.11 | Compilación y dev |
| drizzle-kit / @types/pg | ^0.20/^8.10 | Migraciones Drizzle + tipos PG |
| jest / ts-jest / @types/jest / supertest / testcontainers | ^29.7/^29.1/^29.5/^6.3/^10.4 | Pruebas unitarias, HTTP e integración con contenedores |
| eslint | ^8.56.0 | Linter (solo academic lo declara) |

### 04-ms-scheduling — Java 21 / Spring Boot 4.1.1 (`scheduling-service`)

Mismo conjunto que authorization: webmvc, data-jpa, security, validation, kafka, liquibase, actuator, springdoc 3.1.0, postgresql, lombok, slices de test + h2. Gestiona ambientes, bloques horarios y sesiones de clase.

### 05-ms-attendance — Java 21 / Spring Boot 4.1.1 (`attendance-service`)

Mismo conjunto que authorization/scheduling. Gestiona registros de asistencia, justificaciones y reportes.

### 06-ms-biometric — Python 3.12 / FastAPI / Poetry (`biometric-service:0.1.0`)

| Dependencia | Versión | Uso |
|---|---|---|
| fastapi | ^0.110.0 | Framework API |
| uvicorn[standard] | ^0.28.0 | Servidor ASGI |
| pydantic / pydantic-settings | ^2.6/^2.1 | Modelos/validación y config por entorno |
| motor / pymongo | ^3.3/^4.6 | Acceso a MongoDB (embeddings, async + sync) |
| opencv-python-headless | ^4.9.0 | Procesamiento de imagen sin libGL/X11 |
| numpy | ^1.26.0 | Álgebra de vectores/embeddings |
| face-recognition | ^1.3.0 | Cómputo del embedding facial (dlib) |
| face-recognition-models (git) | repo ageitgey | Datos del modelo (requerimiento explícito) |
| setuptools | <81 | Provee `pkg_resources` que exige el paquete de modelos |
| Pillow | ^10.0.0 | I/O de imágenes |
| python-multipart | ^0.0.9 | Cargas multipart |
| structlog | ^24.1.0 | Logging estructurado |
| httpx | ^0.27.0 | Cliente HTTP |
| pytest / pytest-asyncio / testcontainers[mongodb] (dev) | ^8.0/^0.23/^4.0 | Pruebas async + Mongo en contenedor |

### 07-ms-configuration — Node + TypeScript (`configuration-service:0.1.0`)

fastify ^4.26.0, kafkajs ^2.2.4, pg ^8.11.0 (SQL directo, sin ORM), pino ^8.16.0, zod ^3.22.0; dev: jest/ts-jest/@types/jest/typescript/ts-node/ts-node-dev/@types/node/@types/pg.

### 08-ms-notification — Go 1.22 (`github.com/faceattend/notification-service`)

| Dependencia directa | Versión | Uso |
|---|---|---|
| github.com/gin-gonic/gin | v1.9.1 | Router/framework HTTP |
| github.com/go-playground/validator/v10 | v10.16.0 | Validación de structs |
| github.com/golang-migrate/migrate/v4 | v4.17.0 | Migraciones SQL |
| github.com/jackc/pgx/v5 | v5.5.0 | Driver/pool PostgreSQL |
| github.com/stretchr/testify | v1.8.4 | Aserciones/mocks en pruebas |
| go.opentelemetry.io/otel | v1.24.0 | Trazas OTel |
| go.uber.org/zap | v1.26.0 | Logger estructurado de alto rendimiento |

### 09-ms-quality — Node + TypeScript (`quality-service:0.1.0`)

Conjunto idéntico a 07-configuration (fastify, kafkajs, pg, zod, pino + toolchain TS/Jest).

### 10-ms-face-auth — (sin build)

El directorio solo contiene `.pytest_cache`; sin `pyproject.toml`, `requirements*.txt` ni Dockerfile funcional. La lógica facial vive hoy en `06-ms-biometric`.

### 99-api-gateway — Kong DB-less (sin dependencias de código)

Configuración declarativa en `kong/kong.yml` (`_format_version: "3.0"`). Las "librerías" son plugins Kong: `cors` (orígenes `*`, métodos GET/POST/PUT/PATCH/DELETE/OPTIONS, credenciales) y `rate-limiting` (p. ej. 60/min identity, 100/min authorization; `policy: redis`, `fault_tolerant: true`, `redis_host: redis:6379`). Infra asociada: Kong + Redis.

## 2. Front-end (Expo SDK 57 + React 19.2.3 + RN 0.86.3 + React Navigation 7)

### Web (`front-end/Web`, `FaceAttend-EDU v1.0.0`)

40 dependencias, entre ellas: `@digitalpersona/fingerprint` ^1.0.0 y `@digitalpersona/websdk` ^1.1.0 (captura de huella; websdk vendorizado vía `scripts/copy-digitalpersona.mjs`), `@vladmandic/face-api` ^1.7.14 (reconocimiento facial en web), `xlsx` ^0.18.5 (import/export de reportes), `tinycolor2` ^1.6.0 (temas), `@gorhom/bottom-sheet` latest, paquetes expo (constants, document-picker, font, haptics, image, image-picker, linear-gradient, linking, splash-screen, status-bar, symbols, system-ui, web-browser), navegación (bottom-tabs ^7.4.0, elements ^2.6.3, native ^7.2.2, native-stack ^5.0.5), gesture-handler ~2.32.0, reanimated 4.5.1, safe-area-context ~5.7.0, screens ~4.26.0, svg 15.15.4, async-storage 2.2.0, datetimepicker 9.1.0, slider 5.2.0. Dev: typescript ~6.0.3, eslint ^9.25.0, eslint-config-expo ~57.0.2, jest ^29.7.0, jest-expo ~57.0.5, @types/react ~19.2.4.

### Mobile (`front-end/Mobile`, `faceattendedu v1.0.0`)

27 dependencias. Solo-Mobile: `axios` ^1.16.1, `expo-camera` ~57.0.4 (captura facial), `expo-secure-store` ~57.0.3 (tokens), `expo-file-system` ~57.0.6, `expo-localization` ~57.0.1, `i18next` ^26.0.4 + `react-i18next` ^17.0.2, `jwt-decode` ^4.0.0, datetimepicker 8.4.4. NO incluye DigitalPersona, face-api ni xlsx. Dev: typescript ~5.9.2, jest ^29.7.0, jest-expo ~57.0.5, @react-native/jest-preset ^0.86.3, @types/react ~19.1.10.

## 3. Notas transversales

- Java (01/02/04/05): stack homogéneo Boot 4.1.1/Java 21; única divergencia: Redis en identity.
- Node (03/07/09): base común Fastify/pg/kafkajs/zod/pino; 03-academic es el más equipado (Drizzle + OTel + eslint + supertest/testcontainers).
- Python (06): único con ML facial y MongoDB.
- Go (08): único con Gin + pgx + golang-migrate + zap.
- Gateway: sin código propio; política en plugins Kong (CORS, rate-limit con Redis).
