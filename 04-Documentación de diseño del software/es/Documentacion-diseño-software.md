---
title: "Documentación de Diseño Arquitectónico"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
abstract: |
  El presente documento detalla la arquitectura técnica del sistema FaceAttendEDU, describiendo el modelo arquitectónico, los patrones de diseño implementados, el modelo de datos y el stack tecnológico utilizado. Se explica la aplicación de la Arquitectura Hexagonal y los principios de Domain-Driven Design (DDD) para garantizar la escalabilidad, mantenibilidad y seguridad del sistema. El documento sirve como guía técnica para desarrolladores, auditores y arquitectos de software, asegurando que la implementación técnica esté alineada con los objetivos estratégicos y funcionales del proyecto.
keywords:
  - diseño arquitectónico
  - arquitectura hexagonal
  - patrones de diseño
  - modelo de datos
  - microservicios
  - DDD
---

# Introducción

El diseño arquitectónico de FaceAttendEDU representa la traducción de los requerimientos funcionales y no funcionales en una estructura técnica robusta. A diferencia del análisis de software, que define el "qué" y el "por qué", este documento se centra en el "cómo": la organización del código, la gestión de datos, la comunicación entre componentes y la selección tecnológica.

El sistema ha sido concebido bajo un enfoque de **Polyglot Modular Monolith** (organizado en un monorepo), donde cada módulo opera como un microservicio independiente con su propio contexto delimitado (*bounded context*), permitiendo la coexistencia de múltiples lenguajes de programación según la necesidad técnica (Java, TypeScript, JavaScript, Python y Go).

## Objetivo del documento
Proporcionar una descripción detallada y formal de la arquitectura del sistema, los patrones de diseño aplicados y el modelo de datos, estableciendo la base técnica para la implementación, el despliegue y la evolución del software.

## Alcance del documento
Este documento cubre la arquitectura de los servicios del back-end, la estructura de los front-ends, la estrategia de persistencia de datos, la gestión de API mediante Gateway y el flujo de información entre capas.

# Modelo Arquitectónico

## Arquitectura Hexagonal (Ports and Adapters)
FaceAttendEDU implementa la **Arquitectura Hexagonal**, cuyo objetivo principal es aislar la lógica de negocio (el núcleo) de las tecnologías externas (frameworks, bases de datos, APIs). Esta separación garantiza que el sistema sea testeable y que los cambios en la infraestructura no afecten las reglas de negocio.

La estructura interna de cada servicio se divide en tres capas principales:

### 1. Capa de Dominio (The Core)
Es el centro del hexágono y la parte más estable del sistema.
- **Entidades y Objetos de Valor**: Representan los conceptos del negocio (ej. `User`, `AttendanceRecord`).
- **Puertos (Ports)**: Interfaces que definen cómo el dominio interactúa con el exterior. Existen puertos de entrada (para casos de uso) y puertos de salida (para persistencia o servicios externos).
- **Reglas de Negocio**: Lógica pura que no depende de ninguna librería externa.

### 2. Capa de Aplicación
Actúa como el orquestador del sistema.
- **Casos de Uso (Use Cases)**: Implementan la lógica de aplicación. Reciben datos de los adaptadores de entrada, coordinan el dominio y devuelven una respuesta.
- **Orquestación**: No contienen reglas de negocio complejas, sino que dirigen el flujo de datos entre el dominio y la infraestructura.

### 3. Capa de Infraestructura (Adapters)
Contiene las implementaciones concretas de los puertos.
- **Adaptadores de Entrada (Driving Adapters)**: Controladores REST (Spring Boot, FastAPI, etc.) que reciben solicitudes HTTP y las traducen a llamadas de casos de uso.
- **Adaptadores de Salida (Driven Adapters)**: Implementaciones de persistencia (JPA, MongoDB, pgx) que traducen las necesidades del dominio en consultas a bases de datos.

## Flujo de Información
Un flujo típico de una solicitud sigue la siguiente ruta:
`Cliente` $\rightarrow$ `Kong Gateway` $\rightarrow$ `Controller (Infrastructure)` $\rightarrow$ `Use Case (Application)` $\rightarrow$ `Domain Entity/Port (Domain)` $\rightarrow$ `Persistence Adapter (Infrastructure)` $\rightarrow$ `Database`.

# Patrones de Diseño

Para asegurar la calidad del código y la facilidad de mantenimiento, se han aplicado los siguientes patrones:

## Patrones Estructurales y de Comportamiento
- **Repository Pattern**: Se utiliza para abstraer la capa de datos. El dominio define una interfaz (puerto) y la infraestructura la implementa, permitiendo cambiar la base de datos sin afectar el negocio.
- **Use Case / Interactor**: Cada funcionalidad del sistema se encapsula en una clase de caso de uso única, facilitando la trazabilidad y las pruebas unitarias.
- **Data Transfer Object (DTO)**: Se utilizan objetos específicos para el transporte de datos entre la API y la aplicación, evitando exponer las entidades del dominio directamente al cliente.
- **Mapper Pattern**: Componentes encargados de transformar entidades de dominio en DTOs y viceversa, manteniendo la pureza de la capa de dominio.
- **API Gateway**: Implementado mediante **Kong 3.6**, centralizando la autenticación (JWT RS256), el enrutamiento, la limitación de tasa (*rate limiting*) y la seguridad.

