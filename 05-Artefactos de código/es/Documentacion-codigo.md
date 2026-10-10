---
title: "Documentación Técnica del Código Fuente — FaceAttendEDU"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
date: "10 de octubre de 2026"
abstract: |
  El presente documento constituye la documentación formal del código fuente del sistema FaceAttendEDU, redactada bajo los estándares de la séptima edición de las normas APA. Se describe la implementación de un monolito modular políglota, estructurado mediante una arquitectura hexagonal (puertos y adaptadores) para garantizar la segregación de la lógica de negocio frente a la infraestructura. El sistema integra nueve microservicios especializados desarrollados en Java, TypeScript, Python y Go, coordinados a través de un API Gateway (Kong) y un bus de eventos asíncronos (Apache Kafka). Se detalla la persistencia políglota mediante PostgreSQL, MongoDB y Redis, así como la implementación de algoritmos de similitud coseno para el procesamiento biométrico y modelos de puntuación ponderada para la evaluación de calidad basada en la norma ISO/IEC 25010. Esta documentación sirve como evidencia técnica de la alineación entre el diseño arquitectónico y la implementación real del software.
keywords:
  - arquitectura hexagonal
  - monolito modular políglota
  - persistencia políglota
  - biometría facial
  - ISO/IEC 25010
  - APA 7ma Edición
---

# Introducción

La implementación técnica de FaceAttendEDU representa la materialización de un ecosistema complejo de gestión de asistencia académica, donde la precisión biométrica y la seguridad de los datos son los pilares fundamentales. El desarrollo del sistema no se limitó a la elección de un único lenguaje de programación, sino que adoptó una estrategia de selección tecnológica basada en la especialización funcional, resultando en un entorno políglota.

## Declaración del Problema
La gestión de asistencia en instituciones académicas suele enfrentarse a problemas de suplantación de identidad y a la ineficiencia de los registros manuales. FaceAttendEDU aborda este desafío mediante la automatización del registro a través de biometría facial y dactilar, eliminando el error humano y el fraude.

## Objetivos del Documento
El objetivo primordial de este documento es proveer una descripción exhaustiva y formal de la implementación del código fuente. Se busca documentar no solo el "qué" hace el sistema, sino el "cómo" está construido, detallando la relación entre las entidades de dominio, los casos de uso y los adaptadores de infraestructura, asegurando que el sistema sea mantenible, auditable y escalable.

## Alcance de la Documentación
Este documento cubre la totalidad del back-end del sistema, incluyendo los nueve microservicios de negocio y el API Gateway. Se detalla la lógica implementada en Java, TypeScript, Python y Go, así como la configuración de la infraestructura de datos y la orquestación de contenedores.

# Arquitectura del Sistema

El sistema FaceAttendEDU se fundamenta en un paradigma de **Monolito Modular Políglota**. A diferencia de los microservicios distribuidos tradicionalmente, el sistema se organiza en módulos independientes dentro de un mismo repositorio, permitiendo un despliegue coordinado mientras se mantiene la autonomía lógica de cada contexto delimitado (*bounded context*).

## Patrones Arquitectónicos

### Monolito Modular Políglota
La elección de un enfoque políglota responde a la necesidad de optimizar el rendimiento según la tarea. Se implementó Java para la orquestación de procesos robustos, Python para el procesamiento científico de vectores biométricos, Go para la alta concurrencia en notificaciones y TypeScript para servicios ligeros. Esta estrategia permite que cada módulo sea desarrollado con la herramienta más eficiente para su propósito específico.

### Arquitectura Hexagonal (Puertos y Adaptadores)
Para mitigar el acoplamiento tecnológico, el sistema implementa la Arquitectura Hexagonal. Esta estructura organiza el código en tres capas concéntricas:
1. **Capa de Dominio (The Core)**: Contiene las entidades y reglas de negocio puras, totalmente independientes de cualquier framework o librería externa.
2. **Capa de Aplicación**: Define los "Casos de Uso", que actúan como orquestadores que reciben datos de los puertos de entrada y coordinan la ejecución del dominio.
3. **Capa de Infraestructura (Adapters)**: Implementa los puertos definidos por el dominio. Incluye los controladores REST (entrada) y las implementaciones de persistencia o mensajería (salida).

## Infraestructura y Stack Tecnológico

