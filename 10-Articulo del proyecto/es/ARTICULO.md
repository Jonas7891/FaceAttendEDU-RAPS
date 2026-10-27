# FaceAttend EDU: diseño, implementación y revisión de una plataforma de asistencia facial basada en microservicios

**[Jonattan Steven Rizo Solano¹], [Diego Andres Gutierrez Nuñez²], [Juan David Arboleda Perdomo³]**

*Nota para los autores: nombres, afiliaciones y correos son marcadores. Completarlos antes de enviar.*

---

**Abstract**—Recording attendance in technical and higher education is still largely manual. It is slow, and it is easy to falsify with signatures or shared cards. This paper describes FaceAttend EDU, a platform that records attendance with facial recognition and a liveness check. The system has nine backend services behind an API gateway, a React web client, and an Expo mobile client. We describe the architecture, the relational model shared by the services, the biometric subsystem, and the security review we ran before any pilot. We do not report accuracy, latency, or cost figures, because the system has not been measured under field conditions. We report instead what can be checked in the repository: 1 138 commits across all branches between 26 March and 27 October 2026, a 37-table data model, and 2 317 automated tests in the web, mobile, TypeScript, and Java suites. The review found one critical and one serious access-control defect, both open when this text was written. We use the case to discuss three problems that were harder in practice than expected: business rules duplicated across three languages, different route normalization in the gateway and in the services, and documentation that drifted away from the code.

**Index Terms**—Facial recognition, liveness detection, microservices, attendance management, software architecture, security review, higher education.

**Resumen**—El registro de asistencia en la educación técnica y superior sigue siendo mayoritariamente manual, lento y fácil de falsificar. Este artículo describe FaceAttend EDU, una plataforma que registra la asistencia mediante reconocimiento facial con verificación de vida. El sistema consta de nueve servicios de backend detrás de un API gateway, un cliente web en React y un cliente móvil en Expo. Describimos la arquitectura, el modelo relacional compartido, el subsistema biométrico y la revisión de seguridad hecha antes de cualquier piloto. No reportamos cifras de precisión, latencia ni costo, porque el sistema no se ha medido en condiciones reales de uso. Reportamos en cambio lo que puede verificarse en el repositorio, y discutimos tres problemas que resultaron más difíciles de lo previsto: reglas de negocio duplicadas en tres lenguajes, normalización de rutas distinta entre el gateway y los servicios, y documentación que se separó del código.

---

## I. Introducción

En una sesión de clase, el registro de asistencia en papel o en un formulario digital consume tiempo que sale de la clase. El registro manual además admite errores de transcripción y firmas de otra persona, y suele revisarse cuando ya no hay forma sencilla de corregirlo.

FaceAttend EDU nació para atender ese problema en un contexto concreto: un centro de formación técnica con varias sedes, programas organizados por cohortes, instructores que abren y cierran sesiones, y aprendices que se registran frente a una cámara. Quisimos que la asistencia facial fuera una opción dentro de un sistema académico completo y no un módulo aislado. Eso obligó a resolver identidad, roles por sede, horarios, justificaciones y alertas.

Este artículo no describe un sistema validado en producción. Describe el diseño que adoptamos, las decisiones que tuvieron costo y los resultados que podemos respaldar con el código y con las pruebas automatizadas. Aportamos tres cosas:

1. Una descripción de la arquitectura de nueve servicios y de la asignación de lenguajes, con sus limitaciones.
2. Un modelo relacional de 37 tablas repartido en contextos de negocio, con reglas de unicidad que la base de datos hace cumplir y no solo la aplicación.
3. Una revisión de seguridad previa al piloto, con hallazgos concretos, algunos corregidos en una rama local y otros abiertos.

La sección II ubica el trabajo frente a la literatura. Las secciones III a V describen la arquitectura, el modelo de datos y el subsistema biométrico. La sección VI resume la revisión de seguridad, la VII reporta la verificación automatizada, la VIII discute límites y la IX concluye. El Apéndice indica cómo reproducir las verificaciones.

## II. Trabajos relacionados

No hicimos una revisión sistemática de la literatura. Esta sección presenta los trabajos que usamos como base técnica y los que contrastamos con nuestras decisiones.

