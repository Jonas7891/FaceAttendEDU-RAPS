---
title: "Dependencias y librerías — FaceAttend EDU"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
date: "8 de noviembre de 2026"
abstract: |
  Este documento inventaría las dependencias y librerías de FaceAttend EDU a partir de los archivos de build del repositorio ProyectoFaceAttendEDU en el tag final-delivery-2026: microservicios Java/Spring Boot, Node/TypeScript y Python/FastAPI, el gateway Kong DB-less y las aplicaciones web y móvil Expo, con versiones declaradas y notas de uso.
keywords:
  - dependencias
  - librerías
  - microservicios
  - archivos de build
  - Normas APA 7ma edición
---

# Dependencias y librerías — FaceAttend EDU

Fuente de verdad: los archivos de build del repositorio `ProyectoFaceAttendEDU` (`pom.xml`, `package.json`, `pyproject.toml`, `kong.yml`) en el tag `final-delivery-2026`, relevados el 2026-11-08. Las versiones son las declaradas. Los starters de Spring Boot heredan su versión del BOM padre (Spring, 2024), salvo cuando se indica otra. Las versiones exactas instaladas quedan fijadas en `package-lock.json` y `poetry.lock`.

## 1. Back-end

### 01-ms-identity — Java 21 / Spring Boot 4.1.2 (Maven, `identity-service:0.0.1-SNAPSHOT`)

| Dependencia | Versión | Uso |
|---|---|---|
| spring-boot-starter-webmvc | BOM 4.1.2 | API REST (`/persons`, `/users`, `/auth`, `/sessions`) |
| spring-boot-starter-data-jpa | BOM | Persistencia JPA/Hibernate (schema `identity`) |
| spring-boot-starter-data-redis | BOM | Intentos de inicio de sesión y códigos de recuperación en Redis (solo identity lo tiene) |
| spring-boot-starter-security | BOM | Filtro de autenticación y hash de contraseñas |
| spring-boot-starter-validation | BOM | Validación Bean de DTOs |
| spring-boot-starter-kafka | BOM | Eventos entre microservicios |
| spring-boot-starter-actuator | BOM | Salud y métricas |
| springdoc-openapi-starter-webmvc-ui | 3.1.0 | OpenAPI / Swagger UI |
| faceattend-auth | 1.0.0 | Guardia de sesión compartida (`back-end/shared/auth-java`) |
| minio + okhttp-jvm | 8.6.0 / 5.1.0 | Documentos del acudiente en almacenamiento S3 compatible |
| lz4-java, bcprov-jdk18on (`dependencyManagement`) | 1.11.4 / 1.86 | Versiones fijadas de dependencias transitivas (Kafka de pruebas y MinIO) para cerrar avisos de seguridad publicados (GHSA) |
| postgresql (runtime) | BOM | Driver PostgreSQL 17 |
| lombok (optional) | BOM | Reduce código repetitivo |
| starters `*-test` + h2 (test) | BOM | Pruebas con base de datos embebida |
| jacoco-maven-plugin | 0.8.13 | Cobertura; el build falla por debajo del mínimo del `pom.xml` |

Las migraciones no viven en el servicio: las aplica Liquibase desde `database/`.

### 02-ms-authorization — Java 21 / Spring Boot 4.1.2 (`authorization-service`)

Mismo conjunto que identity **sin Redis ni MinIO**: webmvc, data-jpa, security, validation, kafka, actuator, springdoc 3.1.0, faceattend-auth 1.0.0, lz4-java 1.11.4 fijada, postgresql, lombok, starters de test + h2 y JaCoCo 0.8.13. Endpoints RBAC (`/roles`, `/permissions`, `/user-roles`, `/auth/evaluate`).

### 03-ms-academic — Node 24 + TypeScript (`academic-service:0.1.0`)

| Dependencia | Versión | Uso |
|---|---|---|
| fastify | ^5.12.7 | Framework HTTP |
| pg | ^8.11.0 | Driver PostgreSQL (SQL directo, sin ORM) |
| zod | ^4.6.7 | Validación de esquemas de DTO |
| kafkajs | ^2.2.4 | Cliente Kafka |
| pino | ^9.14.0 | Logging estructurado |
| @faceattend/auth, @faceattend/infra | `file:../shared/*` | Guardia de sesión, errores HTTP centralizados, acceso a BD |
| typescript / ts-node / ts-node-dev / @types/node | ^5.4 / ^10.9 / ^2.0 / ^24.19 | Compilación y desarrollo |
| jest / ts-jest / @types/jest / @types/pg | ^29.7 / ^29.1 / ^29.5 / ^8.10 | Pruebas unitarias con umbral de cobertura en `jest.config.js` |

