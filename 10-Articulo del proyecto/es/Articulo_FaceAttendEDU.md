---
title: "FaceAttend EDU: sistema de gestión de asistencia con reconocimiento facial para ambientes de formación del SENA"
authors: Jonattan Steven Rizo Solano, Diego Andrés Gutiérrez Nuñez, Juan David Arboleda Perdomo
date: "22 de octubre de 2026"
abstract: |
  En muchos ambientes de formación del SENA, la asistencia depende de un instructor que llama lista a más de 30 aprendices. Esto produce registros inconsistentes, quita tiempo de clase y retrasa el seguimiento de ausencias y retardos. FaceAttend EDU es una plataforma web y móvil que registra la entrada y salida por reconocimiento facial, con registro manual o por importación cuando el escaneo falla, y que centraliza el historial, las justificaciones, los reportes por rol y las alertas. El sistema se construyó con Scrum en sprints de dos semanas, documentación de diseño antes del código (SDD) y desarrollo guiado por pruebas (TDD). La arquitectura tiene nueve microservicios hexagonales detrás de Kong, autenticación por sesión opaca validada en el borde, PostgreSQL con un schema por contexto, MongoDB para los vectores biométricos, Kafka para eventos y clientes Expo para web y Android. El reporte IEEE 829 del 21 de septiembre de 2026 registra 81 pruebas con 96,3 % de éxito y 72 % de cobertura de código. La valoración ISO/IEC 25010 subió el servicio de identidad de un puntaje estimado de 2,6/5 a 4,0/5. El artículo describe también las medidas de privacidad exigidas por la Ley 1581 de 2012, parte de ellas todavía en desarrollo, y las limitaciones de la evidencia disponible.
keywords:
  - reconocimiento facial
  - gestión de asistencia
  - microservicios
  - arquitectura hexagonal
  - protección de datos personales
  - ISO/IEC 25010
  - IEEE 829
  - SENA
---

# Resumen

En muchos ambientes de formación del SENA la asistencia depende de un instructor que llama lista a más de 30 aprendices [7], [8]. FaceAttend EDU automatiza ese registro con reconocimiento facial, ofrece registro manual o por importación cuando el escaneo falla y centraliza historiales, justificaciones, reportes y alertas. Este artículo presenta el problema, los objetivos, el marco normativo y de calidad, la metodología, la arquitectura, las medidas de seguridad y privacidad y los resultados de las pruebas registradas hasta octubre de 2026. También discute qué falta por validar, en especial el piloto con usuarios reales.

**Palabras clave:** reconocimiento facial, gestión de asistencia, microservicios, arquitectura hexagonal, protección de datos personales, ISO/IEC 25010, IEEE 829, SENA.

# Introducción

## Problema

En los procesos académicos del SENA la asistencia suele tomarse con una lista manual a cargo del instructor. La especificación de requisitos del proyecto señala que delegar el registro de más de 30 usuarios en una sola persona, que además tiene otras responsabilidades, es ineficiente y propenso a errores [8]. Las consecuencias que identifica el equipo son cuatro: registros inconsistentes, sobrecarga administrativa, riesgo de suplantación y seguimiento tardío de ausencias y retardos [7].

Las soluciones actuales (lista en papel, hojas de cálculo informales, excusas por correo o mensaje) no dan visibilidad en tiempo real, son difíciles de consolidar por fecha, ficha o ambiente y no dejan rastro de quién aprobó o rechazó una justificación [8]. La revisión documental del equipo encontró que los proveedores comerciales de asistencia biométrica no incluyen un flujo de justificaciones y que ninguna de las opciones revisadas integra ficha, horario y justificación [8].

## Contexto

FaceAttend EDU es el proyecto formativo del equipo 10 de la ficha 3145556 del Tecnólogo en Análisis y Desarrollo de Software (ADSO). El equipo tiene tres aprendices y no cuenta con un rol dedicado de operaciones [7]. El alcance está congelado en el MVP del horizonte 1 (0 a 3 meses) hasta validar el producto en un piloto [7].

Los usuarios son tres: el aprendiz, que registra su asistencia sin contacto, consulta su historial y presenta justificaciones; el instructor, que abre y cierra sesiones de clase, ve la asistencia de su ficha, recibe alertas y revisa justificaciones; y el administrador, que gestiona usuarios, roles, estructura académica y configuración [7].

# Objetivos