**Reconocimiento facial.** FaceNet [1] aprende un embedding euclidiano de 128 dimensiones con pérdida de tripletas. ArcFace [2] añade un margen angular a la pérdida softmax y se usa con frecuencia como línea base en verificación. Nuestro servicio biométrico calcula embeddings de 128 dimensiones con la biblioteca `face_recognition` [4], que envuelve los modelos de dlib [3]. Las cifras de benchmark de FaceNet y ArcFace se obtienen en conjuntos públicos y no transfieren a un aula, así que no las tomamos como evidencia de desempeño de nuestro sistema.

**Microservicios.** Richardson [5] y Newman [6] describen la descomposición por contexto de negocio, la comunicación asíncrona y la consistencia eventual como patrones recurrentes. Varios de esos patrones, como el circuit breaker o la saga, aparecen con frecuencia en la literatura y en presentaciones de la industria. En nuestro código no encontramos un circuit breaker implementado, así que no lo describimos como parte del sistema.

**Calidad y prácticas de desarrollo.** Martin [7] sostiene que el código limpio reduce el costo de mantenimiento a largo plazo. Piñero González et al. [8] tratan los riesgos de eficiencia en productos de software desde las primeras fases del desarrollo. Ambas referencias orientaron nuestra disciplina de pruebas y la decisión de medir antes de optimizar, aunque en esta versión del trabajo no medimos desempeño de forma sistemática.

## III. Arquitectura del sistema

### A. Servicios y lenguajes

El backend tiene nueve servicios y un gateway. Cada servicio es dueño de su esquema de datos y expone una API REST bajo `/api/v1`. La Tabla I resume la asignación.

**Tabla I. Servicios y tecnologías.**

| Servicio | Responsabilidad | Tecnología |
|---|---|---|
| 01 identity | Personas, usuarios, sesiones, segundo factor, recuperación | Java, Spring Boot 4.1.x |
| 02 authorization | Roles, permisos, evaluación RBAC | Java, Spring Boot 4.1.x |
| 03 academic | Sedes, programas, cohortes, matrículas | TypeScript, Fastify 5, Drizzle |
| 04 scheduling | Bloques horarios, sesiones de clase | Java, Spring Boot 4.1.x |
| 05 attendance | Registros, justificaciones, reportes | Java, Spring Boot 4.1.x |
| 06 biometric | Enrolamiento, verificación, prueba de vida | Python 3.12, FastAPI |
| 07 configuration | Parámetros por sede y de seguridad | TypeScript, Fastify 5 |
| 08 notification | Alertas y notificaciones | TypeScript, Fastify 5 |
| 09 quality | Seguimiento de calidad del proyecto | TypeScript, Fastify 5 |
| 99 api-gateway | Entrada única, enrutamiento, límites | Kong 3.6 [9] |

Los clientes son una aplicación web en React 19 y una aplicación móvil en Expo SDK 57. Ambos se comunican solo con el gateway.

Usamos varios lenguajes, y eso tiene un costo que retomamos en la sección VIII. Los servicios Java se apoyan en Spring Security y en migraciones de Liquibase. Los servicios TypeScript usan Fastify [10]. El servicio Python usa FastAPI [11] y es el único que depende del ecosistema de visión por computador (OpenCV, dlib y NumPy). La asignación de lenguajes responde a decisiones del equipo; el repositorio no la justifica por escrito más allá de lo que se ve en el código.

### B. Comunicación y persistencia

Cada servicio tiene su propio esquema en PostgreSQL 17 [12]. Las claves foráneas no cruzan esquemas: cuando un registro de asistencia referencia a un actor académico, la columna guarda el identificador y la aplicación valida que exista. La documentación del proyecto fija esta regla para que los servicios puedan migrar por separado. El precio es que varias validaciones quedan fuera del alcance de la base de datos.

Kafka [13] publica eventos de dominio en los servicios Java y en Academic. MongoDB guarda las plantillas biométricas, porque cada registro contiene un vector y la versión del modelo que lo produjo. Redis guarda los contadores de intentos fallidos de inicio de sesión en Identity. Los archivos de soporte de las justificaciones se almacenan en MinIO, un servicio compatible con S3 que se levanta con Docker Compose.

