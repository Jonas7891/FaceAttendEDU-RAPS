---
title: "Variables de entorno — FaceAttend EDU"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
date: "13 de octubre de 2026"
abstract: |
  Este documento inventaría las variables de entorno de FaceAttend EDU a partir de FULL (.env.example, .env, archivos docker-compose): variables globales y por servicio, con alcance, origen y valores de ejemplo. Los secretos se enmascaran.
keywords:
  - variables de entorno
  - configuración
  - docker compose
  - Normas APA 7ma edición
---

# Variables de entorno — FaceAttend EDU

Fuente: `.env.example` (120 líneas) + `.env` (103 líneas) + `docker-compose.yml` + `docker-compose.tls.yml` + `back-end/docker-compose.yml` en `FULL` (relevado 2026-10-13). Los secretos se enmascaran como `****`. Ningún secreto real está en el repo: los archivos declaran "single source for dev credentials: nothing below is a real secret".

Nota: `.env` NO incluye `WEBHOOK_URL`, `WEBHOOK_SECRET`, `WEBHOOK_TIMEOUT_SECONDS` ni `DOMAIN` (solo están en `.env.example`).

## 1. Variables globales (definidas en `.env` / `.env.example`, interpoladas como `${VAR:-default}` en los compose)

| Variable | Alcance | Descripción / origen | Ejemplo |
|---|---|---|---|
| POSTGRES_USER | global | Usuario PG 17, 1 instancia / 8 schemas | postgres |
| POSTGRES_PASSWORD | global (secreto) | Contraseña PG de dev | **** |
| POSTGRES_DB | global | BD lógica `faceattend_db` | faceattend_db |
| POSTGRES_PORT | global | Puerto host de PG | 5432 |
| FACEATTEND_LIQUIBASE_IMAGE | global | Imagen Liquibase | liquibase/liquibase:4.29.0 |
| MONGO_USER | global | Root MongoDB 7 (vectores biométricos) | mongoadmin |
| MONGO_PASSWORD | global (secreto) | Root MongoDB | **** |
| MONGO_DB | global | BD biométrica | faceattend_biometric |
| MONGO_PORT | global | Puerto host de Mongo | 27017 |
| REDIS_PORT | global | Puerto host de Redis 7 (rate-limit Kong) | 6379 |
| KAFKA_PORT | global | Puerto host de Kafka KRaft | 9092 |
| KONG_PROXY_PORT | global | Proxy público de Kong | 8080 |
| KONG_ADMIN_PORT | global | Admin Kong (loopback) | 8001 |
| BIND_IP | global | Fija puertos no-edge a loopback | 127.0.0.1 |
| FRONTEND_PORT | global | Expo Web + nginx | 8090 |
| EXPO_PUBLIC_API_URL | global | URL del backend horneada en el build del frontend; vacío = infiere `location.hostname` en runtime | (vacío) |
| BIOMETRIC_FINGERPRINT_MATCH_THRESHOLD | global → ms-biometric | Keypoints coincidentes de huella (default 8) | 8 |
| BIOMETRIC_LIVENESS_TTL_SECONDS | global → ms-biometric | TTL del reto liveness (default 120) | 120 |
| BIOMETRIC_RATE_LIMIT_MAX_ATTEMPTS | global → ms-biometric | Rate limit (default 20) | 20 |
| BIOMETRIC_RATE_LIMIT_WINDOW_SECONDS | global → ms-biometric | Ventana del rate-limit (default 60) | 60 |
| BIOMETRIC_MAX_IMAGE_BYTES | global → ms-biometric | Tamaño máx. de imagen (default 3145728) | 3145728 |
| BIOMETRIC_MAX_IMAGE_PIXELS | global → ms-biometric | Píxeles máx. (default 16000000) | 16000000 |
| BIOMETRIC_LIVENESS_CHALLENGE_SECRET | global (secreto, requerido) | Firma retos liveness; sin fallback (`${VAR:?msg}`) | **** |
| BIOMETRIC_WS_IDLE_TIMEOUT_SECONDS | global → ms-biometric | Cierre de WS sin mensajes (default 60) | 60 |
| BIOMETRIC_WS_IDEMPOTENCY_TTL_SECONDS | global → ms-biometric | TTL de caché `client_message_id` (default 300) | 300 |
| BIOMETRIC_WS_IDEMPOTENCY_MAX_ENTRIES | global → ms-biometric | Tamaño de caché de idempotencia (default 2000) | 2000 |
| SEED_USERNAME / SEED_PASSWORD | global | Script `tools/seed-api-test-data` (la contraseña es secreto dev) | seed.admin / **** |
| BOOTSTRAP_USERNAME / BOOTSTRAP_PASSWORD | global | Admin bootstrap (la contraseña es secreto dev) | admin.faceattend / **** |
| SMTP_HOST | global → ms-notification/ms-identity | Canal de correo; vacío = deshabilitado | host.docker.internal |
| SMTP_PORT | global → ms-notification | Puerto SMTP (default del compose 587) | 1025 |
| SMTP_USERNAME | global → ms-notification | Usuario SMTP (vacío en dev Mailpit) | (vacío) |
| SMTP_PASSWORD | global (secreto) | App-password SMTP (vacío en dev) | **** |
| SMTP_FROM | global → ms-notification | Remitente | FaceAttend EDU <no-reply@faceattend.local> |
| SMTP_STARTTLS | global → ms-notification | STARTTLS (default del compose true; `.env` trae false para Mailpit) | false |
| SMTP_TIMEOUT_SECONDS | global → ms-notification | Timeout SMTP (default 10) | 10 |
| EMAIL_INTERNAL_TOKEN | global (secreto) | Secreto compartido identity→notification (`X-Internal-Token`); vacío = sin verificación | **** |
| WEBHOOK_URL | global → ms-notification | Webhook de alertas; vacío = no-op (solo `.env.example`) | (vacío) |
| WEBHOOK_SECRET | global (secreto) | Firma del webhook (solo `.env.example`) | **** |
| WEBHOOK_TIMEOUT_SECONDS | global → ms-notification | Timeout del webhook, default 5 (solo `.env.example`) | 5 |
| DOMAIN | global → caddy | Dominio real para LE; sin valor = `localhost` con CA local (solo `.env.example` + `docker-compose.tls.yml`) | (vacío) |