## Objetivo general

Desarrollar una solución de software para el registro de asistencia, la consulta de historiales, la gestión de justificaciones y la generación de reportes, que garantice la seguridad de los datos conforme a la Ley 1581 de 2012, la usabilidad y la trazabilidad [7].

## Objetivos específicos

1. Gestionar usuarios, roles y permisos, con carga masiva por CSV (contextos Identity y Authorization).
2. Gestionar ambientes, fichas, programas, cursos y periodos académicos (Academic y Scheduling).
3. Registrar entrada y salida por reconocimiento facial, con registro manual o por importación como alternativa (Attendance y Biometric).
4. Configurar el sistema y emitir alertas por retardos y ausencias (Configuration y Notification).
5. Gestionar el flujo de justificaciones con documentos de soporte (Attendance).
6. Generar reportes y tableros por fecha, ficha y ambiente (Attendance y Academic).

Cada objetivo específico está trazado a los requisitos funcionales RF 1 a RF 8 y al requisito no funcional de seguridad RNF 4 de la especificación [7].

# Marco de referencia

## Ley 1581 de 2012

La Ley Estatutaria 1581 de 2012 establece el régimen general de protección de datos personales en Colombia [1]. Clasifica los datos biométricos como datos sensibles (art. 5) y solo permite tratarlos con autorización explícita del titular, salvo excepciones legales (art. 6). Para FaceAttend EDU esto implica que el enrolamiento facial requiere consentimiento previo, que los vectores biométricos deben tratarse como información personal protegida y que el aprendiz debe poder conocer, actualizar y revocar su autorización. El documento de alcance del proyecto lo recoge como supuesto: el sistema apoya la gestión educativa, no es un mecanismo legal de control de asistencia, y el tratamiento biométrico exige consentimiento y cifrado en reposo [7].

## ISO/IEC 25010

ISO/IEC 25010:2011 define un modelo de calidad de producto con ocho características: adecuación funcional, eficiencia de desempeño, compatibilidad, usabilidad, fiabilidad, seguridad, mantenibilidad y portabilidad [2]. El equipo convirtió el modelo en un instrumento de 24 ítems con escala Likert de 1 a 5, publicado por el servicio `ms-quality`, que calcula un puntaje global ponderado: adecuación funcional 20 %, eficiencia, fiabilidad y seguridad 15 % cada una, compatibilidad, usabilidad y mantenibilidad 10 % cada una y portabilidad 5 % [13].

## IEEE 829

IEEE Std 829-1998 define la documentación de pruebas de software: plan, especificación de casos, reporte de incidentes y reporte resumido [3]. El proyecto produjo los cuatro documentos para el back-end [12]. El plan de pruebas fija como criterios de aceptación una tasa de éxito de al menos 95 %, una cobertura de al menos 70 %, un P95 de máximo 200 ms y una tasa de errores 5xx de máximo 1 % [11].

# Metodología

## Scrum con sprints de dos semanas

El equipo trabaja con Scrum [4] en sprints de dos semanas, de lunes a viernes de la segunda semana, con una capacidad estimada de 20 puntos de historia por sprint [10]. El primer sprint fue del 16 al 27 de septiembre de 2026 [10]. Las ceremonias son planeación (máximo 2 horas), reunión diaria de 15 minutos, refinamiento a mitad de sprint, revisión y retrospectiva; cada retrospectiva debe producir al menos una acción de mejora con responsable y fecha [10]. Los instructores técnicos actúan como Product Owner, Diego Andrés Gutiérrez Nuñez como Scrum Master, y Juan David Arboleda Perdomo y Jonattan Steven Rizo Solano como desarrolladores [16].

## SDD y TDD

El plan de trabajo combina dos prácticas [16]. La primera es SDD (Software Design Documentation): el diseño se documenta antes de escribir código y la documentación se actualiza después de implementar. Esa documentación vive en el repositorio `fae-docs` e incluye 20 registros de decisiones de arquitectura (ADR) [9]. La segunda es TDD [6]: cada cambio empieza con una prueba que falla, sigue con el código mínimo que la hace pasar y termina con una refactorización. La arquitectura hexagonal abarata este ciclo porque el dominio no importa ningún framework y se prueba con dobles en memoria [11]. Los identificadores de casos IEEE 829 (`TC-xx-xxx`) aparecen en los nombres de las pruebas para trazarlas a los requisitos [11].