### C. Organización interna de un servicio

El servicio de identidad sigue una arquitectura hexagonal, y la estructura del código la refleja. El paquete `domain/` contiene los modelos (`Person`, `User`, `UserSession`), los eventos de dominio, las excepciones y los servicios de dominio. El paquete `application/` contiene 34 implementaciones de casos de uso y los puertos de entrada y de salida. El paquete `adapter/` contiene los adaptadores: entrada web y mensajería; salida hacia persistencia JPA, seguridad, Kafka, notificaciones, aprovisionamiento y clientes HTTP.

La regla de dependencia se cumple en el dominio: ningún archivo de `domain/` importa `application` ni `adapter`. El dominio declara 21 eventos, entre ellos `PersonCreated`, `UserAuthenticated` y `UserLocked`.

### D. Decisiones con costo

Tres decisiones tuvieron costos visibles en el repositorio.

La primera fue repartir las reglas de alcance por sede en cada servicio. Como esas reglas se escribieron en Java, TypeScript y Python, una corrección en un lenguaje no llegaba a los otros. La sección VI describe un caso.

La segunda fue usar un gateway declarativo. Kong decide qué rutas son públicas y cuáles exigen sesión, y cada servicio vuelve a aplicar sus propias reglas. La doble verificación sirve, pero solo funciona si ambas capas interpretan la misma ruta. Cuando no la interpretan igual aparece una brecha.

La tercera fue declarar un archivo DBML como fuente de verdad del modelo. Eso obliga a sincronizar la documentación derivada cada vez que cambia el modelo, y es una carga que el equipo subestimó. La sección IV muestra un caso concreto de desfase.

## IV. Modelo de datos y reglas de negocio

El modelo relacional tiene 37 tablas agrupadas en contextos: identidad, autorización, académico, programación, asistencia, biométrico, configuración, notificación y calidad. Las entidades centrales son `person`, `app_user` y `user_session` en identidad; `role`, `permission` y `user_role` en autorización; `cohort`, `enrollment` y `academic_actor` en lo académico; `schedule_block` y `class_session` en programación; y `attendance_record` y `justification` en asistencia.

La persona y la cuenta de usuario están separadas. Una persona puede tener registros de asistencia sin tener cuenta, y una cuenta siempre apunta a una persona. Así, la identidad no depende de la sede en la que una persona estudia o enseña. La persona guarda nombre, apellido, documento, correo, teléfono, tipo y grupo sanguíneo, fecha de nacimiento, dirección y estado, con las columnas de auditoría. La cuenta guarda el nombre de usuario, el hash de la contraseña, el tipo de autenticación (`Local`, `Windows` o `External`) y las marcas de cambio obligatorio de contraseña y de segundo factor. El hash usa BCrypt con costo 12.

Los permisos siguen el formato `contexto.recurso:acción`. Las semillas actuales definen cuatro roles (Super Administrador, Administrador, Instructor y Aprendiz) y 13 permisos. La documentación de datos del proyecto describe otra cosa: cuatro roles con los nombres `SUPER_ADMIN`, `SCHOOL_ADMIN`, `INSTRUCTOR` y `STUDENT`, y 12 permisos. El código cambió y la documentación no lo siguió. Es el mismo problema que describimos en la sección III.D.

En asistencia, los estados válidos son `Present`, `Absent`, `Late` y `Justified`, y los métodos de captura son `FACIAL`, `MANUAL`, `FINGERPRINT` e `IMPORT`. La tasa de asistencia cuenta como asistencia los estados `Present` y `Late`. En el código de asistencia no encontramos una regla automática de tardanza: el estado `Late` se registra desde la captura.

Varias reglas se expresan como restricciones de base de datos y no solo en código:

