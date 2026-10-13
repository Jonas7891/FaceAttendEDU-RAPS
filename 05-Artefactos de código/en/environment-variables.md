---
title: "Environment Variables — FaceAttend EDU"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
date: "October 13, 2026"
abstract: |
  This document inventories the environment variables of FaceAttend EDU from FULL (.env.example, .env, docker-compose files): global variables and per-service variables, with scope, origin and example values. Secrets are masked.
keywords:
  - environment variables
  - configuration
  - docker compose
  - APA 7th Edition
---

# Environment Variables — FaceAttend EDU

Source: `.env.example` (120 lines) + `.env` (103 lines) + `docker-compose.yml` + `docker-compose.tls.yml` + `back-end/docker-compose.yml` in `FULL` ( surveyed 2026-10-13 ). Secrets are masked as `****`. No real secret is stored in the repo: the files declare "single source for dev credentials: nothing below is a real secret".

Note: `.env` does NOT include `WEBHOOK_URL`, `WEBHOOK_SECRET`, `WEBHOOK_TIMEOUT_SECONDS` nor `DOMAIN` (only in `.env.example`).

## 1. Global variables (defined in `.env` / `.env.example`, interpolated as `${VAR:-default}` in the compose files)

| Variable | Scope | Description / origin | Example |
|---|---|---|---|
| POSTGRES_USER | global | PG 17 user, 1 instance / 8 schemas | postgres |
| POSTGRES_PASSWORD | global (secret) | Dev PG password | **** |
| POSTGRES_DB | global | Logical DB `faceattend_db` | faceattend_db |
| POSTGRES_PORT | global | Host PG port | 5432 |
| FACEATTEND_LIQUIBASE_IMAGE | global | Liquibase image | liquibase/liquibase:4.29.0 |
| MONGO_USER | global | MongoDB 7 root (biometric vectors) | mongoadmin |
| MONGO_PASSWORD | global (secret) | MongoDB root | **** |
| MONGO_DB | global | Biometric DB | faceattend_biometric |
| MONGO_PORT | global | Host Mongo port | 27017 |
| REDIS_PORT | global | Host Redis 7 port (Kong rate-limit) | 6379 |
| KAFKA_PORT | global | Host Kafka KRaft port | 9092 |
| KONG_PROXY_PORT | global | Public Kong proxy | 8080 |
| KONG_ADMIN_PORT | global | Kong admin (loopback) | 8001 |
| BIND_IP | global | Binds non-edge ports to loopback | 127.0.0.1 |
| FRONTEND_PORT | global | Expo Web + nginx | 8090 |
| EXPO_PUBLIC_API_URL | global | Backend URL baked into frontend build; empty = infer `location.hostname` at runtime | (empty) |
| BIOMETRIC_FINGERPRINT_MATCH_THRESHOLD | global → ms-biometric | Fingerprint matching keypoints (default 8) | 8 |
| BIOMETRIC_LIVENESS_TTL_SECONDS | global → ms-biometric | Liveness challenge TTL (default 120) | 120 |
| BIOMETRIC_RATE_LIMIT_MAX_ATTEMPTS | global → ms-biometric | Rate limit (default 20) | 20 |
| BIOMETRIC_RATE_LIMIT_WINDOW_SECONDS | global → ms-biometric | Rate-limit window (default 60) | 60 |
| BIOMETRIC_MAX_IMAGE_BYTES | global → ms-biometric | Max image size (default 3145728) | 3145728 |
| BIOMETRIC_MAX_IMAGE_PIXELS | global → ms-biometric | Max pixels (default 16000000) | 16000000 |
| BIOMETRIC_LIVENESS_CHALLENGE_SECRET | global (secret, required) | Signs liveness challenges; no fallback (`${VAR:?msg}`) | **** |
| BIOMETRIC_WS_IDLE_TIMEOUT_SECONDS | global → ms-biometric | WS close without messages (default 60) | 60 |
| BIOMETRIC_WS_IDEMPOTENCY_TTL_SECONDS | global → ms-biometric | `client_message_id` cache TTL (default 300) | 300 |
| BIOMETRIC_WS_IDEMPOTENCY_MAX_ENTRIES | global → ms-biometric | Idempotency cache size (default 2000) | 2000 |
| SEED_USERNAME / SEED_PASSWORD | global | `tools/seed-api-test-data` script (password is dev secret) | seed.admin / **** |
| BOOTSTRAP_USERNAME / BOOTSTRAP_PASSWORD | global | Bootstrap admin (password is dev secret) | admin.faceattend / **** |
| SMTP_HOST | global → ms-notification/ms-identity | Mail channel; empty = disabled | host.docker.internal |
| SMTP_PORT | global → ms-notification | SMTP port (compose default 587) | 1025 |
| SMTP_USERNAME | global → ms-notification | SMTP user (empty in dev Mailpit) | (empty) |
| SMTP_PASSWORD | global (secret) | SMTP app-password (empty in dev) | **** |
| SMTP_FROM | global → ms-notification | Sender | FaceAttend EDU <no-reply@faceattend.local> |
| SMTP_STARTTLS | global → ms-notification | STARTTLS (compose default true; `.env` has false for Mailpit) | false |
| SMTP_TIMEOUT_SECONDS | global → ms-notification | SMTP timeout (default 10) | 10 |
| EMAIL_INTERNAL_TOKEN | global (secret) | Shared identity→notification secret (`X-Internal-Token`); empty = no check | **** |
| WEBHOOK_URL | global → ms-notification | Alert webhook; empty = no-op (only `.env.example`) | (empty) |
| WEBHOOK_SECRET | global (secret) | Webhook signature (only `.env.example`) | **** |
| WEBHOOK_TIMEOUT_SECONDS | global → ms-notification | Webhook timeout, default 5 (only `.env.example`) | 5 |
| DOMAIN | global → caddy | Real domain for LE; unset = `localhost` with local CA (only `.env.example` + `docker-compose.tls.yml`) | (empty) |