La integración continua corre en GitHub Actions: el flujo `ci.yml` ejecuta las pruebas de los nueve servicios y de los dos clientes en cada push o pull request a `main` [9].

# Arquitectura

## Microservicios hexagonales

El sistema se divide en ocho contextos delimitados (Identity, Authorization, Academic, Scheduling, Attendance, Biometric, Configuration y Notification) más un servicio de herramientas de calidad. Cada uno corre en su propio contenedor, en los puertos 8081 a 8089 [7]. Cada servicio sigue la arquitectura hexagonal de puertos y adaptadores [5], con carpetas `domain/`, `application/` e `infrastructure/` [9].

| # | Servicio | Responsabilidad | Tecnología |
|---|---|---|---|
| 01 | `ms-identity` | Personas, usuarios, sesiones, política de contraseñas | Java 21 + Spring Boot 4.1 |
| 02 | `ms-authorization` | Roles, permisos y asignaciones (RBAC) | Java 21 + Spring Boot 4.1 |
| 03 | `ms-academic` | Sedes, programas, periodos, fichas, cursos, matrículas | TypeScript + Fastify |
| 04 | `ms-scheduling` | Ambientes, bloques horarios, sesiones de clase sin doble reserva | Java 21 + Spring Boot 4.1 |
| 05 | `ms-attendance` | Registros de asistencia, justificaciones, soportes, reportes | Java 21 + Spring Boot 4.1 |
| 06 | `ms-biometric` | Vectores faciales y dactilares | Python 3.12 + FastAPI |
| 07 | `ms-configuration` | Configuración académica y de seguridad | TypeScript + Fastify |
| 08 | `ms-notification` | Tipos de alerta y alertas | TypeScript + Fastify |
| 09 | `ms-quality` | Instrumentos ISO/IEC 25010, ISTQB e ISO/IEC 29110 | TypeScript + Fastify |

Fuentes de la tabla: [7] y el código de la rama `develop` [17]. El servicio de notificaciones se diseñó en Go 1.22 con Gin, y así aparece en los reportes de septiembre de 2026 [12], [14]. Luego se reescribió en TypeScript con Fastify para compartir el runtime de Node con Academic y Configuration; la versión vigente es la de TypeScript [17].

## Kong y sesión opaca

Kong OSS 3.6 es la única puerta de entrada. Corre sin base de datos, con configuración declarativa en `kong.yml`, y enruta por ruta a los nueve servicios [9]. La autenticación no usa JWT: al iniciar sesión, `ms-identity` emite un identificador de sesión opaco (un UUID) que se guarda en la tabla `identity.user_session` [9]. En cada ruta no pública, una función previa de Kong exige la cabecera `Authorization: Bearer <uuid>` y consulta a `ms-identity` si la sesión sigue activa. Responde 401 si la sesión falta, no es válida o expiró, y 503 si el servicio de identidad no contesta. Las respuestas positivas se guardan 10 segundos en caché y se invalidan al cerrar sesión [9]. Además, cada servicio repite la validación de sesión y de permisos con su propio guardián, como defensa en profundidad [9]. Las contraseñas se guardan con bcrypt de costo 12 [9], [17].

## Datos: PostgreSQL por schema y MongoDB para embeddings

Los datos relacionales viven en una sola base lógica, `faceattend_db`, en PostgreSQL 17, con un schema por contexto, un rol de base de datos por schema y ninguna llave foránea entre schemas. Las referencias entre contextos son identificadores públicos que se resuelven por API o por eventos [9]. Las migraciones se hacen con Liquibase en ocho trabajos encadenados, uno por contexto [9].

Los vectores biométricos viven en MongoDB 7 porque su patrón de acceso es la búsqueda por similitud, no la consulta relacional [9]. El servicio biométrico detecta el rostro con OpenCV y extrae un vector de 128 dimensiones con `face_recognition` (basado en dlib) [9]. En el código vigente, el umbral de coincidencia es una similitud coseno de 0,85. Durante la prueba de vida, la comprobación de que todos los cuadros son de la misma persona usa un umbral más flexible, 0,75, porque los cuadros tomados a mitad de un gesto alteran el vector [17].

## Eventos con Kafka