- **Un registro de asistencia por actor y sesión.** La restricción única sobre `(class_session_id, academic_actor_id)` impide duplicados aunque dos solicitudes lleguen casi al mismo tiempo. Los servicios convierten el error de base de datos en una respuesta de conflicto.
- **Una justificación por registro.** Solo se justifican registros en estado `Absent` o `Late`, y la restricción única sobre `attendance_record_id` impide una segunda justificación para el mismo registro.
- **Bloques horarios sin traslape.** La base de datos impone unicidad sobre la combinación de aula, día y hora de inicio, y sobre la de instructor, día y hora de inicio. Esa restricción solo atrapa horas idénticas. Por eso el servicio de programación valida además que los intervalos no se crucen en el mismo día, tanto para el aula como para el instructor. Los bloques que solo se tocan en un extremo son válidos.
- **Una sesión por bloque y fecha.** El estado de la sesión pasa de `Open` a `Closed` o `Cancelled`, y el cierre registra quién lo hizo y cuándo.

Las restricciones de existencia entre servicios no pueden expresarse en la base de datos y se validan en la aplicación. Es la parte más frágil del diseño, y la primera que revisaríamos en una segunda versión.

## V. Reconocimiento facial y verificación de vida

El flujo tiene dos fases. En el enrolamiento, el servicio biométrico recibe una imagen, verifica que contenga exactamente un rostro, calcula el vector de 128 dimensiones y lo guarda en MongoDB junto con la versión del modelo. Si la imagen no tiene rostro, el servicio lo informa; si tiene más de uno, rechaza la solicitud. En la verificación, calcula el vector de la imagen capturada y lo compara con la plantilla activa de la persona.

La comparación facial usa distancia euclidiana, no similitud coseno. Según el comentario de configuración del propio servicio, los vectores de dlib no están centrados y la similitud coseno entre personas distintas supera 0,85 con frecuencia; por eso el servicio usa la métrica para la que se entrenan los modelos. La puntuación es `1 − distancia`. El valor por omisión exige una distancia máxima de 0,5 (`face_match_max_distance`). Cada sede puede elegir su nivel de confianza en un control deslizante de 50 % a 100 %. Esa elección se convierte en una distancia máxima entre 0,60 (50 %) y 0,35 (100 %). Así, una sede puede ser más estricta que el valor por omisión, pero no puede aflojar el criterio más allá de la tolerancia de 0,6 que la biblioteca `face_recognition` usa por omisión [4]. El umbral coseno de 0,85 de la configuración no se aplica a los rostros: se usa en la verificación de vectores de huella.

La verificación de vida pide acciones de una lista cerrada: parpadear, girar a la izquierda, girar a la derecha, sonreír e inclinar la cabeza. El servicio conserva acciones heredadas para clientes antiguos. Cada desafío expira a los 120 segundos y se firma con un secreto de configuración. Una acción respondida en menos de un segundo se considera no realizada. Durante el desafío, el servicio compara cada paso con la referencia de identidad del propio desafío mediante similitud coseno, con umbral 0,75, para comprobar que pertenecen a la misma persona. Ese umbral es más laxo que el de comparación facial porque los cuadros tomados durante un gesto cambian el vector. Esta prueba no sustituye una evaluación de ataques de presentación con un protocolo independiente, y lo reconocemos en la sección VIII.

No reportamos tasas de falsos positivos ni de falsos negativos. El repositorio no incluye una medición de ese tipo, y preferimos no publicar una cifra que no pudiéramos reproducir.

## VI. Revisión de seguridad

Antes de cualquier piloto, revisamos el código leyendo los filtros de seguridad, los routers, la configuración de Kong y Docker Compose. También ejecutamos las suites de pruebas y `npm audit` sobre los proyectos de producción.

**Hallazgos corregidos en una rama local** (sin fusionar a la fecha de la revisión):