### 04-ms-scheduling — Java 21 / Spring Boot 4.1.2 (`scheduling-service`)

Mismo conjunto que authorization. Gestiona ambientes, bloques horarios y sesiones de clase.

### 05-ms-attendance — Java 21 / Spring Boot 4.1.2 (`attendance-service`)

Conjunto de authorization más **minio 8.6.0 + okhttp-jvm 5.1.0** (soportes de justificaciones, con bcprov-jdk18on 1.86 fijada), y **testcontainers-postgresql / testcontainers-junit-jupiter** para pruebas de integración contra PostgreSQL real. Gestiona registros de asistencia, justificaciones y reportes.

### 06-ms-biometric — Python 3.12 / FastAPI / Poetry (`biometric-service:0.1.0`)

| Dependencia | Versión | Uso |
|---|---|---|
| fastapi / starlette | ^0.162.0 / ^1.10.0 | Framework API |
| uvicorn[standard] | ^0.54.0 | Servidor ASGI |
| pydantic / pydantic-settings | ^2.6 / ^2.1 | Modelos, validación y configuración por entorno |
| motor / pymongo | ^3.3 / ^4.6 | Acceso a MongoDB (embeddings, async y sync) |
| opencv-python-headless | ^4.9.0 | Procesamiento de imagen sin libGL/X11 |
| numpy | ^1.26.0 | Álgebra de vectores y embeddings |
| face-recognition | ^1.3.0 | Cálculo del embedding facial (dlib) |
| face-recognition-models (git) | repositorio ageitgey, commit fijado en `poetry.lock` | Datos del modelo |
| Pillow | ^13.0.0 | Lectura y escritura de imágenes |
| python-multipart | ^0.0.32 | Cargas multipart |
| structlog | ^24.1.0 | Logging estructurado |
| httpx | ^0.28.1 | Cliente HTTP |
| cryptography / certifi | ^50.0.0 / >=2026.10.19 | Cifrado y certificados raíz |
| pytest / pytest-asyncio / testcontainers[mongodb] (dev) | ^9.0 / ^0.23 / ^4.0 | Pruebas async y MongoDB en contenedor |

`setuptools` ya no es dependencia: `face_recognition_models` solo necesita `pkg_resources.resource_filename`, y el servicio lo resuelve con un módulo mínimo (`pkg_resources.py`) basado en `importlib.resources`.

### 07-ms-configuration — Node 24 + TypeScript (`configuration-service:0.1.0`)

Mismo conjunto que 03-academic: fastify ^5.12.7, pg ^8.11.0, zod ^4.6.7, kafkajs ^2.2.4, pino ^9.14.0, `@faceattend/auth` y `@faceattend/infra`, y la cadena TypeScript/Jest. Única diferencia: `@types/jest` ^30.0.0.

### 08-ms-notification — Node 24 + TypeScript (`notification-service`)

Mismo conjunto que 03-academic más **nodemailer ^10.2.1** (`@types/nodemailer` ^8.0.2) para el correo saliente (SMTP). Reemplazó a la versión anterior en Go.

### 09-ms-quality — Node 24 + TypeScript (`quality-service:0.1.0`)

Mismo conjunto que 03-academic sin `@faceattend/infra`: fastify, pg, zod, kafkajs, pino, `@faceattend/auth` y la cadena TypeScript/Jest.

### 99-api-gateway — Kong 3.6 DB-less (sin dependencias de código)

Configuración declarativa en `kong/kong.yml` (`_format_version: "3.0"`). Las «librerías» son plugins de Kong:

- `cors`: los orígenes salen de la variable `CORS_ORIGINS` (marcador `__CORS_ORIGINS__`), con credenciales.
- `rate-limiting` con `policy: redis`, por IP: 60/min y 600/h en los endpoints de credenciales, 5/min y 20/h en el alta pública de sedes, y el resto usa el límite general `KONG_RATE_LIMIT_PER_MINUTE`.
- `request-size-limiting` y un `post-function` propio.

La infraestructura asociada es Kong + Redis 7.