La comunicación síncrona pasa por REST a través de Kong. La asíncrona usa eventos de dominio en Apache Kafka en modo KRaft, publicados con `KafkaTemplate` en Java y `kafkajs` en TypeScript [9]. El registro de asistencia se escribe en una transacción local del schema `attendance`; las reacciones desacopladas, como las alertas, se disparan por evento después del commit [9]. El patrón outbox, las colas de mensajes fallidos y los manejadores de compensación aún no están implementados: es la deuda técnica AT-006 [9].

## Clientes Expo para web y móvil

Los dos clientes usan Expo SDK 57 [9], [17]. El cliente web es una exportación estática de Expo Web servida por nginx; incluye el SDK de DigitalPersona para la huella dactilar. La app móvil Android se compila y firma en EAS [9]. Ambos usan el patrón MVVM y la cámara mediante `expo-camera`, con una prueba de vida antes de capturar el rostro [9].

## Despliegue

Según el ADR-020, el back-end se despliega con Docker Compose en una instancia AWS EC2 `t3.xlarge` con 13 contenedores (nueve servicios, Kong, MongoDB, Redis y Kafka). PostgreSQL corre en Neon, en la región `sa-east-1`, y el cliente web se publica en Vercel [9].

# Seguridad y privacidad

## Lo que está implementado

- Autenticación por sesión opaca validada en Kong y en cada servicio, con rotación de sesión (prueba `RefreshSessionUseCaseImplTest`) [9], [15].
- Bloqueo de la cuenta tras 5 intentos fallidos de inicio de sesión, durante 30 minutos, valores por defecto de `AuthProperties` (prueba `shouldLockAccountAfterFiveFailedAttempts`) [15], [17].
- Control de acceso por roles (administrador, instructor, aprendiz), probado en `CheckPermissionUseCaseImplTest` [15].
- Contraseñas con bcrypt de costo 12 [9].
- Prueba de vida contra ataques de presentación: una foto o una pantalla se rechaza con HTTP 400 [15].
- Trazabilidad: cada petición lleva un identificador de correlación y los errores salen en un formato uniforme, sin trazas de pila [13], [14].

## Lo que está en desarrollo

Al 22 de octubre de 2026, tres controles exigidos por la Ley 1581 y por los ADR aceptados todavía no están en la rama `develop` [17]:

1. **Consentimiento informado.** El enrolamiento biométrico debe registrar la autorización explícita del aprendiz antes de capturar el rostro, como exige el artículo 6 de la Ley 1581 [1], [7].
2. **Cifrado de biometría y datos personales.** El ADR-016 acepta el cifrado en reposo de los datos personales y los vectores biométricos, pero su nota del 17 de octubre de 2026 confirma que no está implementado; hoy el cifrado en reposo depende de los proveedores administrados [9]. El trabajo en curso cifra esos campos en la aplicación con AES-256-GCM.
3. **Soportes en MinIO.** El ADR-009 decide guardar los documentos de justificación en MinIO, un almacenamiento de objetos compatible con S3 y autoalojado, con cifrado del lado del servidor [9]. La subida de soportes a MinIO está en desarrollo.

En paralelo se eliminan las cuentas semilla de demostración y se endurece el despliegue. Por ejemplo, el ADR-020 registra el puerto de administración de Kong abierto en EC2 como pendiente de endurecimiento [9].

# Resultados

## Funcionalidad entregada

El MVP del horizonte 1 tiene validados los contextos Identity, Academic, Scheduling y el núcleo de Attendance, además del enrolamiento facial, el flujo de justificaciones y las notificaciones dentro de la aplicación [7]. El enrolamiento y la verificación de huella dactilar en el cliente web se adelantaron a la hoja de ruta, que los ubicaba en el horizonte 3 [8].

## Pruebas según IEEE 829

El reporte resumido IEEE 829 del 21 de septiembre de 2026 cubre ocho servicios del back-end [12]. En ese momento notificaciones estaba en Go, y `ms-quality` no hacía parte de la suite.

| Tipo | Ejecutadas | Pasadas | Fallidas | Éxito |
|---|---:|---:|---:|---:|
| Unitarias | 3 | 3 | 0 | 100 % |
| CRUD | 55 | 53 | 2 | 96,4 % |
| API | 23 | 22 | 1 | 95,7 % |
| **Total** | **81** | **78** | **3** | **96,3 %** |

Fuente: [12].