## 2. Per-service variables (fixed in compose `environment:`, not in `.env`)

| Variable | Service | Description | Example |
|---|---|---|---|
| MONGO_INITDB_ROOT_USERNAME/PASSWORD/DATABASE | mongodb | Map MONGO_USER/PASSWORD/DB into the `mongo:7` init | see globals |
| KAFKA_NODE_ID, KAFKA_PROCESS_ROLES, KAFKA_CONTROLLER_QUORUM_VOTERS, KAFKA_LISTENERS, KAFKA_ADVERTISED_LISTENERS, KAFKA_LISTENER_SECURITY_PROTOCOL_MAP, KAFKA_CONTROLLER_LISTENER_NAMES, KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR, KAFKA_TRANSACTION_STATE_LOG_* , KAFKA_GROUP_INITIAL_REBALANCE_DELAY_MS, KAFKA_AUTO_CREATE_TOPICS_ENABLE, CLUSTER_ID | kafka | KRaft single-node (`apache/kafka:3.8.0`) | 1 / broker,controller / faceattend-kraft-cluster-id-0001 / … |
| SPRING_DATASOURCE_URL | ms-identity/authorization/scheduling/attendance | JDBC PG (`jdbc:postgresql://postgres:5432/${POSTGRES_DB}…`) | jdbc:postgresql://postgres:5432/faceattend_db |
| SPRING_DATASOURCE_USERNAME/PASSWORD | Java ×4 | Map POSTGRES_USER/PASSWORD | postgres / **** |
| SPRING_DATASOURCE_HIKARI_SCHEMA | Java ×4 | Schema per bounded context | identity / authorization / scheduling / attendance |
| SPRING_DATA_REDIS_HOST, FACEATTEND_RECOVERY_STORE | ms-identity | Password recovery in Redis | redis |
| SPRING_DATA_MONGODB_URI | ms-attendance | Mongo with interpolated credentials (`?authSource=admin`) | mongodb://mongoadmin:****@mongodb:27017/… |
| MONGODB_URL | ms-biometric | Same as above (FastAPI+Motor) | idem |
| SPRING_KAFKA_BOOTSTRAP_SERVERS | Java ×4 | Internal broker | kafka:29092 |
| KAFKA_BROKERS | ms-academic/configuration/biometric/notification/quality | Node/Go/Python equivalent | kafka:29092 |
| SPRING_LIQUIBASE_ENABLED=false, SPRING_JPA_HIBERNATE_DDL_AUTO=update | Java ×4 | External Liquibase migrates; Hibernate `update` | fixed |
| FACEATTEND_AUTH_IDENTITY_URL, FACEATTEND_AUTH_AUTHORIZATION_URL | Java ×4 + ms-biometric | Session/RBAC guards | http://ms-identity:8081, http://ms-authorization:8082 |
| IDENTITY_URL, AUTHORIZATION_URL | ms-academic/configuration/biometric/notification/quality | Node/Go/Python equivalent | idem |
| FACEATTEND_NOTIFICATION_BASE_URL | ms-identity | identity→notification `POST /api/v1/emails` | http://ms-notification:8088 |
| FACEATTEND_NOTIFICATION_INTERNAL_TOKEN | ms-identity (secret) | Copy of EMAIL_INTERNAL_TOKEN | **** |
| DATABASE_URL | ms-academic/configuration/notification/quality | PG Drizzle/Go URL (notification adds `?sslmode=disable`) | postgres://postgres:****@postgres:5432/faceattend_db |
| PG_SCHEMA | ms-academic/configuration/notification/quality | Own schema | academic / configuration / notification / quality |
| PORT | ms-academic/configuration/biometric/notification/quality | Internal port | 8083 / 8087 / 8086 / 8088 / 8089 |
| SERVER_PORT | Java ×4 | Spring port | 8081 / 8082 / 8084 / 8085 |
| NODE_ENV=production | ms-academic/configuration/quality | Node mode | production |
| SMTP_*, EMAIL_INTERNAL_TOKEN, WEBHOOK_* | ms-notification | Direct passthrough of the homonymous globals | see §1 |
| KONG_DATABASE=off, KONG_DECLARATIVE_CONFIG, KONG_PROXY/ADMIN_ACCESS/ERROR_LOG, KONG_ADMIN_LISTEN, KONG_DNS_ORDER | kong-gateway | Kong DB-less 3.6 | off, /kong/kong.yml, 0.0.0.0:8001, LAST,A,AAAA,CNAME |
| DOMAIN | caddy (TLS overlay) | `${DOMAIN:-localhost}` | localhost |

# References

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). https://doi.org/10.1037/0000165-000

Docker Inc. (2024). *Compose file reference*. https://docs.docker.com/compose/compose-file/