- **Bypass por codificación de ruta en los servicios Node.** El guard de autenticación decidía si una ruta era pública usando la URL sin decodificar, pero Fastify sí decodifica al enrutar. Una petición a `/%61pi/v1/configurations/...` llegaba al handler protegido y el guard la trataba como pública. Kong no era explotable por esta vía: se probó con `kong:3.6` y reenvía la ruta ya normalizada. Los servicios sí lo eran cuando se les llamaba por su puerto directamente. La corrección decodifica y normaliza antes de comparar, responde 400 a rutas ambiguas y añade cuatro pruebas. Con Fastify real, la petición pasa de 200 a 401.
- **Endpoints de autorización públicos.** `GET /api/v1/auth/evaluate` y `GET /api/v1/users/{id}/roles` estaban marcados como `permitAll` y Kong los enrutaba. La corrección exige sesión y, para consultar a otro usuario, el permiso `configuration:manage`.
- **Wrapper de Maven roto en un clon limpio.** El `.gitignore` ignoraba `/.mvn/`, así que `mvnw` se versionaba pero `maven-wrapper.properties` no. Un desarrollador sin Maven instalado no podía compilar. La corrección ajusta el `.gitignore` y versiona el archivo.

**Hallazgos abiertos:**

- Un defecto crítico de control de acceso: un administrador de sede puede obtener una sesión válida de cualquier usuario, incluido el superadministrador, sin contraseña ni segundo factor.
- Un defecto grave de control de acceso: los instructores de una sede pueden leer y borrar plantillas biométricas de personas de otras sedes.
- El mismo bypass de codificación de ruta del guard de Node existe en el filtro de autenticación de Java, que usa la ruta sin decodificar. La corrección local no lo incluye.
- Un WebSocket de asistencia difunde eventos a todas las sedes, no solo a la del usuario conectado.
- Tokens de sesión en `sessionStorage` en el cliente web.
- Límites de tasa con valores de desarrollo (`minute: 6000`) en casi todas las rutas de Kong.
- Swagger y Actuator accesibles sin autenticación en los cuatro servicios Java.
- `npm audit` reporta 51 vulnerabilidades en dependencias de producción, 34 de ellas altas. La librería `xlsx` 0.18.5 no tiene parche en npm.
- Archivos de respaldo con secretos (`.env.bak`, `.env.prod.bak`, `.env.prod.bak2`) en el directorio de trabajo, y un `.env` de desarrollo en el historial de git. El archivo `.env.prod` contiene el secreto del desafío de vida. No está versionado en git, pero es un riesgo de manejo de secretos que hay que cerrar.

Ninguno de estos hallazgos está resuelto en la rama principal. Los documentamos porque un sistema que manejará datos biométricos de menores debe registrar sus fallos tanto como sus logros. El control de acceso y la validación de entrada son las áreas que recomendamos auditar primero en cualquier API similar [14].

## VII. Verificación automatizada

La Tabla II resume las pruebas ejecutadas durante la revisión del 27 de octubre de 2026. Son pruebas unitarias y de integración, no pruebas de extremo a extremo de historias de usuario.

**Tabla II. Pruebas automatizadas ejecutadas.**

| Componente | Suites | Pruebas | Resultado |
|---|---|---|---|
| Cliente web (React) | 56 | 463 | todas pasan |
| Cliente móvil (Expo) | 34 | 586 | todas pasan |
| Servicios TypeScript (03, 07, 08, 09) | — | 540 | todas pasan, tras `npm ci` |
| Servicios Java (compartido: 11; 01: 285; 02: 134; 04: 121; 05: 177) | — | 728 | todas pasan |
| Servicio biométrico (Python) | — | no ejecutado | sin entorno en la máquina de revisión |

Las 2 317 pruebas ejecutadas no dicen si el sistema funciona con aprendices reales. No miden desempeño ni precisión facial, y no hay pruebas de carga. El directorio de pruebas de extremo a extremo tiene 191 líneas y cubre pocos flujos.

En la autoevaluación interna, con una escala de 1 (inicial) a 5 (listo para producción), el proyecto obtiene 2,5. La funcionalidad del alcance MVP se evalúa en 3,5. Cumplimiento legal (1,5) y seguridad (2) son las dimensiones más bajas. Esta calificación se basa en la lectura del código y de los requisitos, no en una evaluación con usuarios.

Dos observaciones de la revisión nos parecen útiles para quien intente repetir el trabajo. La primera: el wrapper de Maven no funcionaba en un clon limpio, mientras que la suite Java sí pasaba en un contenedor con Maven. Una prueba puede depender de un entorno que nadie documentó. La segunda: el bypass de la sección VI se hizo visible al leer el código y al probar una ruta codificada; las pruebas existentes no cubrían ese caso.