### Estrategia de Persistencia Políglota
El sistema evita la dependencia de un único motor de base de datos, aplicando la persistencia según la naturaleza del dato:
- **PostgreSQL 17**: Utilizado para datos relacionales y estructurados en 8 esquemas independientes, garantizando consistencia ACID.
- **MongoDB 7**: Empleado exclusivamente por el servicio biométrico para el almacenamiento de *embeddings* (vectores de alta dimensionalidad), optimizando la búsqueda de similitudes.
- **Redis 7**: Implementado para la gestión de sesiones y el control de tráfico (*rate limiting*) en el Gateway, reduciendo la latencia de respuesta.

### Comunicación Basada en Eventos
La sincronización entre módulos se realiza mediante **Apache Kafka 3.8.0**. El sistema emplea un modelo de publicación/suscripción donde los servicios emiten "Eventos de Dominio" (ej. `UserCreated`, `AttendanceRecorded`). Esto permite que servicios como el de Notificaciones reaccionen asíncronamente a cambios en otros módulos sin generar dependencias circulares.

### Gestión de Tráfico y Seguridad (Edge Routing)
El acceso al sistema está centralizado en **Kong Gateway 3.6**, configurado en modo *DB-less*. El Gateway es responsable de:
- **Enrutamiento**: Mapeo de rutas externas a servicios internos.
- **Seguridad**: Validación de tokens JWT RS256 y aplicación de políticas de CORS.
- **Resiliencia**: Limitación de tasa para prevenir ataques de denegación de servicio.

# Implementación de los Microservicios

## 01-ms-identity: Gestión de Identidad y Sesiones
Este servicio constituye el núcleo de seguridad del sistema, gestionando la existencia legal de las personas y sus credenciales de acceso.

### Modelo de Dominio y Casos de Uso
El dominio se centra en las entidades `Person`, `User` y `UserSession`. Los casos de uso principales incluyen el registro de usuarios, la autenticación mediante contraseñas hasheadas y la recuperación de cuentas mediante códigos temporales SHA-256 con un TTL de 10 minutos.

### Lógica de Implementación
Implementa un mecanismo de **Expiración Perezosa (Lazy Expiration)**. Las sesiones no son eliminadas activamente por un proceso en segundo plano, sino que se validan en cada solicitud; si el tiempo de vida ha expirado, el servicio cierra la sesión en el momento de la lectura.

## 02-ms-authorization: Control de Acceso Basado en Roles (RBAC)
Implementa la seguridad granular mediante el patrón RBAC, asegurando que cada usuario acceda solo a las funciones permitidas por su rol.

### Implementación del RBAC
El sistema mapea la relación `Usuario → Rol → Permiso`. Para optimizar la evaluación de permisos en tiempo real, el servicio utiliza **Redis** para cachear el árbol de permisos de cada usuario, evitando consultas repetitivas a PostgreSQL.

## 03-ms-academic: Estructura Académica
Gestiona la jerarquía organizacional de la institución, desde la sede hasta el estudiante.

### Modelado de Datos
Utiliza un modelo relacional complejo que vincula `School` $\rightarrow$ `Program` $\rightarrow$ `Cohort` $\rightarrow$ `Course`. Para gestionar estas relaciones con alta eficiencia, implementa **Drizzle ORM**, facilitando la ejecución de JOINs optimizados sobre el esquema `academic`.

## 04-ms-scheduling: Programación de Sesiones
Transforma la estructura académica en un calendario operativo de clases.

### Prevención de Conflictos
La lógica central implementa restricciones de unicidad en la base de datos sobre la terna `(ambiente, día, hora)`, impidiendo la programación de dos clases en el mismo espacio físico o la asignación de un instructor a dos sesiones simultáneas.

## 05-ms-attendance: Registro de Asistencia y Justificaciones
Es el módulo transaccional donde se registran las marcas de tiempo de entrada y salida.

### Flujo de Justificación
Implementa un flujo de estados para la gestión de inasistencias: `Sometido` $\rightarrow$ `Pendiente` $\rightarrow$ `Aprobado/Rechazado`. Los soportes digitales se almacenan en un sistema de objetos compatible con S3 (MinIO).

## 06-ms-biometric: Procesamiento de Vectores Biométricos
Es el componente más complejo técnicamente, encargado de la extracción y comparación de rasgos faciales y dactilares.