## 2. Variables por servicio (fijas en `environment:` de los compose, no van en `.env`)

| Variable | Servicio | Descripción | Ejemplo |
|---|---|---|---|
| MONGO_INITDB_ROOT_USERNAME/PASSWORD/DATABASE | mongodb | Mapean MONGO_USER/PASSWORD/DB al init de la imagen `mongo:7` | ver globales |
| KAFKA_NODE_ID, KAFKA_PROCESS_ROLES, KAFKA_CONTROLLER_QUORUM_VOTERS, KAFKA_LISTENERS, KAFKA_ADVERTISED_LISTENERS, KAFKA_LISTENER_SECURITY_PROTOCOL_MAP, KAFKA_CONTROLLER_LISTENER_NAMES, KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR, KAFKA_TRANSACTION_STATE_LOG_*, KAFKA_GROUP_INITIAL_REBALANCE_DELAY_MS, KAFKA_AUTO_CREATE_TOPICS_ENABLE, CLUSTER_ID | kafka | KRaft single-node (`apache/kafka:3.8.0`) | 1 / broker,controller / faceattend-kraft-cluster-id-0001 / … |
| SPRING_DATASOURCE_URL | ms-identity/authorization/scheduling/attendance | JDBC PG (`jdbc:postgresql://postgres:5432/${POSTGRES_DB}…`) | jdbc:postgresql://postgres:5432/faceattend_db |
| SPRING_DATASOURCE_USERNAME/PASSWORD | Java ×4 | Mapean POSTGRES_USER/PASSWORD | postgres / **** |
| SPRING_DATASOURCE_HIKARI_SCHEMA | Java ×4 | Schema por bounded context | identity / authorization / scheduling / attendance |
| SPRING_DATA_REDIS_HOST, FACEATTEND_RECOVERY_STORE | ms-identity | Recuperación de contraseña en Redis | redis |
| SPRING_DATA_MONGODB_URI | ms-attendance | Mongo con credenciales interpoladas (`?authSource=admin`) | mongodb://mongoadmin:****@mongodb:27017/… |
| MONGODB_URL | ms-biometric | Igual que la anterior (FastAPI+Motor) | ídem |
| SPRING_KAFKA_BOOTSTRAP_SERVERS | Java ×4 | Broker interno | kafka:29092 |
| KAFKA_BROKERS | ms-academic/configuration/biometric/notification/quality | Equivalente Node/Go/Python | kafka:29092 |
| SPRING_LIQUIBASE_ENABLED=false, SPRING_JPA_HIBERNATE_DDL_AUTO=update | Java ×4 | Liquibase externo migra; Hibernate `update` | fijos |
| FACEATTEND_AUTH_IDENTITY_URL, FACEATTEND_AUTH_AUTHORIZATION_URL | Java ×4 + ms-biometric | Guardas de sesión/RBAC | http://ms-identity:8081, http://ms-authorization:8082 |
| IDENTITY_URL, AUTHORIZATION_URL | ms-academic/configuration/biometric/notification/quality | Equivalente Node/Go/Python | ídem |
| FACEATTEND_NOTIFICATION_BASE_URL | ms-identity | identity→notification `POST /api/v1/emails` | http://ms-notification:8088 |
| FACEATTEND_NOTIFICATION_INTERNAL_TOKEN | ms-identity (secreto) | Copia de EMAIL_INTERNAL_TOKEN | **** |
| DATABASE_URL | ms-academic/configuration/notification/quality | URL PG Drizzle/Go (notification añade `?sslmode=disable`) | postgres://postgres:****@postgres:5432/faceattend_db |
| PG_SCHEMA | ms-academic/configuration/notification/quality | Schema propio | academic / configuration / notification / quality |
| PORT | ms-academic/configuration/biometric/notification/quality | Puerto interno | 8083 / 8087 / 8086 / 8088 / 8089 |
| SERVER_PORT | Java ×4 | Puerto Spring | 8081 / 8082 / 8084 / 8085 |
| NODE_ENV=production | ms-academic/configuration/quality | Modo Node | production |
| SMTP_*, EMAIL_INTERNAL_TOKEN, WEBHOOK_* | ms-notification | Passthrough directo de las globales homónimas | ver §1 |
| KONG_DATABASE=off, KONG_DECLARATIVE_CONFIG, KONG_PROXY/ADMIN_ACCESS/ERROR_LOG, KONG_ADMIN_LISTEN, KONG_DNS_ORDER | kong-gateway | Kong DB-less 3.6 | off, /kong/kong.yml, 0.0.0.0:8001, LAST,A,AAAA,CNAME |
| DOMAIN | caddy (overlay TLS) | `${DOMAIN:-localhost}` | localhost |

# Referencias

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7ma ed.). https://doi.org/10.1037/0000165-000

Docker Inc. (2024). *Referencia del archivo Compose*. https://docs.docker.com/compose/compose-file/