## VIII. Discusión y limitaciones

**Lo que costó.** Buena parte del esfuerzo no estuvo en el reconocimiento facial sino en las costuras entre servicios. Las reglas de alcance por sede se duplicaron en tres lenguajes. Según el informe interno de la revisión, esa duplicación explica fallos como el de acceso entre sedes. Si el proyecto se rehiciera, centralizaríamos esas reglas en una librería compartida desde el inicio.

La normalización de rutas es el segundo caso. Dos componentes que interpretan la misma URL de forma distinta crean una superficie de ataque que ninguno ve por sí solo. En la rama local se corrigió el guard de Node; el filtro de Java tiene el mismo patrón y sigue abierto. La lección es general: la autorización debe evaluarse sobre la ruta ya enrutada, no sobre la cadena que llegó por la red.

**Lo que no sabemos.** No sabemos qué tan precisa es la verificación en un aula real, con iluminación variable, gafas o cámaras de baja resolución. Tampoco sabemos cuánto tiempo ahorra el registro frente al método anterior, porque no hicimos mediciones comparativas. Un piloto en una sede, con un procedimiento de medición definido antes de empezar, sería el paso natural.

**Límites del estudio.** Quienes escribimos este trabajo también desarrollamos el sistema y hicimos su revisión de seguridad. Eso introduce sesgo. Mitigamos el problema registrando cada hallazgo con evidencia de código, pero no reemplaza una auditoría externa.

**Cumplimiento legal.** Los datos biométricos son datos sensibles según el artículo 5 de la Ley 1581 de 2012 [15]. La misma ley restringe el tratamiento de datos de menores de edad (artículo 7), y el Decreto 1377 de 2013 regula ese tratamiento [16]. A la fecha de la revisión, el flujo de autorización del representante no estaba implementado, y tampoco la política de tratamiento ni la evaluación de impacto. Son requisitos previos a cualquier uso con datos reales.

**Licencias y versiones.** Liquibase 5.x se distribuye bajo una licencia FSL, que exige una decisión explícita antes de adoptar esa versión. El SDK de huellas que usa el cliente web no incluía sus avisos de licencia en la revisión. La imagen de Kafka en Docker Compose es la 3.8.0, que según fuentes secundarias ya no recibe soporte de la comunidad; conviene planear su actualización.

## IX. Conclusiones y trabajo futuro

FaceAttend EDU es, en su mayor parte, un sistema funcional. Tiene una arquitectura de nueve servicios, un modelo de datos de 37 tablas y 2 317 pruebas automatizadas que pasan. No está listo para datos reales. La revisión de seguridad encontró fallos de control de acceso que deben corregirse antes de un piloto, y el cumplimiento legal va por detrás del código.

Proponemos cuatro pasos, en este orden:

1. Corregir y fusionar los hallazgos de seguridad, empezando por el control de acceso entre sedes, con una prueba de regresión para cada uno.
2. Centralizar las reglas de alcance por sede en una librería compartida, con pruebas generadas desde una sola fuente.
3. Montar un entorno de preproducción y medir falsos positivos y falsos negativos con un conjunto de datos y consentimientos válidos.
4. Ejecutar un piloto en una sede, midiendo el tiempo de registro y el error antes y después con un diseño comparable.

## Apéndice. Datos y reproducción

**Código.** Repositorio `andresguti17/ProyectoFaceAttendEDU` (GitHub). Las cifras de la Tabla II provienen de la revisión del 27 de octubre de 2026. No registramos con certeza el commit exacto de esa ejecución. El commit `478785d1` existe en el repositorio y fue el que se consultó al cierre de la revisión, pero la rama `tmp/integration` ya no existe localmente, y la rama activa cambió después (`develop`, commit `bb5f0fb7` al momento de esta verificación). Antes de enviar, conviene repetir las pruebas sobre el commit que se cite.

**Reproducción de pruebas (orientativo):**