### Algoritmo de Similitud Coseno
Para la identificación 1:N, el sistema extrae un vector de características (*embedding*) de la imagen capturada utilizando modelos de redes neuronales profundas (FaceNet/ArcFace) vía **OpenCV DNN**. La comparación se realiza mediante la **Similitud Coseno**, calculando el coseno del ángulo entre dos vectores en un espacio multidimensional:
$$\text{similitud} = \frac{\mathbf{A} \cdot \mathbf{B}}{\|\mathbf{A}\| \|\mathbf{B}\|}$$
Donde $\mathbf{A}$ es el vector almacenado en MongoDB y $\mathbf{B}$ es el vector capturado.

## 07-ms-configuration: Parámetros y Actualizaciones
Gestiona la configuración global del sistema y el flujo de aprobación para cambios biométricos.

### Workflow de Actualización Biométrica
Implementa un proceso de auditoría donde cualquier solicitud de cambio de plantilla biométrica debe pasar por un estado de `Revisión` antes de ser `Aprobada`, evitando que un usuario altere sus datos para suplantar a otro.

## 08-ms-notification: Sistema de Alertas y Notificaciones
Implementado en **Go**, este servicio optimiza el envío de alertas masivas y notificaciones push.

### Orquestación Basada en Eventos
El servicio consume tópicos de Kafka de todos los demás módulos. Por ejemplo, al detectar un evento de `InasistenciaDetectada`, el servicio dispara automáticamente una notificación al tutor y al estudiante a través de Firebase y SMTP.

## 09-ms-quality: Evaluación de Calidad de Software
Implementa instrumentos de medición basados en estándares internacionales para evaluar la madurez del sistema.

### Modelo de Puntuación Ponderada
Para la evaluación bajo la norma **ISO/IEC 25010**, el sistema implementa un modelo de puntuación ponderada. Cada característica de calidad (ej. Portabilidad, Mantenibilidad) tiene un peso relativo. El puntaje global se calcula como:
$$\text{Puntaje Global} = \sum (\text{Puntaje Característica}_i \times \text{Peso}_i)$$

## 99-api-gateway: Orquestación de API
El Gateway actúa como la fachada del sistema, abstrayendo la complejidad de los microservicios internos.

### Implementación de Kong
Configurado en modo *DB-less*, el Gateway utiliza un archivo declarativo `kong.yml` para definir el enrutamiento. Implementa la seguridad en el "borde" (*edge*), validando que cada solicitud posea un token JWT válido antes de redirigirla al servicio correspondiente.

# Consideraciones Transversales

## Seguridad y Autorización
La seguridad se implementa en capas. El Gateway valida la identidad, mientras que el servicio de Autorización valida el permiso específico. Esta separación permite cambiar la política de permisos sin afectar el mecanismo de autenticación.

## Observabilidad y Calidad
La implementación de la norma **ISO/IEC 29110** se refleja en el proceso de desarrollo, mientras que el servicio de calidad (`09-ms-quality`) permite que el sistema se autoevalúe periódicamente, generando reportes de conformidad técnica.

# Discusión y Conclusiones

La adopción de un monolito modular políglota ha permitido a FaceAttendEDU optimizar el rendimiento en áreas críticas, como la biometría y las notificaciones, sin sacrificar la simplicidad del despliegue. La Arquitectura Hexagonal ha demostrado ser eficaz para aislar la lógica de negocio, permitiendo que el sistema evolucione tecnológicamente sin riesgo de regresiones en las reglas de negocio.

A pesar de la complejidad añadida por la gestión de múltiples lenguajes, los beneficios en términos de eficiencia computacional y escalabilidad modular justifican la arquitectura implementada. El sistema se presenta como una solución robusta y alineada con los estándares internacionales de ingeniería de software.

# Referencias

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). https://doi.org/10.1037/0000165-000

Cockburn, A. (2005). *Hexagonal Architecture (Ports and Adapters)*.

International Organization for Standardization. (2011). *Systems and software engineering — Systems and software quality requirements and evaluation (SQuaRE) — System and software quality models* (ISO/IEC 25010:2011).

International Organization for Standardization. (2011). *Software engineering — Lifecycle profiles for Very Small Entities (VSEs)* (ISO/IEC 29110).

IEEE. (n.d.). *Standard for Biometric Data Interchange*.