## Patrones de Datos y Mensajería
- **Database-per-Service**: Cada microservicio posee su propio esquema en PostgreSQL, eliminando dependencias físicas (FKs) entre contextos y asegurando el bajo acoplamiento.
- **Event-Driven Architecture (EDA)**: Uso de **Apache Kafka** para la comunicación asíncrona. Los servicios publican eventos de dominio (ej. `attendance-events`) que pueden ser consumidos por otros módulos en el futuro.
- **Polyglot Persistence**: Selección de la base de datos según el tipo de dato:
    - **PostgreSQL**: Para datos estructurados y relacionales.
    - **MongoDB**: Para almacenamiento de vectores (biometría facial y dactilar).
    - **Redis**: Para caché y control de tráfico en el Gateway.

# Modelo de Datos

El sistema utiliza una arquitectura de datos distribuida basada en esquemas lógicos vinculados mediante UUIDs.

## Entidades Principales y Relaciones
### Contexto de Identidad y Autorización
- **User**: Entidad central vinculada a credenciales y sesiones.
- **Role & Permission**: Implementación de **RBAC (Role-Based Access Control)**. Un rol agrupa múltiples permisos, y un usuario puede tener uno o varios roles.

### Contexto Académico y Asistencia
- **Academic Actor**: Representa la relación entre una persona y su rol en el colegio (estudiante, instructor).
- **Course (Ficha) & Program**: Estructura jerárquica de la oferta académica.
- **Attendance Record**: Registra la entrada/salida, vinculando al usuario, el dispositivo IoT y la marca temporal.
- **Justification**: Entidad vinculada a un registro de asistencia, permitiendo la carga de soportes digitales.

## Estrategia de Persistencia
| Tipo de Dato | Tecnología | Justificación |
|---|---|---|
| Datos Relacionales | PostgreSQL 17 | Consistencia ACID y soporte para esquemas complejos. |
| Embeddings Biométricos | MongoDB 7 | Alta eficiencia en la búsqueda y almacenamiento de vectores de alta dimensionalidad. |
| Sesiones y Rate Limit | Redis 7 | Baja latencia para validaciones en tiempo real. |

# Información Técnica y Stack Tecnológico

El éxito del proyecto FaceAttendEDU se basa en una selección tecnológica orientada al rendimiento y la especialización.

## Stack Tecnológico
| Componente | Tecnología | Versión | Propósito |
|---|---|---|---|
| **Backend (Core)** | Java / Spring Boot | 21 / 4.1.1 | Servicios de identidad y autorización. |
| **Backend (Biometría)** | Python / FastAPI | 3.12 / 0.110 | Procesamiento de vectores biométricos. |
| **Backend (Notificaciones)** | Go / Gin | 1.22 / v1.9.1 | Alta concurrencia en alertas. |
| **Backend (TS)** | TypeScript / Fastify | 5.4 / 4.26.0 | Servicios ligeros y rápidos. |
| **Frontend** | React Native / Expo | RN 0.86 / Expo 57 | App móvil multiplataforma. |
| **Gateway** | Kong | 3.6 | Orquestación de API y Seguridad. |
| **Mensajería** | Apache Kafka | 3.8.0 | Comunicación asíncrona. |
| **Migraciones** | Liquibase | 4.29.0 | Control de versiones de base de datos. |

## Factores de Éxito Técnico
1. **Desacoplamiento Total**: Gracias a la Arquitectura Hexagonal, el sistema puede evolucionar sus frameworks sin reescribir la lógica de negocio.
2. **Seguridad Robusta**: El uso de JWT RS256 y un Gateway centralizado garantiza que ninguna solicitud llegue a los microservicios sin ser validada.
3. **Persistencia Políglota**: El uso de MongoDB para biometría y PostgreSQL para administración optimiza el rendimiento según la naturaleza del dato.
4. **Escalabilidad Modular**: La estructura de microservicios permite escalar independientemente el módulo de asistencia (alta carga) del módulo de configuración (baja carga).

# Conclusiones

El diseño arquitectónico de FaceAttendEDU no es solo una elección técnica, sino una estrategia para mitigar los riesgos identificados en la fase de análisis. La adopción de la Arquitectura Hexagonal y DDD permite que el sistema sea resiliente al cambio y fácil de auditar. La combinación de lenguajes especializados (Java para robustez, Python para IA/Biometría, Go para velocidad) asegura que cada componente sea implementado con la herramienta más eficiente para su propósito.

**Nota del autor.** Jonas es el autor y responsable técnico del diseño arquitectónico. La correspondencia técnica puede dirigirse a jonas@consultoria.example.

# Referencias

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). https://doi.org/10.1037/0000165-000

International Organization for Standardization & International Electrotechnical Commission. (2011). *Systems and software engineering — Systems and software quality requirements and evaluation (SQuaRE) — System and software quality models* (ISO/IEC 25010:2011). https://www.iso.org/standard/35733.html

Evans, E. (2003). *Domain-Driven Design: Tackling Complexity in the Heart of Software*. Addison-Wesley.

Alistair Cockburn. (2005). *Hexagonal Architecture (Ports and Adapters)*.