## 2. Front-end (Expo SDK 57 + React 19.2.3 + React Native 0.86.3 + React Navigation 7)

### Web (`front-end/Web`, `FaceAttend-EDU v1.0.0`)

41 dependencias, entre ellas:

- Captura de huella: `@digitalpersona/fingerprint` ^1.0.0 y `@digitalpersona/websdk` ^1.1.0. El websdk se copia al proyecto con `scripts/copy-digitalpersona.mjs`.
- Reconocimiento facial en el navegador: `@vladmandic/face-api` ^1.7.14.
- Exportación de reportes a Excel: `exceljs` ^4.4.0.
- Idiomas: `i18next` ^26.5.10 y `react-i18next` ^17.1.8.
- Temas: `tinycolor2` ^1.6.0.
- Interfaz: `@gorhom/bottom-sheet` 5.2.14 y paquetes Expo 57 (constants, document-picker, font, haptics, image, image-picker, linear-gradient, linking, splash-screen, status-bar, symbols, system-ui, web-browser).
- Navegación: bottom-tabs ^7.4.0, elements ^2.6.3, native ^7.2.2 y native-stack ^5.0.5. Esta última es una versión antigua y obsoleta: ver la nota 3.
- Otros: gesture-handler ~2.32.0, reanimated 4.5.1, safe-area-context ~5.7.0, screens ~4.26.0, svg 15.15.4, async-storage 2.2.0, datetimepicker 9.1.0, slider 5.2.0.

Dev: typescript ~6.0.3, eslint ^9.25.0, eslint-config-expo ~57.0.2, jest ^29.7.0, jest-expo ~57.0.5, playwright-core ^1.64.1 y @types/react ~19.2.4.

### Mobile (`front-end/Mobile`, `faceattendedu v1.0.0`)

33 dependencias. Las que solo tiene Mobile:

- `axios` ^1.16.1 y `@react-navigation/stack` ^7.8.10.
- `expo-camera` ~57.0.4 (captura facial) y `expo-secure-store` ~57.0.3, que guarda el `sessionId` en el keystore/Keychain y en web cae a AsyncStorage.
- `expo-notifications` ~57.0.24, `expo-updates` ~57.0.26, `expo-device`, `expo-file-system`, `expo-sharing`, `expo-localization` y `expo-navigation-bar`.
- `i18next` ^26.0.4 y `react-i18next` ^17.0.2.
- `jwt-decode` ^4.0.0, que solo se usa si el token tiene forma de JWT; el backend emite identificadores de sesión UUID.
- datetimepicker 8.4.4.

No incluye DigitalPersona, face-api ni exceljs. Dev: typescript ~5.9.2, jest ^29.7.0, jest-expo ~57.0.5, @react-native/jest-preset ^0.86.3 y @types/react ~19.1.10.

## 3. Notas transversales

- **Java (01/02/04/05):** stack homogéneo Spring Boot 4.1.2 / Java 21. Identity y attendance suman almacenamiento de archivos (MinIO); solo identity usa Redis.
- **Node (03/07/08/09):** base común Fastify 5 / pg / kafkajs / zod 4 / pino 9 sobre Node 24, con las librerías compartidas `back-end/shared/auth-node` y `back-end/shared/node-infra`. Ningún servicio Node usa ORM.
- **Python (06):** único servicio con reconocimiento facial y MongoDB.
- **Gateway:** sin código propio. La política vive en los plugins de Kong (CORS, límites con Redis y tamaño de petición).
- **Pendientes de actualización** (insumo para el plan de mejora de la carpeta 08):
  - `@react-navigation/native-stack` ^5.0.5 (Web) está obsoleto: el resto de React Navigation va en la versión 7.
  - `kafkajs` 2.2.4 no tiene versiones nuevas desde 2023.
  - `numpy` ^1.26 y las imágenes Kong 3.6, Kafka 3.8, MongoDB 7, Redis 7 y Liquibase 4.29 tienen versiones mayores más recientes.

La separación en servicios pequeños con su propia base de datos sigue el enfoque de Newman (2021). El formato del documento sigue la American Psychological Association (2020).

# Referencias

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7.ª ed.). https://doi.org/10.1037/0000165-000

Newman, S. (2021). *Building microservices* (2.ª ed.). O'Reilly Media.

Spring. (2024). *Documentación de referencia de Spring Boot*. https://docs.spring.io/spring-boot/
