# INFORME TÉCNICO

## PROYECTO: FaceAttend EDU

---

**INTEGRANTES:**
- DIEGO ANDRÉS GUTIÉRREZ NUÑEZ
- JUAN DAVID ARBOLEDA PERDOMO
- JONATTAN STEVEN RIZO SOLANO

**INSTRUCTOR:**
- MOTTA VARGAS JOSÉ DE JESÚS

**INSTITUCIÓN:**
- SERVICIO NACIONAL DE APRENDIZAJE – SENA
- ANÁLISIS Y DESARROLLO DE SOFTWARE – 3145556

**AÑO:** 2025

---

## TABLA DE CONTENIDO

1. [Resumen Ejecutivo](#1-resumen-ejecutivo)
2. [Introducción](#2-introducción)
3. [Descripción del Proyecto](#3-descripción-del-proyecto)
4. [Requerimientos](#4-requerimientos)
5. [Solución Propuesta](#5-solución-propuesta)
6. [Arquitectura del Sistema](#6-arquitectura-del-sistema)
7. [Modelo de Datos](#7-modelo-de-datos)
8. [API y Microservicios](#8-api-y-microservicios)
9. [Seguridad](#9-seguridad)
10. [Infraestructura y DevOps](#10-infraestructura-y-devops)
11. [Plan de Trabajo](#11-plan-de-trabajo)
12. [Equipo de Trabajo](#12-equipo-de-trabajo)
13. [Presupuesto](#13-presupuesto)
14. [Gestión de Calidad](#14-gestión-de-calidad)
15. [Gestión de Riesgos](#15-gestión-de-riesgos)
16. [Anexos](#16-anexos)

---

## 1. RESUMEN EJECUTIVO

### 1.1 Descripción del Proyecto

FaceAttend EDU es una plataforma integral de gestión de asistencia mediante reconocimiento biométrico dual: reconocimiento facial utilizando la cámara del dispositivo (computador o móvil) y reconocimiento dactilar mediante el SDK de DigitalPersona 4500. Diseñada específicamente para instituciones educativas, el sistema automatiza el registro de entrada y salida de estudiantes, centraliza historiales académicos, soporta flujos de justificación y genera reportes por rol con alertas automáticas.

### 1.2 Objetivo General

Desarrollar un sistema de software integral que utilice la cámara del dispositivo para reconocimiento facial y el SDK de DigitalPersona 4500 para biometría dactilar, aplicaciones móviles, web y herramientas de IA implementadas directamente en código, para validar de manera eficiente la asistencia de los alumnos y realizar un seguimiento efectivo de su participación en clases, minimizando inconsistencias en los registros, mejorando el rendimiento académico y facilitando el control por parte de los instructores.

### 1.3 Objetivos Específicos

1. Diseñar e implementar un módulo de registro de asistencia que integre tecnologías biométricas duales: reconocimiento facial mediante la cámara del dispositivo y reconocimiento dactilar mediante el SDK de DigitalPersona 4500, implementando directamente en código toda la funcionalidad necesaria para capturar datos en tiempo real, asegurando una validación precisa y automatizada en entornos presenciales, híbridos o virtuales.

2. Desarrollar una interfaz de usuario intuitiva que permita a instructores y administradores visualizar registros de asistencia, monitorear la participación en clases mediante herramientas de interacción, y recibir alertas automáticas sobre ausencias o baja participación.

3. Integrar un sistema de notificaciones automatizadas vía email o apps móviles que informe a instructores, padres y alumnos sobre anomalías en la asistencia o participación, facilitando acciones correctivas inmediatas y promoviendo la responsabilidad.

4. Asegurar la escalabilidad y compatibilidad del software con diversas tecnologías del mercado, permitiendo su adaptación a diferentes instituciones educativas y entornos de aprendizaje.

5. Implementar medidas de seguridad y privacidad en el software, incluyendo encriptación de datos y cumplimiento de normativas como la Ley 1581 de Protección de Datos Personales (Colombia), para proteger la información sensible de los alumnos y garantizar un uso ético de las tecnologías integradas.

### 1.4 Alcance

**En alcance (MVP - Horizon 1, 0-3 meses):**
- Gestión de usuarios con registro CSV y manual
- Login y autenticación con sessionId opaco
- Recuperación y cambio de contraseña
- Gestión de ambientes, cohortes, cursos y períodos académicos
- Registros de entrada/salida FACIAL + MANUAL/IMPORT
- Configuración del sistema y académica
- Flujo de justificación con documentos de soporte
- Historiales y reportes por rango de fechas/cohorte
- Reconocimiento facial con liveness detection
- Frontend responsive (Web + Mobile)

**Fuera de alcance:**
- Nómina de profesores, calificaciones, pagos
- Control de acceso físico completo (puertas/torniquetes)
- Integración con sistemas externos (enrollments/calificaciones/IdP)
- Mensajería avanzada (SMS/WhatsApp)
- Analítica predictiva de deserción
- Alertas de fallas de dispositivos IoT
- Apps móviles nativas separadas (el MVP es responsive web + mobile web)

### 1.5 Personal Involucrado

| Nombre | Rol | Responsabilidad |
|--------|-----|-----------------|
| Diego Andrés Gutiérrez Nuñez | Administrador, Desarrollador, IA | Análisis de información, diseño y programación |
| Juan David Arboleda Perdomo | Analista, Base de Datos | Análisis de información, diseño y programación |
| Jonattan Steven Rizo Solano | Hardware, Desarrollador | Análisis de información, diseño y programación |

### 1.6 Resumen

El proyecto FaceAttend EDU representa una solución tecnológica completa que transforma la gestión de asistencia en instituciones educativas mediante el uso de reconocimiento facial, arquitectura de microservicios polyglot y un frontend unificado con React Native/Expo. El sistema está diseñado con estándares de calidad profesional, incluyendo documentación IEEE 829, ISO 25010, ISO 29110 e ISTQB.

---

## 2. INTRODUCCIÓN

### 2.1 Planteamiento del Problema

En el entorno educativo actual, el registro de asistencia manual presenta múltiples problemáticas:

- **Inconsistencias en los registros:** Los métodos tradicionales (listas en papel, llamados) son propensos a errores humanos, olvidos y manipulación.
- **Sobrecarga administrativa:** Los instructores dedican tiempo valioso de clase a tareas administrativas en lugar de enfocarse en la enseñanza.
- **Riesgo de suplantación:** Sin verificación de identidad automatizada, es fácil que un estudiante registre asistencia por otro.
- **Falta de trazabilidad:** No existe un sistema centralizado que permita consultar históricos de asistencia de manera eficiente.
- **Dificultad en la generación de reportes:** La consolidación manual de datos para reportes es lenta y propensa a errores.

### 2.2 Propósito

FaceAttend EDU surge como respuesta a estas problemáticas, proporcionando una plataforma que:

- Automatiza el registro de asistencia mediante reconocimiento facial y biométrico
- Garantiza la identidad del estudiante mediante verificación biométrica con prueba de vida (liveness detection)
- Centraliza toda la información de asistencia en una base de datos estructurada
- Genera reportes automáticos y alertas en tiempo real
- Reduce la carga administrativa de los instructores
- Cumple con normativas de protección de datos (Ley 1581)

### 2.3 Justificación

La implementación de FaceAttend EDU se justifica por:

1. **Eficiencia operativa:** Reducción del tiempo de registro de asistencia de minutos a segundos por estudiante.
2. **Precisión y seguridad:** Eliminación de errores humanos y suplantación de identidad mediante biometría.
3. **Trazabilidad completa:** Historiales completos y auditables de asistencia.
4. **Mejora académica:** Identificación temprana de patrones de ausentismo que afectan el rendimiento.
5. **Escalabilidad:** Arquitectura de microservicios que permite crecer con la institución.
6. **Cumplimiento normativo:** Alineación con la Ley 1581 de Protección de Datos Personales.

---

## 3. DESCRIPCIÓN DEL PROYECTO

### 3.1 Descripción General

FaceAttend EDU es una aplicación web y móvil diseñada para instituciones educativas que buscan optimizar el proceso de registro de asistencia mediante tecnología biométrica dual: reconocimiento facial utilizando la cámara del dispositivo y reconocimiento dactilar mediante el SDK de DigitalPersona 4500. Este sistema automatiza la verificación de presencia de los estudiantes y permite generar reportes precisos, mejorando la gestión académica y reduciendo la carga administrativa.

La aplicación se integra como una herramienta de apoyo a la gestión educativa, con funcionalidades adaptables para distintos niveles académicos y con enfoque en usabilidad, seguridad y eficiencia. Es una solución autónoma, aunque puede ser complementada con otros sistemas institucionales de información académica.

### 3.2 Perspectiva del Producto

FaceAttend EDU es un sistema compuesto por:

- **Frontend Web y Mobile:** Aplicación responsive construida con React Native y Expo, compatible con navegadores modernos y dispositivos móviles iOS/Android.
- **Backend:** 9 microservicios especializados en diferentes dominios del negocio.
- **API Gateway:** Kong OSS como punto de entrada único para todas las peticiones.
- **Bases de Datos:** PostgreSQL 17 (datos relacionales) y MongoDB 7 (datos biométricos).
- **Mensajería:** Apache Kafka para comunicación asíncrona entre servicios.
- **Infraestructura:** Docker Compose para orquestación de contenedores.

### 3.3 Características de los Usuarios

| Tipo de Usuario | Descripción | Necesidades Principales |
|-----------------|-------------|------------------------|
| **Aprendiz/Estudiante** | Usuario principal que registra asistencia | Registro rápido de asistencia, consulta de historial, justificación de ausencias |
| **Instructor/Docente** | Responsable de gestionar la asistencia en sus clases | Captura automática de asistencia, visualización de reportes, gestión de justificaciones |
| **Administrador** | Gestiona la configuración del sistema | Gestión de usuarios, configuración académica, generación de reportes globales |
| **Supervisor** | Monitorea el cumplimiento académico | Alertas de ausentismo, reportes consolidados, métricas de participación |

### 3.4 Funcionalidades Principales

1. **Registro de Asistencia Facial:** Captura de rostro con la cámara del dispositivo (computador o móvil), detección de vida (liveness), y registro automático con fecha/hora.
2. **Registro de Asistencia Dactilar:** Captura de huella mediante DigitalPersona 4500 SDK, procesamiento en código y registro automático con fecha/hora.
3. **Registro Manual/Import:** Método de respaldo para casos especiales o fallos técnicos.
4. **Gestión de Usuarios:** CRUD completo de aprendices, instructores y administradores.
5. **Gestión Académica:** Administración de sedes, programas, cohortes, cursos y períodos.
6. **Justificaciones:** Flujo completo de solicitud, revisión y aprobación/rechazo de justificaciones.
7. **Reportes y Dashboard:** Visualización de métricas, exportación a Excel, filtros por fecha/cohorte.
8. **Notificaciones y Alertas:** Sistema de alertas automáticas por ausencias o anomalías.
9. **Configuración:** Parámetros académicos, de seguridad y biométricos.

---

## 4. REQUERIMIENTOS

### 4.1 Requerimientos de Software

#### 4.1.1 Sistemas Operativos

**Servidor:**
- **Linux:** Ubuntu Server 22.04 LTS, CentOS 8 o Debian 12 (recomendado para producción)
- **Windows:** Windows Server 2019/2022 (alternativa)
- **Contenedores:** Docker 24+ y Docker Compose 2+

**Cliente (Navegador Web):**
- Chrome 90+
- Firefox 88+
- Edge 90+
- Safari 14+
- Brave / Opera (cualquier versión reciente)

**Móvil:**
- Android 8.0 (API 26) o superior
- iOS 14.0 o superior

#### 4.1.2 Tecnologías de Backend

| Tecnología | Versión | Uso |
|------------|---------|-----|
| Java | 21 LTS | 4 microservicios principales |
| Spring Boot | 4.1.1 | Framework principal Java |
| Spring Security | 6.x | Autenticación y autorización |
| Spring Data JPA | 4.x | Persistencia Java |
| TypeScript | 5.x | 3 microservicios + Frontend Web |
| Fastify | 4.x | Framework HTTP TypeScript |
| Drizzle ORM | 0.31.0 | ORM TypeScript |
| Python | 3.12 | Microservicio biométrico |
| FastAPI | 0.110 | Framework Python |
| Go | 1.22 | Microservicio notificaciones |
| Gin | 1.9.1 | Framework HTTP Go |

#### 4.1.3 Tecnologías de Frontend

| Tecnología | Versión | Uso |
|------------|---------|-----|
| Expo SDK | 57 | Framework React Native |
| React | 19.2.3 | Librería UI |
| React Native | 0.86.3 | Framework móvil |
| React Native Web | 0.21 | Soporte web |
| React Navigation | 7.x | Navegación |
| @digitalpersona/fingerprint | 1.0.0 | Lector de huellas DigitalPersona |
| @digitalpersona/websdk | 1.1.0 | SDK DigitalPersona Web |
| axios | 1.16.1 | Cliente HTTP |
| i18next | 26.0.4 | Internacionalización |
| xlsx | 0.18.5 | Exportación Excel |

#### 4.1.4 Bases de Datos

| Motor | Versión | Uso |
|-------|---------|-----|
| PostgreSQL | 17 | Base de datos principal (8 schemas) |
| MongoDB | 7 | Almacenamiento de vectores biométricos |
| Redis | 7 | Caché y rate-limiting |

#### 4.1.5 Infraestructura y Herramientas

| Tecnología | Versión | Uso |
|------------|---------|-----|
| Docker | 24+ | Contenedores |
| Docker Compose | 2+ | Orquestación |
| Kong | 3.6 | API Gateway |
| Apache Kafka | 3.8 (KRaft) | Mensajería de eventos |
| Liquibase | 4.29.0 | Migraciones de base de datos |
| OpenTelemetry | 1.24 | Observabilidad |
| Jenkins | 2.x | CI/CD |

#### 4.1.6 Tecnología Biométrica

| Componente | Tecnología | Descripción |
|------------|------------|-------------|
| Reconocimiento Facial | Cámara del dispositivo + face-api.js | Detección y reconocimiento facial utilizando la cámara del computador o móvil |
| SDK Dactilar | DigitalPersona 4500 SDK | Captura y procesamiento de huellas dactilares |
| Almacenamiento | MongoDB 7 | Embeddings faciales y templates dactilares con índice |
| Matching Facial | Similitud coseno | Comparación de vectores 128-d |
| Matching Dactilar | DigitalPersona SDK | Comparación de templates dactilares |

**Nota:** El sistema utiliza dos métodos biométricos complementarios: reconocimiento facial mediante la cámara del dispositivo (computador o móvil) y reconocimiento dactilar mediante el SDK de DigitalPersona 4500. Toda la funcionalidad se implementará directamente en código.

#### 4.1.7 Seguridad y Validación

| Mecanismo | Detalles |
|-----------|---------|
| Autenticación | SessionId opaco (UUID) - nunca JWT |
| Password Hashing | BCrypt con cost factor 12 |
| Autorización | RBAC (Role-Based Access Control) |
| Cifrado | SSL/TLS para comunicaciones |
| Rate Limiting | Kong (5173, 8090, 3000 orígenes) |
| Validación biométrica | Liveness detection (facial) + Captura dactilar (DigitalPersona 4500) |
| Protección de datos | Cumplimiento Ley 1581 |

### 4.2 Requerimientos de Hardware

#### 4.2.1 Servidor

| Componente | Mínimo | Recomendado |
|------------|--------|-------------|
| **Procesador** | Intel i5 (8ª gen) o equivalente AMD | Intel i7 (12ª gen) o AMD Ryzen 7 |
| **Memoria RAM** | 8 GB | 16 GB |
| **Almacenamiento** | SSD 256 GB | SSD 512 GB |
| **GPU** | No requerida | No requerida |
| **Red** | Ethernet 100 Mbps | Ethernet 1 Gbps |

#### 4.2.2 Dispositivos Biométricos

| Tipo | Especificación | Uso |
|------|----------------|-----|
| Cámara del dispositivo | Webcam 720p+ (computador) o cámara frontal (móvil) | Reconocimiento facial |
| DigitalPersona 4500 | Lector de huellas dactilares USB con SDK | Reconocimiento dactilar |

**Nota:** El sistema utiliza la cámara del dispositivo (computador o móvil) para reconocimiento facial y el DigitalPersona 4500 para reconocimiento dactilar. Ambos métodos biométricos trabajan de forma complementaria.

#### 4.2.3 Dispositivos Móviles

| Plataforma | Requisitos |
|------------|------------|
| **Android** | Android 8.0+, cámara frontal 720p+, 2 GB RAM |
| **iOS** | iOS 14.0+, cámara frontal 720p+, 2 GB RAM |

#### 4.2.4 IoT y Sensores (Futuro - Horizon 3)

| Dispositivo | Función |
|-------------|---------|
| Sensores de Movimiento | Verificar presencia física en el aula |
| Sensores NFC/Proximidad | Confirmar ubicación del estudiante |
| Raspberry Pi | Procesamiento edge para IoT |

---

## 5. SOLUCIÓN PROPUESTA

### 5.1 Visión General de la Solución

La solución propuesta es un sistema de software integral basado en una arquitectura de microservicios polyglot que utiliza tecnologías disponibles en el mercado para validar de manera eficiente la asistencia de los alumnos y realizar un seguimiento efectivo de su participación en clases.

### 5.2 Pilares de la Solución

1. **Captura Automatizada de Asistencia:** Registro biométrico dual: facial (cámara del dispositivo) y dactilar (DigitalPersona 4500 SDK).
2. **Seguimiento Académico Confiable:** Historiales completos, reportes automáticos y alertas.
3. **Flujo de Justificación Transparente:** Proceso completo de solicitud, revisión y resolución.
4. **Seguridad, Privacidad y Usabilidad:** Cumplimiento normativo, autenticación robusta y UX intuitiva.
5. **Operación Escalable y Compatible:** Arquitectura de microservicios, contenedores Docker y API Gateway.

### 5.3 Roadmap del Producto

| Horizonte | Periodo | Entregables |
|-----------|---------|-------------|
| **Horizon 1 (MVP)** | 0-3 meses | Identity, Academic, Scheduling, Attendance core, Biometric facial enroll (cámara) + dactilar enroll (DigitalPersona 4500), Justification workflow, Notification in-app |
| **Horizon 2 (Trazabilidad)** | 3-6 meses | Justificaciones end-to-end, dashboards, alertas, configuración por school |
| **Horizon 3 (Escala)** | 6-12 meses | Analítica avanzada, multi-school tenancy, optimización SDK |

### 5.4 Principios de Diseño

- **Precisión sobre velocidad:** La exactitud del reconocimiento es prioritaria.
- **Privacy by design:** Los datos biométricos se tratan como PII sensibles.
- **Offline-first (futuro):** Capacidad de captura sin conexión con sincronización posterior.
- **API-First:** Contratos OpenAPI como fuente de verdad.
- **Observability by Design:** Logs, métricas y tracing desde el día uno.

---

## 6. ARQUITECTURA DEL SISTEMA

### 6.1 Estilo Arquitectónico

El sistema implementa una arquitectura de **Microservicios Polyglot** con **Arquitectura Hexagonal** (Puertos y Adaptadores) y **Domain-Driven Design** (Bounded Contexts).

### 6.2 Diagrama de Contenedores (C4)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         CLIENTES                                        │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐      │
│  │   Web Browser    │  │  Mobile App      │  │  Tablet          │      │
│  │   (React Native  │  │  (Expo/React     │  │  (Expo/React     │      │
│  │    Web)          │  │   Native)        │  │   Native)        │      │
│  └────────┬─────────┘  └────────┬─────────┘  └────────┬─────────┘      │
└───────────┼─────────────────────┼─────────────────────┼────────────────┘
            │                     │                     │
            └─────────────────────┼─────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      API GATEWAY                                        │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │   Kong OSS 3.6 (DB-less)                                         │   │
│  │   Puerto: 8080 (proxy) / 8001 (admin)                           │   │
│  │   Plugins: CORS, Rate Limiting                                  │   │
│  └──────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                    MICROSERVICIOS                                       │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐       │
│  │ 01-Identity │ │02-Authoriz. │ │03-Academic  │ │04-Schedul.  │       │
│  │ Java 8081   │ │ Java 8082   │ │ TS 8083     │ │ Java 8084  │       │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘       │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐       │
│  │05-Attendan. │ │06-Biometric │ │07-Config.   │ │08-Notif.    │       │
│  │ Java 8085   │ │ Python 8086 │ │ TS 8087     │ │ Go 8088     │       │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘       │
│  ┌─────────────┐                                                      │
│  │ 09-Quality  │                                                      │
│  │ TS 8089     │                                                      │
│  └─────────────┘                                                      │
└─────────────────────────────────────────────────────────────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                      DATOS                                              │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐         │
│  │ PostgreSQL 17   │  │ MongoDB 7       │  │ Redis 7         │         │
│  │ 8 schemas       │  │ Biometric       │  │ Kong cache      │         │
│  │ faceattend_db   │  │ embeddings      │  │                 │         │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘         │
│  ┌─────────────────┐                                                    │
│  │ Kafka 3.8       │                                                    │
│  │ Domain Events   │                                                    │
│  └─────────────────┘                                                    │
└─────────────────────────────────────────────────────────────────────────┘
```

### 6.3 Catálogo de Microservicios

| # | Servicio | Puerto | Lenguaje | Framework | Base de Datos | Responsabilidad |
|---|----------|--------|----------|-----------|---------------|-----------------|
| 01 | ms-identity | 8081 | Java 21 | Spring Boot 4.1 | PostgreSQL | Personas, usuarios, sesiones, políticas de contraseña |
| 02 | ms-authorization | 8082 | Java 21 | Spring Boot 4.1 | PostgreSQL | RBAC (roles, permisos, asignaciones) |
| 03 | ms-academic | 8083 | TypeScript | Fastify + Drizzle | PostgreSQL | Sedes, programas, cohortes, cursos, matrículas |
| 04 | ms-scheduling | 8084 | Java 21 | Spring Boot 4.1 | PostgreSQL | Ambientes, bloques horarios, sesiones de clase |
| 05 | ms-attendance | 8085 | Java 21 | Spring Boot 4.1 | PostgreSQL | Registro de asistencia, justificaciones, documentos |
| 06 | ms-biometric | 8086 | Python 3.12 | FastAPI | MongoDB | Embeddings faciales + templates dactilares, matching |
| 07 | ms-configuration | 8087 | TypeScript | Fastify | PostgreSQL | Configuración académica/seguridad, casos biométricos |
| 08 | ms-notification | 8088 | Go 1.22 | Gin | PostgreSQL | Alertas y notificaciones |
| 09 | ms-quality | 8089 | TypeScript | Fastify | PostgreSQL | Evaluación calidad ISO 25010/29110, ISTQB |
| 99 | api-gateway | 8080/8001 | - | Kong OSS 3.6 | Redis | Routing, auth, rate-limit, CORS |

### 6.4 Arquitectura Hexagonal

Cada microservicio sigue el patrón de Puertos y Adaptadores:

```
┌─────────────────────────────────────────────────────────┐
│                   INFRASTRUCTURE                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │ HTTP        │  │ Repository  │  │ Event       │     │
│  │ Controllers │  │ Adapters    │  │ Publishers  │     │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘     │
│         │                │                │             │
│         └────────────────┼────────────────┘             │
│                          │                              │
│                          ▼                              │
│  ┌─────────────────────────────────────────────────┐   │
│  │                  APPLICATION                     │   │
│  │  ┌─────────────┐  ┌─────────────┐               │   │
│  │  │ Use Cases   │  │ DTOs        │               │   │
│  │  └─────────────┘  └─────────────┘               │   │
│  └────────────────────┬────────────────────────────┘   │
│                       │                                 │
│                       ▼                                 │
│  ┌─────────────────────────────────────────────────┐   │
│  │                    DOMAIN                        │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────┐  │   │
│  │  │ Aggregates  │  │ Value       │  │ Domain  │  │   │
│  │  │             │  │ Objects     │  │ Events  │  │   │
│  │  └─────────────┘  └─────────────┘  └─────────┘  │   │
│  │  ┌─────────────┐  ┌─────────────┐               │   │
│  │  │ Ports (In)  │  │ Ports (Out) │               │   │
│  │  └─────────────┘  └─────────────┘               │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

**Regla de dependencia:** `infrastructure → application → domain` (siempre hacia adentro).

### 6.5 Patrones Arquitectónicos Adoptados

| Patrón | Estado | Descripción |
|--------|--------|-------------|
| API Gateway (Kong OSS) | Implementado | Punto de entrada único con CORS y rate-limiting |
| Database per Service (schema-per-context) | Implementado | Cada bounded context tiene su propio schema en PostgreSQL |
| Hexagonal Architecture | Implementado | Puertos y adaptadores en todos los servicios |
| Message Broker (Kafka) | Implementado | Comunicación asíncrona entre servicios |
| Circuit Breaker | Parcial | Definido pero no completamente implementado |
| Saga (choreographed) | Parcial | Para operaciones distribuidas |
| Outbox Pattern | Parcial | Para publicación confiable de eventos |
| Retry + Exponential Backoff + DLQ | Implementado | Para resiliencia en comunicación |
| CQRS | No implementado | Futuro |
| Event Sourcing | No implementado | Futuro |

### 6.6 Redes Docker

| Red | Miembros | Propósito |
|-----|----------|-----------|
| `faceattend-edge` | frontend-web, kong-gateway | Punto de contacto exterior |
| `faceattend-app` | kong-gateway, 9 ms-*, redis | Comunicación entre servicios |
| `faceattend-data` | 9 ms-*, postgres, mongodb, kafka, liquibase | Acceso a datos |

---

## 7. MODELO DE DATOS

### 7.1 Resumen

- **29 tablas SQL** en PostgreSQL 17 (8 schemas)
- **2 colecciones MongoDB** para datos biométricos (facial + dactilar)
- **28 entidades** + **4 Value Objects**
- **Normalización:** 3NF en el núcleo con 0 desnormalizaciones intencionales

### 7.2 Esquemas de PostgreSQL

| Schema | Tablas | Descripción |
|--------|--------|-------------|
| **identity** | person, app_user, user_session, password_policy | Identidad y credenciales |
| **authorization** | role, permission, role_permission, user_role | RBAC |
| **academic** | school, program, academic_period, cohort, course, academic_actor_type, academic_actor, enrollment | Estructura académica |
| **scheduling** | environment, schedule_block, class_session | Horarios y sesiones |
| **attendance** | attendance_record, justification_type, justification, supporting_document, attendance_report | Asistencia |
| **configuration** | academic_configuration, security_configuration, biometric_update_case | Configuración |
| **notification** | alert_type, alert | Alertas |
| **biometric** | (vacío - usa MongoDB) | Solo schema |
| **quality** | quality_project, quality_evaluation, quality_evaluation_item, process_assessment, process_assessment_rating, istqb_assessment, istqb_assessment_item | Calidad |

### 7.3 Colecciones MongoDB

| Colección | Campos Clave |
|-----------|--------------|
| **facial_embedding** | person_id, template_version, encoding (128-d), model_version, enrolled_at, is_active |
| **fingerprint_embedding** | person_id, finger_number, template_version, encoding, model_version, enrolled_at, is_active |

### 7.4 Entidades Principales por Contexto

#### Identity
- **Person:** UUID, document_type, document_number (único), primer_nombre, segundo_nombre, primer_apellido, segundo_apellido, email, phone, birth_date, status
- **App_User:** UUID, username, password_hash, status, last_login_at, failed_attempts, locked_until
- **User_Session:** UUID, session_id (opaco), user_id, status, start_date, end_date, source_ip, user_agent
- **Password_Policy:** UUID, min_length, require_uppercase, require_lowercase, require_digit, require_special, max_age_days, history_count

#### Authorization
- **Role:** UUID, name, description, status
- **Permission:** UUID, name (formato: context.resource:action), description
- **Role_Permission:** role_id, permission_id (compuesta PK)
- **User_Role:** user_id, role_id (compuesta PK)

#### Academic
- **School:** UUID, name, code, address, city_id, status
- **Program:** UUID, school_id, name, code, duration_hours, status
- **Academic_Period:** UUID, school_id, name, start_date, end_date, status
- **Cohort:** UUID, program_id, instructor_id, name, code, start_date, end_date, status
- **Course:** UUID, school_id, name, code, credits, status
- **Academic_Actor_Type:** UUID, name (INSTRUCTOR, LEARNER, etc.)
- **Academic_Actor:** UUID, person_id, actor_type_id, status
- **Enrollment:** UUID, cohort_id, learner_id, status, enrollment_date

#### Scheduling
- **Environment:** UUID, school_id, name, capacity, status
- **Schedule_Block:** UUID, environment_id, day_of_week, start_time, end_time
- **Class_Session:** UUID, cohort_id, course_id, environment_id, session_date, status (Open/Closed/Cancelled)

#### Attendance
- **Attendance_Record:** UUID, class_session_id, learner_id, status (Present/Absent/Late/Justified), recording_method (FACIAL/MANUAL/IOT/IMPORT), recorded_at, confidence_score
- **Justification_Type:** UUID, name, description, max_days, requires_document
- **Justification:** UUID, attendance_record_id, justification_type_id, reason, status (Pending/Approved/Rejected), reviewed_by, reviewed_at
- **Supporting_Document:** UUID, justification_id, file_name, file_path, file_size, mime_type, uploaded_at
- **Attendance_Report:** UUID, cohort_id, generated_at, filters (JSONB), result (JSONB)

#### Biometric
- **Facial_Embedding (MongoDB):** person_id, template_version, encoding (128-d array), model_version, enrolled_at, is_active
- **Fingerprint_Embedding (MongoDB):** person_id, finger_number, template_version, encoding, model_version, enrolled_at, is_active
- **Biometric_Update_Case (SQL):** UUID, person_id, update_type, status (Pending/In_Review/Approved/Rejected), requested_by, reviewed_by

#### Notification
- **Alert_Type:** UUID, name, severity (INFO/WARNING/CRITICAL), channel (DASHBOARD/EMAIL/PUSH)
- **Alert:** UUID, alert_type_id, user_id, title, message, status, created_at, resolved_at

#### Configuration
- **Academic_Configuration:** UUID, school_id, attendance_threshold, late_tolerance_minutes, justification_window_days
- **Security_Configuration:** UUID, session_timeout_minutes, max_failed_attempts, lockout_duration_minutes, password_policy_id

### 7.5 Reglas de Diseño

- **Sin FK entre contextos** (solo comentarios, referencias por UUID)
- **UUID** para entidades cross-context
- **Soft delete** con `deleted_at`
- **Auditoría:** created_at, updated_at, deleted_at, created_by, updated_by, deleted_by, row_version
- **ENUMs** nativos de PostgreSQL
- **Nomenclatura:** snake_case, tablas en singular

### 7.6 Reglas de Negocio (Resumen)

| ID | Regla |
|----|-------|
| RN-01 | App_User debe tener asociado un Person |
| RN-07 | Todo App_User debe tener al menos un rol |
| RN-14 | Cohort debe tener instructor válido antes de registrar asistencia |
| RN-18 | Schedule_Block no puede solaparse en el mismo ambiente |
| RN-22 | Un attendance_record por actor por sesión |
| RN-23 | Asistencia solo se registra en sesión con estado Open |
| RN-28 | Un solo facial embedding y un solo template dactilar activo por persona y dedo |
| RN-32 | Justificación es 1:1 con attendance_record |
| RN-36 | Justificación aprobada → attendance_status = Justified |

---

## 8. API Y MICROSERVICIOS

### 8.1 API Gateway (Kong)

| Servicio | Rutas | Descripción |
|----------|-------|-------------|
| identity-service | `/api/v1/persons`, `/api/v1/users`, `/api/v1/auth`, `/api/v1/sessions`, `/api/v1/password-policies` | Identidad y autenticación |
| authorization-service | `/api/v1/roles`, `/api/v1/permissions`, `/api/v1/user-roles` | RBAC |
| academic-service | `/api/v1/schools`, `/api/v1/programs`, `/api/v1/academic-periods`, `/api/v1/cohorts`, `/api/v1/courses`, `/api/v1/actor-types`, `/api/v1/academic-actors`, `/api/v1/enrollments` | Estructura académica |
| scheduling-service | `/api/v1/environments`, `/api/v1/schedule-blocks`, `/api/v1/class-sessions` | Horarios |
| attendance-service | `/api/v1/attendance-records`, `/api/v1/attendance-reports`, `/api/v1/justifications`, `/api/v1/justification-types`, `/api/v1/supporting-documents` | Asistencia |
| biometric-service | `/api/v1/biometric` | Biometría dual: facial (cámara) + dactilar (DigitalPersona 4500) |
| configuration-service | `/api/v1/academic-configurations`, `/api/v1/security-configurations`, `/api/v1/biometric-update-cases` | Configuración |
| notification-service | `/api/v1/alert-types`, `/api/v1/alerts` | Alertas |
| quality-service | `/api/v1/quality` | Calidad |

### 8.2 Endpoints Biométricos Detallados

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| POST | `/api/v1/biometric/facial/enroll` | Registrar rostro (cámara del dispositivo) |
| POST | `/api/v1/biometric/facial/verify` | Verificar identidad facial (1:1) |
| POST | `/api/v1/biometric/facial/identify` | Identificar persona facial (1:N) |
| GET | `/api/v1/biometric/facial/{person_id}` | Obtener template facial activo |
| GET | `/api/v1/biometric/facial/{person_id}/history` | Historial de templates faciales |
| DELETE | `/api/v1/biometric/facial/{person_id}` | Eliminar template facial |
| POST | `/api/v1/biometric/fingerprint/enroll` | Registrar huella dactilar (DigitalPersona 4500) |
| POST | `/api/v1/biometric/fingerprint/verify` | Verificar identidad dactilar (1:1) |
| POST | `/api/v1/biometric/fingerprint/identify` | Identificar persona dactilar (1:N) |
| GET | `/api/v1/biometric/fingerprint/{person_id}` | Obtener template dactilar activo |
| GET | `/api/v1/biometric/fingerprint/{person_id}/history` | Historial de templates dactilares |
| DELETE | `/api/v1/biometric/fingerprint/{person_id}` | Eliminar template dactilar |
| WS | `/api/v1/biometric/ws` | Canal WebSocket |

### 8.3 Autenticación

**Mecanismo:** `Authorization: Bearer <sessionId>` donde sessionId es un UUID opaco (nunca JWT).

**Flujo de Login:**
1. `POST /api/v1/auth/login` con `{identifier (email|username), password}`
2. El servidor valida credenciales con BCrypt
3. Se crea un `user_session` con sessionId UUID
4. Respuesta: `{sessionId, userId, sessionStatus, startDate, endDate, sourceIp}`
5. Fallos siempre retornan 401 "Invalid email or password" (sin enumeración de usuarios)

**Validación:**
- Servicios Java (01/02/04/05): AuthTokenFilter valida el sessionId
- TS/Python/Go services (03/06/07/08/09): Pendiente implementar (deuda técnica AT-004)
- Kong NO valida el token (solo CORS + rate-limiting)

### 8.4 Eventos de Dominio

| Contexto | Eventos |
|----------|---------|
| Identity | UserRegistered, UserActivated, UserDeactivated, SessionStarted, PasswordChanged |
| Authorization | RoleAssigned, RoleRemoved, PermissionGranted, PermissionRevoked |
| Academic | SchoolRegistered, ProgramCreated, CohortRegistered, EnrollmentCreated, CourseAdded, CourseDeleted |
| Scheduling | EnvironmentRegistered, ScheduleBlockCreated, ScheduleBlockConflictDetected, ClassSessionOpened, ClassSessionClosed |
| Biometric | FacialEmbeddingEnrolled, FacialEmbeddingUpdated, FingerprintEmbeddingEnrolled, FingerprintEmbeddingUpdated, BiometricUpdateRequested, BiometricUpdateApproved, BiometricUpdateRejected |
| Attendance | AttendanceRecorded, LateArrivalDetected, AbsenceDetected, JustificationRequested, SupportingDocumentUploaded, JustificationApproved, JustificationRejected |
| Notification | AlertRaised, AlertResolved |

**Garantías de entrega:** At-least-once con handlers idempotentes, Dead Letter Queue (DLQ) con 3-5 retries, backoff exponencial, retención 7 días.

### 8.5 Contratos OpenAPI

Todos los servicios cuentan con especificaciones OpenAPI 3.0.3 en `07-api/contracts/openapi/`:
- `01-identity-service.yaml`
- `02-authorization-service.yaml`
- `03-academic-service.yaml`
- `04-scheduling-service.yaml`
- `05-attendance-service.yaml`
- `06-biometric-service.yaml`
- `07-configuration-service.yaml`
- `08-notification-service.yaml`
- `09-quality-service.yaml`
- `api-gateway.yaml`

---

## 9. SEGURIDAD

### 9.1 Autenticación y Autorización

| Mecanismo | Detalles |
|-----------|---------|
| Autenticación | SessionId opaco (UUID) - nunca JWT |
| Password Hashing | BCrypt con cost factor 12, salt automático 16-byte |
| Autorización | RBAC con roles: Administrador, Instructor, Aprendiz |
| Evaluación de permisos | `GET /api/v1/auth/evaluate?userId&permission` → `{allowed}` |
| Cifrado | SSL/TLS para todas las comunicaciones |
| Rate Limiting | Kong con Redis (orígenes: localhost:3000, 5173, 8090) |

### 9.2 Protección de Datos

- **Datos biométricos como PII:** Los embeddings faciales y templates dactilares se tratan como información personal sensible.
- **Ley 1581:** Cumplimiento de la normativa colombiana de protección de datos personales.
- **Consentimiento:** Se requiere consentimiento explícito para el registro biométrico.
- **Cifrado en reposo:** Los datos sensibles se cifran en la base de datos.
- **Cifrado en tránsito:** Todas las comunicaciones usan HTTPS/TLS.

### 9.3 Validación Biométrica

- **Liveness Detection (facial):** Verificación de vida mediante detección de parpadeo y movimiento.
- **Captura dactilar:** Mediante DigitalPersona 4500 SDK con validación de calidad de huella.
- **Rate Limiting biométrico:** Máximo 20 intentos por ventana de 60 segundos.
- **Almacenamiento seguro:** Embeddings faciales y templates dactilares cifrados en MongoDB.

### 9.4 Deuda Técnica de Seguridad

| ID | Descripción | Prioridad |
|----|-------------|-----------|
| AT-004 | Bearer no validado en TS/Python/Go services (03/06/07/08/09) | P0 |
| AT-005 | Endpoints de recuperación de contraseña no implementados | P1 |
| AT-007 | Kong no tiene plugin JWT | P1 |

---

## 10. INFRAESTRUCTURA Y DEVOPS

### 10.1 Entornos

| Entorno | Descripción |
|---------|-------------|
| **Desarrollo** | Docker Compose local con todas las dependencias |
| **Staging** | Réplica de producción con datos de prueba |
| **Producción** | Despliegue con monitoreo y alertas |

### 10.2 Puertos

| Puerto | Servicio | Bind |
|--------|----------|------|
| 8080 | Kong Gateway (proxy) | 0.0.0.0 |
| 8090 | Frontend Web | 0.0.0.0 |
| 8001 | Kong Admin | 127.0.0.1 |
| 5432 | PostgreSQL | 127.0.0.1 |
| 27017 | MongoDB | 127.0.0.1 |
| 6379 | Redis | 127.0.0.1 |
| 9092 | Kafka | 127.0.0.1 |
| 8081-8089 | Microservicios | 127.0.0.1 |

### 10.3 Variables de Entorno Principales

```env
# PostgreSQL
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=faceattend_db
POSTGRES_PORT=5432

# MongoDB
MONGO_USER=mongoadmin
MONGO_PASSWORD=mongopass
MONGO_DB=faceattend_biometric
MONGO_PORT=27017

# Redis
REDIS_PORT=6379

# Kafka
KAFKA_PORT=9092

# Kong
KONG_PROXY_PORT=8080
KONG_ADMIN_PORT=8001

# Frontend
FRONTEND_PORT=8090
EXPO_PUBLIC_API_URL=

# Biometría
BIOMETRIC_FACIAL_MATCH_THRESHOLD=0.6
BIOMETRIC_FINGERPRINT_MATCH_THRESHOLD=8
BIOMETRIC_LIVENESS_TTL_SECONDS=120
BIOMETRIC_RATE_LIMIT_MAX_ATTEMPTS=20
BIOMETRIC_RATE_LIMIT_WINDOW_SECONDS=60
BIOMETRIC_LIVENESS_CHALLENGE_SECRET=faceattend-dev-only-biometric-liveness-secret-change-me

# SMTP
SMTP_HOST=host.docker.internal
SMTP_PORT=1025
```

### 10.4 CI/CD

- **Jenkins** para integración continua
- **GitHub Actions** para checks automáticos
- **Migraciones Liquibase** para versionamiento de base de datos
- **Docker** para empaquetado consistente

### 10.5 Observabilidad

- **Logs:** JSON estructurados (Pino para TS, structlog para Python, Zap para Go)
- **Métricas:** RED (Rate, Errors, Duration)
- **Tracing:** OpenTelemetry para distributed tracing
- **Alertas:** Configuradas para tiempo de respuesta < 5 min

---

## 11. PLAN DE TRABAJO

### 11.1 Metodología

- **Marco de trabajo:** Ágil con sprints de 2 semanas
- **Enfoque:** TDD (Test-Driven Development)
- **Documentación:** SDD (Software Design Documentation) - diseño antes que código

### 11.2 Fases del Proyecto

| Fase | Actividades | Duración |
|------|-------------|----------|
| **Fase 1: Discovery** | Análisis del problema, investigación de usuarios, benchmarking | Semana 1 |
| **Fase 2: Definition** | Definición de requerimientos, user stories, alcance | Semana 2 |
| **Fase 3: Detailed Design** | Arquitectura, modelo de datos, API contracts, ADRs | Semanas 2-3 |
| **Fase 4: Implementation** | Desarrollo TDD de microservicios y frontend | Semanas 3-10 |
| **Fase 5: Testing** | Pruebas unitarias, integración, E2E, IEEE 829 | Semanas 8-12 |
| **Fase 6: Deployment** | Configuración de producción, monitoreo, go-live | Semanas 12-14 |

### 11.3 Diagrama de Gantt

| Actividad | Mes 1 | Mes 2 | Mes 3 | Mes 4 | Mes 5 | Mes 6 |
|-----------|-------|-------|-------|-------|-------|-------|
| Discovery y Análisis | ████ | | | | | |
| Diseño y Arquitectura | ████ | ████ | | | | |
| Desarrollo Backend | | ████ | ████ | ████ | | |
| Desarrollo Frontend | | ████ | ████ | ████ | | |
| Integración | | | | ████ | ████ | |
| Testing y QA | | | | ████ | ████ | ████ |
| Documentación | ████ | ████ | ████ | ████ | ████ | ████ |
| Despliegue | | | | | | ████ |

### 11.4 Entregables por Sprint

| Sprint | Entregables |
|--------|-------------|
| Sprint 1 | Identity service, Authorization service, modelo de datos base |
| Sprint 2 | Academic service, Scheduling service, API Gateway |
| Sprint 3 | Attendance service, Biometric service (facial + dactilar enroll) |
| Sprint 4 | Frontend Web (Login, Dashboard, gestión de usuarios) |
| Sprint 5 | Frontend Web (Asistencia, reportes), Biometric (identify/verify) |
| Sprint 6 | Justificaciones, Notificaciones, Frontend Mobile |
| Sprint 7 | Integración completa, pruebas E2E, documentación |
| Sprint 8 | Ajustes finales, despliegue, capacitación |

---

## 12. EQUIPO DE TRABAJO

### 12.1 Integrantes

| Nombre | Rol | Categoría | Responsabilidad | Contacto |
|--------|-----|-----------|-----------------|----------|
| Diego Andrés Gutiérrez Nuñez | Administrador, Desarrollador, IA | Aprendiz del tecnólogo en Análisis y Desarrollo de Software | Análisis de información, diseño y programación | diangunu17@hotmail.com |
| Juan David Arboleda Perdomo | Analista, Base de Datos | Aprendiz del tecnólogo en Análisis y Desarrollo de Software | Análisis de información, diseño y programación | arboledaperdomo@gmail.com |
| Jonattan Steven Rizo Solano | Hardware, Desarrollador | Aprendiz del tecnólogo en Análisis y Desarrollo de Software | Análisis de información, diseño y programación | jonathanrizoth08@gmail.com |

### 12.2 Instructor

| Nombre | Rol |
|--------|-----|
| Motta Vargas José de Jesús | Instructor SENA |

---

## 13. PRESUPUESTO

### 13.1 Recursos Humanos

| Rol | Costo Mensual (COP) | Tiempo (meses) | Total (COP) |
|-----|---------------------|----------------|-------------|
| Administrador de Proyecto | 8,000,000 | 6 | 48,000,000 |
| Desarrollador de Software (x2) | 4,000,000 | 6 | 48,000,000 |
| Especialista en IA/Reconocimiento Facial | 5,000,000 | 3 | 15,000,000 |
| Administrador de Base de Datos | 10,000,000 | 3 | 30,000,000 |
| Ingeniero de Hardware/IoT | 10,000,000 | 2 | 20,000,000 |
| **TOTAL RECURSOS HUMANOS** | | | **161,000,000** |

### 13.2 Proveedores y Tecnologías

#### Software

| Proveedor | Detalle | Costo (COP) |
|-----------|---------|-------------|
| OpenCV / face_recognition | Software Open Source | Gratuito |
| Amazon Rekognition (alternativa) | ~4,000 COP por 1,000 imágenes | 40,000/mes |
| Microsoft Azure Face API (alternativa) | ~4,000 COP por 1,000 imágenes | 40,000/mes |

#### Hardware

| Proveedor | Detalle | Costo (COP) |
|-----------|---------|-------------|
| Logitech | Cámaras 1080p (x2) | 800,000 |
| Axis Communications | Cámaras IP (x2) | 2,400,000 |
| Dell/HP | Servidor básico | 6,000,000 |
| Raspberry Pi | Dispositivos IoT (x2) | 400,000 |

### 13.3 Resumen Presupuestal

| Concepto | Valor (COP) |
|----------|-------------|
| Recursos Humanos | 161,000,000 |
| Hardware (opción económica) | 7,200,000 |
| Software (Open Source) | 0 |
| **TOTAL ESTIMADO** | **168,200,000** |

**Presupuesto mínimo estimado:** $163.800.000 COP

---

## 14. GESTIÓN DE CALIDAD

### 14.1 Estándares Aplicados

| Estándar | Aplicación |
|----------|------------|
| **IEEE 829** | Documentación de pruebas de software |
| **ISO 25010** | Modelo de calidad del producto software |
| **ISO 29110** | Procesos para sistemas de software pequeños |
| **ISTQB** | Certificación de testing de software |

### 14.2 Estrategia de Pruebas

| Tipo de Prueba | Cobertura Objetivo | Herramienta |
|----------------|-------------------|-------------|
| Unitarias | ≥ 70% (≥ 80% domain) | JUnit, Jest, pytest |
| Integrías | Endpoints críticos | Supertest, TestContainers |
| E2E | Flujos principales | Detox, Manual |
| Rendimiento | P95 < 2000ms | k6, JMeter |
| Seguridad | OWASP Top 10 | Manual + herramientas |

### 14.3 Requerimientos No Funcionales

| ID | Categoría | Métrica |
|----|-----------|---------|
| NFR-001 | Rendimiento | P95 < 2000ms endpoints críticos, P99 < 5000ms |
| NFR-002 | Disponibilidad | Health checks GET /health y /health/ready |
| NFR-003 | Escalabilidad | Soportar 50% aumento de usuarios en < 1 año |
| NFR-004 | Seguridad | Autenticación opaca sessionId, bcrypt cost 12, RBAC, HTTPS |
| NFR-005 | Compatibilidad | Chrome, Firefox, Edge, Safari, Brave, Opera |
| NFR-006 | Usabilidad | Registro < 1 min, tasa éxito ≥ 90%, WCAG 2.1 AA |
| NFR-007 | Observabilidad | Logs JSON, métricas RED, distributed tracing |
| NFR-008 | Mantenibilidad | Cobertura ≥ 70%, complejidad ciclomática ≤ 10 |
| NFR-009 | Portabilidad | Docker images, variables de entorno |
| NFR-010 | Recuperación | RTO < 5 min, RPO 0, backups diarios |
| NFR-011 | Integridad | Registro único, evidencia inmutable, auditoría |

---

## 15. GESTIÓN DE RIESGOS

### 15.1 Riesgos Identificados

| ID | Riesgo | Probabilidad | Impacto | Mitigación |
|----|--------|--------------|---------|------------|
| R-01 | Fallo de reconocimiento facial en condiciones variables | Alta | Alta | Liveness detection, múltiples intentos, fallback manual |
| R-02 | Resistencia al cambio por parte de usuarios | Media | Alta | Capacitación, interfaz intuitiva, soporte continuo |
| R-03 | Problemas de conectividad en el aula | Media | Alta | Modo offline (futuro), registro manual de respaldo |
| R-04 | Crecimiento del alcance del proyecto | Alta | Media | Definición clara de MVP, roadmap por horizontes |
| R-05 | Incumplimiento de normativa de datos | Baja | Alta | Auditoría legal, cifrado, consentimiento explícito |
| R-06 | Deuda técnica acumulada | Media | Media | Refactorización continua, code reviews |
| R-07 | Dependencia de un solo PostgreSQL | Media | Alta | Plan de migración a instancias separadas |

### 15.2 Deuda Técnica Registrada

| ID | Descripción | Prioridad |
|----|-------------|-----------|
| AT-001 | Single PostgreSQL instance es SPOF | P1 |
| AT-002 | No hay lint rule de import-boundary | P2 |
| AT-003 | Go runtime añade 4º runtime para 2 tablas | P2 |
| AT-004 | Bearer no validado en TS/Python/Go services | P0 |
| AT-005 | Endpoints de recuperación de contraseña no implementados | P1 |
| AT-006 | No hay circuit breaker, transactional outbox, ni Kafka DLQ | P1 |
| AT-007 | Kong no tiene plugin JWT | P1 |

---

## 16. ANEXOS

### 16.1 Glosario de Términos

| Término | Definición |
|---------|------------|
| **Bounded Context** | Contexto delimitado en DDD donde un modelo de dominio es consistente |
| **Liveness Detection** | Verificación de que el rostro capturado pertenece a una persona viva |
| **Embedding** | Vector numérico de 128 dimensiones que representa las características faciales |
| **SessionId** | Identificador opaco (UUID) usado para autenticación sin estado |
| **RBAC** | Control de acceso basado en roles |
| **Saga** | Patrón para gestionar transacciones distribuidas |
| **DLQ** | Dead Letter Queue - cola de mensajes que no pudieron ser procesados |
| **ADR** | Architecture Decision Record - documento de decisión arquitectónica |
| **TDD** | Test-Driven Development - desarrollo guiado por pruebas |
| **SDD** | Software Design Documentation - documentación de diseño de software |

### 16.2 Referencias

- SRS FaceAttendEdu - Especificación de Requerimientos de Software
- Documentación IEEE 829 para pruebas de software
- ISO/IEC 25010 - Calidad del producto software
- ISO/IEC 29110 - Procesos para sistemas pequeños
- ISTQB - International Software Testing Qualifications Board
- Ley 1581 de 2012 - Protección de Datos Personales (Colombia)

### 16.3 Repositorio

- **Documentación:** `fae-docs/`
- **Código fuente:** `ProyectoFaceAttendEDU/`

---

**Fin del documento**

*Documento generado para el proyecto FaceAttend EDU - SENA Análisis y Desarrollo de Software 3145556*