La cobertura fue de 72 % en código (meta ≥ 70 %), 68 % en ramas (meta ≥ 65 %), 100 % en operaciones CRUD y 95 % en validación (meta ≥ 90 %) [12]. Dos servicios, `ms-scheduling` y `ms-configuration`, quedaron en estado pendiente [12]. Los tiempos de respuesta P95 de los cinco endpoints medidos estuvieron entre 30 y 85 ms, y el P99 más alto fue de 150 ms, todos por debajo de la meta de 200 ms [12]. En concurrencia, 50 lecturas y 20 escrituras simultáneas tuvieron 100 % de éxito, y una carga mixta de 100 peticiones tuvo 99 % [12]. Las pruebas de inyección SQL, XSS, CSRF y evasión de autenticación no encontraron vulnerabilidades [12]. Se registraron dos incidencias críticas, ya cerradas (duplicados en la creación masiva de asistencia y error 500 sin base de datos en notificaciones), y dos menores abiertas [12].

La documentación de pruebas unitarias del 10 de octubre de 2026 cubre los servicios de identidad, autorización, programación, asistencia y biometría. Reporta 88 % de cobertura de líneas y 82 % de ramas, y las cinco pruebas trazadas a requisitos pasan [15]. Esta cifra no reemplaza la del reporte IEEE 829: es posterior y mide solo pruebas unitarias de cinco servicios.

Para las pruebas de extremo a extremo, el equipo carga un volumen de datos con 2 sedes, 1.600 aprendices, 80 instructores, unas 1.210 sesiones y unos 81.000 registros de asistencia [11].

## Calidad según ISO/IEC 25010

La primera aplicación del instrumento encontró ocho hallazgos, cuatro de severidad alta y cuatro media. Entre ellos había códigos HTTP incorrectos, falta de validación de entrada, errores 500 que exponían la traza de pila, listados sin paginación y formatos de error distintos entre servicios [13]. Antes de corregirlos, el equipo estimó un promedio de 2,6/5 [13]. Después de las correcciones, la evaluación del CRUD de ciudades de `ms-identity` obtuvo 4,0/5 (80 %), nivel "Bueno" [13]. Fue una sola evaluación de un solo servicio, hecha por el mismo equipo.

# Discusión

Los resultados muestran que la arquitectura cumple los objetivos técnicos que se pueden medir sin usuarios: el porcentaje de pruebas aprobadas y los tiempos de respuesta superan las metas del plan IEEE 829, y el instrumento ISO/IEC 25010 sirvió para encontrar y corregir defectos concretos. Pero la evidencia tiene cuatro límites.

1. **No hay piloto todavía.** Las metas de producto, como 80 % de asistencia capturada automáticamente, 10 segundos o menos por registro facial y 85 % o más de éxito facial con buena luz, siguen siendo hipótesis. Las entrevistas a 5 instructores y 30 aprendices y la observación de 3 sesiones estaban planeadas para octubre de 2026 [8]. Las líneas base también son estimaciones por validar, como los 45 segundos por aprendiz en el registro manual y los 30 minutos para consolidar el reporte de una ficha [8].
2. **Las cifras de calidad vienen de mediciones distintas.** El reporte IEEE 829 es de septiembre y describe el servicio de notificaciones en Go. La cobertura de 88 % es de octubre y solo incluye pruebas unitarias. La evaluación ISO/IEC 25010 cubre un solo CRUD. Ninguna cifra describe el sistema completo en su versión actual.
3. **Hay deuda técnica documentada.** Faltan el patrón outbox y las colas de mensajes fallidos (AT-006), la observabilidad es parcial (hay logs estructurados, pero no Prometheus, Grafana ni Jaeger) y cada stack mantiene su propio guardián de sesión, lo que puede hacer que las implementaciones diverjan [9].
4. **La privacidad está incompleta.** Mientras el consentimiento y el cifrado de campos no estén en producción, el sistema no cumple todavía el principio de privacidad desde el diseño que se propuso [8], [9].

Algunas decisiones cambiaron durante la implementación. El ADR-002 proponía un monolito modular y el sistema corre como nueve contenedores, aunque mantiene los límites de datos por schema. El ADR-011 proponía Jenkins y se reemplazó por GitHub Actions. El ADR-013 descartaba Expo y se reemplazó por el ADR-019 [9]. Que cada cambio quede registrado como una nota de implementación o un ADR nuevo hace auditable la diferencia entre lo planeado y lo construido.

# Conclusiones