- Cliente web: `npm ci` y luego `npm test` en `front-end/Web`.
- Cliente móvil: `npm ci` y luego `npm test` en `front-end/Mobile`.
- Servicios TypeScript: `npm ci` y luego `npm test` en `back-end/03-ms-academic`, `07-ms-configuration`, `08-ms-notification` y `09-ms-quality`.
- Servicios Java: `./mvnw test` en cada servicio Java; si el wrapper no está presente, `mvn test` en una imagen con Maven y JDK 21.
- Servicio biométrico: `poetry install` y `pytest` en `back-end/06-ms-biometric`. No se ejecutó durante la revisión.

**Datos.** El artículo no usa datos de aprendices. Los identificadores de ejemplo de la documentación son ficticios.

---

## Agradecimientos

[Completar: personas, instituciones y financiación, si aplica.]

## Referencias

[1] F. Schroff, D. Kalenichenko y J. Philbin, "FaceNet: A unified embedding for face recognition and clustering," en *Proc. IEEE Conf. Computer Vision and Pattern Recognition (CVPR)*, 2015, pp. 815–823, doi: 10.1109/CVPR.2015.7298682.

[2] J. Deng, J. Guo, N. Xue y S. Zafeiriou, "ArcFace: Additive angular margin loss for deep face recognition," en *Proc. IEEE/CVF Conf. Computer Vision and Pattern Recognition (CVPR)*, 2019, pp. 4690–4699.

[3] D. E. King, "Dlib-ml: A machine learning toolkit," *J. Mach. Learn. Res.*, vol. 10, pp. 1755–1758, 2009. [En línea]. Disponible: https://www.jmlr.org/beta/papers/v10/king09a.html

[4] Documentación de `face_recognition`, función `compare_faces` (tolerancia por omisión 0,6). [En línea]. Disponible: https://face-recognition.readthedocs.io/en/stable/face_recognition.html

[5] C. Richardson, *Microservices Patterns: With Examples in Java*. Shelter Island, NY, EE. UU.: Manning, 2018.

[6] S. Newman, *Building Microservices: Designing Fine-Grained Systems*, 2.ª ed. Sebastopol, CA, EE. UU.: O'Reilly Media, 2021.

[7] R. C. Martin, *Clean Code: A Handbook of Agile Software Craftsmanship*. Upper Saddle River, NJ, EE. UU.: Prentice Hall, 2008.

[8] M. Piñero González, A. Marín Díaz, Y. Trujillo Casañola y D. Buedo Hidalgo, "Buenas prácticas para prevenir los riesgos de la eficiencia del desempeño en los productos de software," *Rev. Cubana de Ciencias Informáticas*, vol. 15, no. 1, pp. 89–113, 2021.

[9] Kong Inc., *Kong Gateway Documentation*, versión 3.6. [En línea]. Disponible: https://docs.konghq.com/

[10] Fastify, *Fastify Documentation*, versión 5. [En línea]. Disponible: https://fastify.dev/docs/latest/

[11] FastAPI, *FastAPI Documentation*, versión 0.162. [En línea]. Disponible: https://fastapi.tiangolo.com/

[12] PostgreSQL Global Development Group, *PostgreSQL 17 Documentation*. [En línea]. Disponible: https://www.postgresql.org/docs/17/

[13] Apache Software Foundation, *Apache Kafka Documentation*, versión 3.8. [En línea]. Disponible: https://kafka.apache.org/38/apis/

[14] OWASP Foundation, *OWASP API Security Top 10 — 2023*. [En línea]. Disponible: https://owasp.org/API-Security/editions/2023/en/0x11-t10/

[15] Congreso de Colombia, *Ley Estatutaria 1581 de 2012, por la cual se dictan disposiciones generales para la protección de datos personales*, Diario Oficial No. 48.587, Bogotá, Colombia, oct. 2012. [En línea]. Disponible: https://www.unidadvictimas.gov.co/wp-content/uploads/Documentos/ley/Ley_1581_2012.pdf

[16] Presidencia de la República de Colombia, *Decreto 1377 de 2013, por el cual se reglamenta parcialmente la Ley 1581 de 2012*. [En línea]. Disponible: https://www.unidadvictimas.gov.co/wp-content/uploads/Documentos/decretos/DECRETO_1377_2013.pdf