1. FaceAttend EDU resuelve el problema planteado con una plataforma web y móvil que registra la asistencia por reconocimiento facial, ofrece alternativas manual y por importación y centraliza historiales, justificaciones, reportes y alertas.
2. La división en nueve servicios hexagonales con un schema por contexto mantuvo aislados los modelos de cada dominio y permitió que cada servicio usara el stack más adecuado, como Python para la visión por computador.
3. La sesión opaca validada en Kong permite revocar el acceso desde el servidor: Kong invalida su caché al cerrar sesión y la caché de cada servicio dura 10 segundos. Un JWT sin estado no permite esto sin infraestructura adicional.
4. Las pruebas registradas superan las metas del plan IEEE 829 en éxito, cobertura y tiempos de respuesta, y el instrumento ISO/IEC 25010 sirvió para mejorar la calidad de forma medible.
5. Antes de un despliegue con datos reales falta cerrar el consentimiento, el cifrado AES-256-GCM de biometría y datos personales y el almacenamiento de soportes en MinIO, y validar las metas de producto en un piloto con una ficha real.

# Referencias

[1] Congreso de la República de Colombia. (2012). *Ley Estatutaria 1581 de 2012, por la cual se dictan disposiciones generales para la protección de datos personales*. Diario Oficial 48.587.

[2] International Organization for Standardization. (2011). *ISO/IEC 25010:2011. Systems and software engineering — Systems and software Quality Requirements and Evaluation (SQuaRE) — System and software quality models*.

[3] IEEE. (1998). *IEEE Std 829-1998. IEEE Standard for Software Test Documentation*.

[4] Schwaber, K., & Sutherland, J. (2020). *The Scrum Guide*. https://scrumguides.org

[5] Cockburn, A. (2005). *Hexagonal architecture*. https://alistair.cockburn.us/hexagonal-architecture/

[6] Beck, K. (2002). *Test-Driven Development: By Example*. Addison-Wesley.

[7] Equipo FaceAttend EDU. (2026). *fae-docs: `01-context/overview.md` y `01-context/scope.md`*. Rama `main`.

[8] Equipo FaceAttend EDU. (2026). *fae-docs: `03-product/problem-framing.md` y `03-product/vision.md`*. Rama `main`.

[9] Equipo FaceAttend EDU. (2026). *fae-docs: `05-architecture/overview.md` y registros ADR-001 a ADR-020*. Rama `main`.

[10] Equipo FaceAttend EDU. (2026). *fae-docs: `00-governance/agile-conventions.md`*. Rama `main`.

[11] Equipo FaceAttend EDU. (2026). *fae-docs: `11-quality/README.md`, `testing-strategy.md` y `tdd-guide.md`*. Rama `main`.

[12] Equipo FaceAttend EDU. (2026, 21 de septiembre). *IEEE 829 — Reporte resumido de prueba*. ProyectoFaceAttendEDU, `back-end/IEEE829_TEST_SUMMARY_REPORT.md`, rama `develop`.

[13] Equipo FaceAttend EDU. (2026). *Informe de calidad ISO/IEC 25010*. ProyectoFaceAttendEDU, `back-end/ISO25010_QUALITY_REPORT.md`, rama `develop`.

[14] Equipo FaceAttend EDU. (2026, 21 de septiembre). *Reporte de implementación del instrumento ISO 9001:2015*. ProyectoFaceAttendEDU, `back-end/QUALITY_REPORT.md`, rama `develop`.

[15] Equipo FaceAttend EDU. (2026, 10 de octubre). *Documentación de pruebas unitarias — Back-end FaceAttendEDU*. FaceAttendEDU-RAPS, `06-Documentación de pruebas/es/Documentacion-pruebas.md`.

[16] Equipo FaceAttend EDU. (2025). *Informe técnico* y *Plan de actividades*. FaceAttendEDU-RAPS, `03-Documentación de propuesta técnica del software/es/`.

[17] Equipo FaceAttend EDU. (2026, 22 de octubre). *Código fuente de ProyectoFaceAttendEDU*, rama `develop`, commit `2f8bf2d`: `back-end/08-ms-notification/STACK.md`, `back-end/06-ms-biometric/infrastructure/config/settings.py`, `back-end/01-ms-identity/.../SecurityConfig.java` y `AuthProperties.java`, `back-end/05-ms-attendance/pom.xml`, `front-end/*/package.json`.
