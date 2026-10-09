---
title: "Informe de Especificación de Requisitos"
authors: 
abstract: |
  El presente informe especifica los requisitos funcionales del sistema de gestión de usuarios, ambientes académicos, control de asistencia, configuraciones móviles, históricos, justificaciones, reportes avanzados y gestión del colegio. Documenta cada requerimiento con propósito, plataforma, entradas, reglas de negocio, criterios de aceptación, consideraciones de seguridad, casos borde y mecanismos de verificación. Su objetivo servir como base contractual, técnica y de auditoría para desarrollo, pruebas, despliegue y mantenimiento del software.
keywords:
  - especificación de requisitos
  - ingeniería de requisitos
  - software educativo
  - control de asistencia
  - biometría
  - IoT
  - RBAC
  - movilidad
---

# Identificación del documento

| Campo | Descripción |
|---|---|
| Código | IER-SW-001 |
| Título | Informe de Especificación de Requisitos |
| Versión | 1.0 |
| Estado | Borrador para revisión técnica |
| Clasificación | Confidencial-interno |
| Propietario | Dirección de Ingeniería de Software |
| Aprobador | Comité de Producto y Calidad |
| Fecha de emisión | 10 de octubre de 2026 |
| Próxima revisión programada | 10 de abril de 2027 |
| Naturaleza del documento | Especificación funcional y de validación de requisitos |

# Introducción

El sistema objeto de este informe soporta la gestión integral de usuarios, ambientes o salones, fichas o cursos, registros de entrada y salida, configuraciones móviles, históricos de asistencia, justificaciones, reportes analíticos y administración del catálogo académico del colegio. Los requerimientos funcionales provienen de la lista entregada por el área de producto y se desglosan aquí en especificaciones verificables, alineadas con buenas prácticas de ingeniería de requisitos (International Organization for Standardization, International Electrotechnical Commission, & Institute of Electrical and Electronics Engineers [ISO, IEC, & IEEE], 2018).

Desde la perspectiva de calidad de software, cada requerimiento debe ser inequívoco, completo, consistente, verificable y trazable. Por ello, este documento no se limita a repetir los identificadores RF y ERF, sino que define condiciones de entrada, procesamiento, salida, reglas de negocio, permisos, eventos de auditoría, criterios de aceptación y casos borde. Esta estructura reduce ambigüedad durante desarrollo, facilita pruebas automatizadas y soporta auditorías internas o externas.

La presentación sigue principios de organización jerárquica, citación autor-fecha y referencias verificables, coherentes con la séptima edición de las Normas APA (American Psychological Association [APA], 2020). En Markdown, la fidelidad visual de interlineado, márgenes, sangría francesa y tipografía debe garantizarse en la exportación final a `.docx` o `.pdf`.

## Objetivo

Especificar formalmente los requisitos funcionales del software, definiendo comportamiento esperado, responsabilidades de roles, restricciones de seguridad, criterios de aceptación y mecanismos de verificación para los módulos de gestión de usuarios, ambientes, asistencia, configuración, históricos, justificaciones, reportes y gestión del colegio.

## Alcance

Este documento aplica al desarrollo, prueba, implantación y mantenimiento de la plataforma web administrativa y la aplicación móvil asociada. Comprende los grupos de requisitos funcionales RF 1 a RF 8 y sus requerimientos específicos ERF.

Incluye:

- Gestión de usuarios, roles y permisos.
- Importación masiva mediante archivos CSV.
- Autenticación móvil y recuperación de credenciales.
- Gestión de ambientes, fichas, responsables y jornadas.
- Registro de asistencia mediante rostro y huella dactilar.
- Control de ingreso de personal no registrado.
- Configuración móvil de alertas, idioma, paletas y parámetros de usuario.
- Alertas de fallos en dispositivos IoT.
- Parametrización de retardos y justificaciones.
- Históricos de asistencia e inasistencia.
- Auditoría de cambios de usuario.
- Carga, aprobación, rechazo y notificación de justificaciones.
- Exportación de reportes, dashboard analítico y consultas por rango de fechas.
- Gestión de cursos y planes de estudio.

No incluye, en esta versión:

- Diseño arquitectónico detallado.
- Modelo físico de base de datos.
- Contratos de API completos.
- Plan de infraestructura productiva.
- Análisis jurídico profundo de protección de datos, salvo lineamientos de seguridad y privacidad aplicables.

## Definiciones

- **RF**: Requerimiento funcional de alto nivel.
- **ERF**: Requerimiento funcional específico derivado de un RF.
- **Actor**: Persona, sistema o dispositivo que interactúa con el software.
- **Precondición**: Estado que debe cumplirse antes de iniciar un requerimiento.
- **Postcondición**: Estado esperado al finalizar exitosamente un requerimiento.
- **Regla de negocio**: Restricción o política que gobierna el comportamiento del sistema.
- **Criterio de aceptación**: Condición objetiva que permite declarar cumplido un requerimiento.
- **RBAC**: Control de acceso basado en roles.
- **Biometría**: Medida fisiológica o conductual usada para identificar o verificar una persona.
- **Plantilla biométrica**: Representación matemática cifrada de rasgos faciales o dactilares, distinta de la imagen cruda.
- **IoT**: Internet de las cosas; dispositivos conectados capaces de generar telemetría o eventos.
- **Ficha**: Grupo, curso o unidad académica operativa asociada a responsables, jornadas y ambientes.
- **Jornada**: Franja horaria o turno asociado a actividades académicas o administrativas.
- **Justificación**: Soporte documental presentado para explicar una inasistencia, retardo o irregularidad.
- **Auditoría**: Registro trazable de quién hizo qué, cuándo, desde dónde y sobre qué dato.

# Actores del sistema

| Actor | Descripción | Interacción principal |
|---|---|---|
| Administrador general | Usuario con privilegios amplios de configuración y gestión | Administra usuarios, roles, permisos, cursos, parámetros y reportes globales |
| Responsable académico | Instructor, coordinador o supervisor asignado a fichas o ambientes | Gestiona asistencia, justificaciones y consultas de su alcance |
| Personal registrado | Usuario autorizado del colegio | Consulta su perfil, marca asistencia y presenta justificaciones |
| Personal no registrado | Visitante, contratista o persona temporal | Ingresa mediante registro alternativo supervisado |
| Aplicación móvil | Cliente usado en dispositivos móviles | Autenticación, registro facial, huella, alertas, configuración y consultas |
| Plataforma web | Interfaz administrativa y de consulta | Gestión maestra, importación CSV, reportes y auditoría |
| Dispositivos IoT | Sensores, controladores o periféricos conectados | Envían estado, fallos y eventos de operación |
| Servicio de notificación | Componente técnico de mensajería | Entrega push, correo o SMS según configuración |
| Auditor o calidad | Rol de supervisión | Revisa trazabilidad, evidencias y cumplimiento normativo |

# Premisas y restricciones

## Premisas

- Los usuarios registrados poseen credenciales únicas o documento de identidad válido.
- Los dispositivos móviles soportan cámara frontal y, opcionalmente, sensor de huella.
- La aplicación móvil requiere permisos del sistema operativo para cámara, biométricos, notificaciones y almacenamiento cuando aplique.
- Existe un catálogo base de cursos, ambientes, fichas y jornadas antes de operar asistencia crítica.
- Los responsables asignados tienen relación contractual o laboral vigente con el colegio.

## Restricciones

- El almacenamiento de datos biométricos debe cumplir principios de minimización, seguridad, consentimiento y retención definida.
- La autorización debe aplicarse en servidor; ocultar menús en interfaz no es control de seguridad suficiente.
- La importación CSV debe mitigar riesgos de inyección de fórmulas y exposición de datos personales.
- Los reportes deben respetar el principio de mínimo privilegio según rol y alcance asignado.
- Las alertas críticas de seguridad o fallo IoT no deberían poder desactivarse completamente por usuario final, salvo política expresa aprobada.
- Los cambios de rol, estado de usuario y parametrización de retardos deben quedar auditados.

# Criterios transversales de aceptación

Todo requerimiento funcional descrito en este informe debe cumplir, como mínimo, los siguientes criterios transversales:

1. Validación de entradas en cliente y servidor.
2. Autenticación previa cuando el requerimiento involucre datos personales o administrativos.
3. Autorización basada en roles y permisos, con denegación por defecto.
4. Mensajes de error claros, sin filtrar información sensible.
5. Registro de auditoría para operaciones creativas, modificativas, eliminatorias, de autenticación y de cambio de estado.
6. Protección de datos personales y biométricos mediante cifrado en tránsito y reposo.
7. Trazabilidad entre interfaz, API, base de datos y evidencia de despliegue.
8. Casos de prueba unitarios, de integración y extremos asociados al requerimiento.
9. Comportamiento determinista ante redes lentas, sesiones expiradas, permisos denegados y datos inconsistentes.
10. Accesibilidad básica: contraste legible, etiquetas claras y navegación por teclado en web.

# Requisitos funcionales

## RF 1 Gestión de usuarios

**Finalidad:** Administrar el ciclo de vida de identities digitales del sistema, incluyendo registro, autenticación, roles, permisos, estado y consulta de información.

**Plataformas:** Web administrativa y móvil según requerimiento específico.

**Dependencias principales:** RF 4.6, RF 5.2.

### ERF 1.1 Registrar usuarios

- **Propósito:** Crear un usuario nuevo con datos personales, credenciales iniciales y relación con roles.
- **Plataforma:** Web.
- **Precondiciones:** El operador autenticado posee permiso de creación de usuarios.
- **Entradas mínimas:** Documento de identidad, tipo de documento, nombres, apellidos, correo electrónico, teléfono opcional, rol inicial y estado.
- **Procesamiento:** Validar formatos, unicidad de documento y correo, aplicar política de contraseñas o generar invitación segura, registrar auditoría y vincular rol inicial.
- **Salidas:** Usuario creado, identificador único, mensaje de éxito o error detallado, evento de auditoría.
- **Reglas de negocio:**
  - Un documento de identidad no puede repetirse dentro de la misma vigencia o tipo documental.
  - El correo debe ser único para autenticación, salvo política contraria aprobada.
  - El usuario nace activo solo si la política de alta lo permite; de lo contrario, queda pendiente de activación.
- **Criterios de aceptación:**
  - El sistema impide registrar usuarios con documento duplicado.
  - El sistema valida formato de correo y teléfono cuando sean obligatorios.
  - El usuario queda persistido con fecha de creación y operador responsable.
  - Se envía invitación o credencial inicial por canal seguro.
- **Seguridad y privacidad:**
  - No almacenar contraseñas en texto plano.
  - Minimizar datos personales en logs.
  - Registrar consentimiento cuando aplique tratamiento de datos sensibles.
- **Casos borde:**
  - Correo con dominio inválido.
  - Documento con caracteres no permitidos.
  - Intento de registro concurrente del mismo documento.
  - Fallo del servicio de correo durante invitación.
- **Pruebas sugeridas:**
  - Unitarias de validación de campos.
  - Integración contra restricción de unicidad.
  - Extremo: dos solicitudes simultáneas con mismo documento.

#### ERF 1.1.1 Asignación de roles

- **Propósito:** Vincular uno o varios roles funcionales al usuario.
- **Plataforma:** Web.
- **Entradas:** Usuario, rol, fecha de vigencia opcional, motivo opcional.
- **Reglas de negocio:**
  - Solo usuarios con permiso pueden asignar roles.
  - Un usuario puede tener rol primario y roles secundarios si el modelo lo admite.
  - La asignación debe ser reversible y auditada.
- **Criterios de aceptación:**
  - El sistema muestra roles disponibles según catálogo vigente.
  - Impide asignar roles inexistentes o inactivos.
  - Registra operador, fecha, rol anterior y rol nuevo.
- **Seguridad:** Evitar escalada de privilegios mediante validación server-side del permiso del operador.
- **Casos borde:** Asignar rol a usuario desactivado, remover último rol obligatorio, cambio de rol durante sesión activa.

#### ERF 1.1.2 Asignación de permisos en plataforma y web

- **Propósito:** Definir capacidades técnicas y funcionales por rol o usuario.
- **Plataforma:** Web y API.
- **Modelo recomendado:** RBAC con permisos granulares, por ejemplo `user:create`, `attendance:view`, `iot:acknowledge`.
- **Reglas de negocio:**
  - Denegación por defecto.
  - Los permisos deben evaluarse en API, no solo en interfaz.
  - La plataforma web y móvil pueden compartir permisos lógicos, con capacidades distintas por canal.
- **Criterios de aceptación:**
  - Un usuario sin permiso recibe error 403 o equivalente y no visualiza acción sensible.
  - Un usuario con permiso ejecuta la operación sin bypass de cliente.
  - Los cambios de permiso se reflejan en nueva sesión o renovación de token.
- **Seguridad:** Tokens firmados, expiración corta, revocación si cambia rol o permiso crítico.
- **Casos borde:** Permiso retirado durante operación larga, caché de permisos desactualizada, rol sin permisos asignados.

### ERF 1.2 Registrar usuarios mediante archivos CSV

- **Propósito:** Permitir importación masiva de usuarios desde archivo estructurado.
- **Plataforma:** Web.
- **Entradas:** Archivo CSV codificado en UTF-8, con cabecera obligatoria y delimitador definido.
- **Columnas mínimas sugeridas:** `documento,tipo_documento,nombres,apellidos,correo,telefono,rol_inicial,estado`
- **Procesamiento:**
  - Validar estructura, codificación, tamaño máximo y cabecera.
  - Parsear filas sin ejecutar fórmulas.
  - Generar informe previo de filas válidas, duplicadas y con error.
  - Importar en lote transaccional o con reporte de éxito parcial, según política.
- **Criterios de aceptación:**
  - El sistema rechaza archivos con cabecera incompleta.
  - Muestra errores por número de fila y campo.
  - Impide inyección de fórmulas CSV en campos exportables.
  - Registra auditoría de importación con total procesado, exitoso y fallido.
- **Seguridad:**
  - Sanitizar valores que comiencen con `=`, `+`, `-`, `@`, tabulador o retorno de carro.
  - Restringir subida a usuarios autorizados.
  - Almacenar archivo temporal de forma segura y eliminarlo tras proceso.
- **Casos borde:**
  - Archivo vacío.
  - Acentos mal codificados.
  - Filas duplicadas dentro del mismo archivo.
  - Archivo muy grande que excede tiempo de solicitud.
- **Pruebas sugeridas:**
  - Prueba de CSV con fórmula maliciosa.
  - Prueba de importación parcial y rollback.
  - Prueba de rendimiento con diez mil filas.

### ERF 1.3 Inicio de sesión (Móvil)

- **Propósito:** Autenticar al usuario en la aplicación móvil.
- **Plataforma:** Móvil.
- **Métodos:** Correo o usuario con contraseña; opcionalmente desbloqueo biométrico local del dispositivo.
- **Reglas de negocio:**
  - Bloquear temporalmente tras N intentos fallidos.
  - No revelar si el usuario existe o la contraseña es incorrecta.
  - Exigir reautenticación para operaciones sensibles.
- **Criterios de aceptación:**
  - Credenciales válidas otorgan token de acceso y refresh seguros.
  - Credenciales inválidas muestran mensaje genérico.
  - Sesión expira conforme a política.
  - El desbloqueo biométrico local no sustituye la autenticación server-side salvo diseño aprobado.
- **Seguridad:**
  - Almacenar tokens en keystore o keychain.
  - Rotación de refresh token.
  - Device binding opcional según riesgo.
- **Casos borde:** Sin conexión, token revocado, cambio de dispositivo, cuenta desactivada durante sesión.

### ERF 1.4 Recuperación de contraseña

- **Propósito:** Permitir restablecer credenciales de forma segura.
- **Plataforma:** Web y móvil.
- **Flujo:** Solicitar recuperación, emitir token de un solo uso, validar identidad, permitir nuevo password.
- **Reglas de negocio:**
  - Token con expiración corta, por ejemplo quince a sesenta minutos.
  - Un solo uso.
  - Rate limiting por correo, IP y dispositivo.
  - Respuesta genérica si el correo no existe.
- **Criterios de aceptación:**
  - El usuario recibe enlace o código por canal seguro.
  - El sistema invalida token tras uso o expiración.
  - Al cambiar contraseña, se revocan sesiones activas salvo la actual si la política lo permite.
- **Seguridad:** No enviar contraseña nueva por correo; no loguear tokens.
- **Casos borde:** Correo no registrado, token reutilizado, intento de enumeración de usuarios, canal de notificación caído.

### ERF 1.5 Cambio de contraseña

- **Propósito:** Permitir al usuario autenticado modificar su contraseña.
- **Plataforma:** Web y móvil.
- **Precondiciones:** Sesión válida o paso de reautenticación.
- **Reglas de negocio:**
  - Exigir contraseña actual o factor adicional.
  - Aplicar política de complejidad.
  - Impedir reutilización de contraseñas recientes si la política lo define.
- **Criterios de aceptación:**
  - Contraseña débil es rechazada.
  - Cambio exitoso genera auditoría y revocación de otras sesiones.
  - El usuario permanece autenticado solo si la política lo permite.
- **Casos borde:** Olvido de contraseña actual, cuenta bloqueada, cambio simultáneo en dos dispositivos.

### ERF 1.6 Activar/Desactivar usuarios

- **Propósito:** Suspender o restaurar el acceso sin eliminar histórico.
- **Plataforma:** Web.
- **Reglas de negocio:**
  - Usuario desactivado no puede autenticarse.
  - Sesiones activas deben revocarse.
  - Se conserva rastro de asistencia, justificaciones y auditoría.
- **Criterios de aceptación:**
  - Cambio de estado queda auditado con motivo y operador.
  - Intento de login de usuario desactivado es rechazado.
  - Reactivación restaura acceso según roles vigentes.
- **Casos borde:** Desactivar administrador propio, desactivar usuario con proceso abierto, reactivación sin roles válidos.

### ERF 1.7 Eliminar usuarios

- **Propósito:** Retirar usuario del sistema respetando retención legal y trazabilidad.
- **Plataforma:** Web.
- **Modelo recomendado:** Eliminación lógica o anonimización, no borrado físico inmediato.
- **Reglas de negocio:**
  - Solo roles elevados pueden eliminar.
  - Debe registrarse motivo.
  - Si existen registros históricos críticos, el usuario se anonimiza pero se conserva evidencia operativa.
- **Criterios de aceptación:**
  - El usuario desaparece de listas activas.
  - No puede autenticarse.
  - Los datos personales sensibles se anonimizan o cifran según política.
  - Queda auditoría irreversible del evento.
- **Casos borde:** Usuario con justificaciones en trámite, usuario con roles asignados, solicitud de borrado derecho al olvido con excepciones legales.

### ERF 1.8 Consulta de información del usuario

- **Propósito:** Permitir visualizar datos de perfil, estado, roles y asignaciones.
- **Plataforma:** Web y móvil.
- **Reglas de negocio:**
  - El usuario puede consultar su propia información.
  - Administradores y responsables consultan según alcance autorizado.
  - Campos sensibles se enmascaran si no hay permiso explícito.
- **Criterios de aceptación:**
  - La consulta muestra datos actualizados.
  - No expone información de otros usuarios sin autorización.
  - Acceso a datos sensibles queda auditado cuando aplique.
- **Casos borde:** Perfil incompleto, roles vencidos, solicitud desde sesión expirada.

## RF 2 Gestión de ambientes

**Finalidad:** Administrar espacios físicos o lógicos, fichas académicas, responsables y jornadas asociadas.

**Plataformas:** Web principalmente; móvil para consulta según rol.

**Dependencias principales:** RF 1, RF 3, RF 8.

### ERF 2.1 Registrar ambientes o salones

- **Propósito:** Crear y mantener el catálogo de ambientes donde ocurren actividades.
- **Plataforma:** Web.
- **Entradas mínimas:** Código, nombre, capacidad, ubicación, tipo, estado.
- **Reglas de negocio:**
  - Código único.
  - Capacidad mayor que cero si es físico.
  - No eliminar ambiente con asignaciones activas; usar inactivación.
- **Criterios de aceptación:**
  - El sistema valida duplicidad de código.
  - Permite consultar ambientes activos e inactivos según permiso.
  - Queda auditada creación, edición e inactivación.
- **Casos borde:** Ambiente sin ubicación, capacidad cero, cambio de código con historial asociado.

### ERF 2.2 Registrar fichas o cursos

- **Propósito:** Definir unidades académicas operativas vinculadas a cursos, periodos y responsables.
- **Plataforma:** Web.
- **Entradas mínimas:** Código de ficha, nombre, curso asociado, periodo, modalidad, capacidad, estado.
- **Reglas de negocio:**
  - Una ficha debe vincularse a un curso vigente del catálogo RF 8.
  - El código de ficha debe ser único dentro del periodo académico.
  - No puede activarse sin responsable asignado si la política lo exige.
- **Criterios de aceptación:**
  - Crear ficha exitosa queda visible para responsables autorizados.
  - Editar ficha conserva histórico si cambia datos críticos.
  - Inactivar ficha impide nuevas asignaciones pero conserva históricos.
- **Casos borde:** Ficha duplicada, curso eliminado, cambio de periodo con asistentes previos.

### ERF 2.3 Asignar responsables

- **Propósito:** Vincular personas autorizadas al manejo de ambientes, fichas o jornadas.
- **Plataforma:** Web.
- **Reglas de negocio:**
  - El responsable debe ser usuario activo con rol pertinente.
  - La asignación puede tener vigencia inicio-fin.
  - Debe permitirse un responsable principal y, opcionalmente, suplentes.
- **Criterios de aceptación:**
  - El sistema impide asignar usuarios inactivos.
  - La asignación se refleja en permisos de consulta y operación.
  - Queda auditada asignación, remoción y cambio de vigencia.

#### ERF 2.3.1 Asignar fichas

- **Propósito:** Relacionar responsables con fichas específicas.
- **Reglas de negocio:**
  - Un responsable solo ve y opera fichas asignadas, salvo rol global.
  - La asignación puede ser múltiple.
  - Debe controlarse traslape injustificado de responsabilidades si la política lo exige.
- **Criterios de aceptación:**
  - Asignación exitosa habilita acciones sobre la ficha.
  - Remoción inmediata o programada revoca acceso según vigencia.
- **Casos borde:** Responsable retirado con justificaciones pendientes, asignación circular, ficha sin responsable.

#### ERF 2.3.2 Asignar jornadas

- **Propósito:** Vincular responsables a franjas horarias o turnos.
- **Entradas:** Responsable, jornada, ambiente opcional, ficha opcional, vigencia.
- **Reglas de negocio:**
  - Una jornada define inicio, fin, día o patrón recurrente.
  - Debe validarse zona horaria.
  - No se permiten solapamientos que generen ambigüedad de responsabilidad, salvo excepción aprobada.
- **Criterios de aceptación:**
  - El sistema calcula correctamente la jornada activa por fecha y hora.
  - La asignación impacta reportes de asistencia y retardos.
- **Casos borde:** Cambio de horario de verano, jornada cruzada de medianoche, responsable con múltiples jornadas simultáneas.

## RF 3 Generar registros de entradas y salidas

**Finalidad:** Capturar eventos de ingreso y egreso de personal registrado y no registrado, con soporte biométrico móvil y alternativas controladas.

**Plataformas:** Móvil principalmente; web para supervisión y auditoría.

**Dependencias principales:** RF 1, RF 2, RF 4.5, RF 4.7, RF 6.

### ERF 3.1 Registrar ingresos y salidas

- **Propósito:** Crear evento de asistencia con dirección entrada o salida.
- **Plataforma:** Móvil y API.
- **Entradas mínimas:** Usuario o credencial temporal, tipo de evento, marca temporal, dispositivo, método, ubicación o ambiente si aplica.
- **Reglas de negocio:**
  - Un mismo usuario no puede registrar dos entradas consecutivas sin salida, salvo corrección autorizada.
  - La marca temporal debe usar hora servidor o sincronización confiable.
  - El evento debe ser idempotente ante reintentos de red.
- **Criterios de aceptación:**
  - Registro exitoso devuelve confirmación y guarda evidencia.
  - Registro duplicado es detectado y rechazado o corregido según política.
  - El evento queda vinculado a ficha, ambiente o jornada cuando aplique.
- **Seguridad:** Firmar payload móvil, validar dispositivo, prevenir replay attacks.
- **Casos borde:** Sin conexión, reloj del dispositivo alterado, cambio brusco de ubicación, usuario desactivado durante intento.

#### ERF 3.1.1 Registro facial (Móvil)

- **Propósito:** Identificar o verificar al usuario mediante reconocimiento facial en móvil.
- **Plataforma:** Móvil.
- **Flujo:** Captura facial, detección de vitalidad, extracción de plantilla, comparación con plantilla registrada, decisión.
- **Reglas de negocio:**
  - Requiere consentimiento y registro de enrolamiento previo.
  - Debe aplicar umbral de coincidencia configurable.
  - Si falla, habilita ruta alterna: huella, credencial o registro asistido.
- **Criterios de aceptación:**
  - Usuario válido es reconocido dentro de umbral aprobado.
  - Suplantación básica con foto o pantalla debe ser rechazada por mecanismos de liveness.
  - No se almacena imagen cruda innecesariamente; se conserva plantilla cifrada y metadatos de auditoría.
- **Seguridad y privacidad:**
  - Cifrado de plantillas biométricas.
  - Retención definida.
  - Prohibición de compartir plantillas entre finalidades no autorizadas.
  - Registro de intentos fallidos.
- **Casos borde:** Luz insuficiente, rostro parcialmente cubierto, cambios fisiológicos, dispositivo rooteado o con emulator, fallo de sensor.

#### ERF 3.1.2 Registro alterno (Huella)

- **Propósito:** Usar huella dactilar como método alternativo o complementario.
- **Plataforma:** Móvil con sensor compatible.
- **Reglas de negocio:**
  - Solo disponible si el dispositivo y sistema operativo lo soportan.
  - Debe respetar enclave seguro del dispositivo cuando exista.
  - Falla de sensor habilita método alterno.
- **Criterios de aceptación:**
  - Huella válida registra asistencia.
  - Huella no registrada genera error seguro y opción alterna.
  - No se almacena imagen dactilar cruda fuera del sistema operativo si la plataforma ofrece plantilla segura.
- **Casos borde:** Dedo húmedo o lesionado, sensor sucio, cambio de dispositivo, usuario con múltiples dedos registrados.

### ERF 3.2 Control de ingresos de personal no registrado

- **Propósito:** Gestionar ingreso de visitantes, contratistas o personas sin cuenta permanente.
- **Plataforma:** Móvil o web operativa.
- **Reglas de negocio:**
  - Requiere anfitrión o responsable que valide el ingreso.
  - Debe generarse credencial temporal, QR o registro equivalente.
  - El acceso se limita a zonas, tiempos y finalidades aprobadas.
- **Criterios de aceptación:**
  - El sistema registra identidad mínima, motivo, empresa si aplica, hora de ingreso y salida.
  - No permite acceso a funciones internas sensibles.
  - La permanencia vencida genera alerta.
- **Seguridad:** No solicitar datos biométricos salvo base legal y técnica aprobada.

#### ERF 3.2.1 Registro alternativo enfocado a usuarios no registrados

- **Propósito:** Capturar manualmente o con apoyo documental a personas externas.
- **Entradas mínimas:** Documento, nombres, apellidos, motivo, empresa opcional, anfitrión, vigencia, foto opcional.
- **Reglas de negocio:**
  - El registro temporal expira automáticamente.
  - Debe permitir checkout manual o automático por geocercas si está disponible.
  - No crea usuario permanente salvo conversión autorizada.
- **Criterios de aceptación:**
  - Se genera identificador temporal único.
  - El anfitrión recibe notificación si la política lo exige.
  - Queda trazabilidad completa del ingreso y egreso.
- **Casos borde:** Visitante sin documento, anfitrión no disponible, salida no registrada, extensión de permanencia.

## RF 4 Configuración del sistema

**Finalidad:** Administrar preferencias móviles, alertas, idioma, temas, parámetros de usuario, fallos IoT, cambios de rol y reglas de retardos o justificaciones.

**Plataformas:** Móvil y web administrativa.

**Dependencias principales:** RF 1, RF 3, RF 5, RF 6, RF 7.

### ERF 4.1 Configurar alertas (Móvil)

- **Propósito:** Permitir al usuario gestionar notificaciones del sistema.
- **Plataforma:** Móvil.
- **Tipos de alerta:** Asistencia, justificación aprobada o rechazada, fallo IoT, cambio de rol, recordatorios académicos, alertas críticas de seguridad.
- **Reglas de negocio:**
  - Las alertas críticas de seguridad o cumplimiento pueden ser no desactivables.
  - La configuración debe sincronizarse entre dispositivo y servidor si el usuario cambia de móvil.
- **Criterios de aceptación:**
  - El usuario activa o desactiva categorías permitidas.
  - El sistema respeta permisos del sistema operativo.
  - Las preferencias persisten tras reinicio de app.

#### ERF 4.1.1 Activar/Desactivar alertas

- **Propósito:** Controlar recepción por tipo de evento.
- **Reglas de negocio:**
  - Desactivar no suprime registro de auditoría server-side.
  - Debe existir valor predeterminado seguro.
- **Criterios de aceptación:**
  - Cambio inmediato reflejado en próxima notificación.
  - Alertas obligatorias no pueden desactivarse desde interfaz de usuario final.
- **Casos borde:** Permiso de notificación denegado por SO, usuario con múltiples dispositivos, sincronización conflictiva.

#### ERF 4.1.2 Cambio en el tono de la notificación

- **Propósito:** Personalizar sonido o vibración.
- **Reglas de negocio:**
  - Solo sonidos locales o assets certificados.
  - Debe respetar accesibilidad y volumen del sistema.
- **Criterios de aceptación:**
  - El usuario previsualiza el tono.
  - La selección persiste por usuario o dispositivo según política.
  - Si el tono no está disponible, aplica default.

### ERF 4.2 Configurar idiomas (Móvil)

- **Propósito:** Permitir selección de idioma de interfaz.
- **Idiomas mínimos sugeridos:** Español e inglés; portugués opcional según contexto.
- **Reglas de negocio:**
  - Fallback automático a español si traducción falta.
  - Fechas, números y moneda se formatean según locale.
- **Criterios de aceptación:**
  - Cambio de idioma actualiza interfaz sin reinicio obligatorio, o informa si requiere reinicio.
  - Mensajes de error también se traducen.
- **Casos borde:** Locale no soportado, texto truncado, mezcla de idiomas por recursos faltantes.

### ERF 4.3 Actualización paletas de colores (Móvil)

- **Propósito:** Aplicar temas visuales y paletas institucionales.
- **Reglas de negocio:**
  - Debe mantener contraste accesible.
  - Modo claro y oscuro recomendados.
  - Paleta no debe alterar semántica de estados: éxito, advertencia, error.
- **Criterios de aceptación:**
  - El usuario selecciona tema y se aplica inmediatamente.
  - Los componentes críticos conservan legibilidad.
  - La preferencia persiste.
- **Casos borde:** Tema personalizado con bajo contraste, actualización de app con paleta incompatible.

### ERF 4.4 Actualización de parámetros del usuario (Móvil)

- **Propósito:** Centralizar ajustes personales del usuario en móvil.
- **Parámetros:** Idioma, tema, notificaciones, datos de contacto opcionales, enrolamiento biométrico, cambio de contraseña, privacidad.
- **Reglas de negocio:**
  - Cambios sensibles requieren reautenticación.
  - Debe validarse formato y permisos.
- **Criterios de aceptación:**
  - Guardado exitoso confirma cambio y audita si es relevante.
  - Cancelación no persiste modificaciones.
- **Casos borde:** Sesión expirada durante edición, conflicto de sincronización, permiso de cámara denegado al enrolar rostro.

### ERF 4.5 Gestión de alertas de fallos de dispositivos IoT

- **Propósito:** Supervisar estado operativo de dispositivos conectados.
- **Eventos mínimos:** Offline, batería baja, sensor fallando, manipulación, lectura inválida, reconexión.
- **Reglas de negocio:**
  - Umbrales configurables por tipo de dispositivo.
  - Escalamiento por responsable si no se atiende en tiempo definido.
  - Acknowledgment obligatorio para eventos críticos.
- **Criterios de aceptación:**
  - El sistema detecta pérdida de heartbeat.
  - Genera alerta con dispositivo, ubicación, hora y severidad.
  - Permite asignar ticket o acción correctiva.
- **Seguridad:** Autenticación mutua dispositivo-servidor, rotación de credenciales, aislamiento de red.
- **Casos borde:** Dispositivo comprometido, ráfaga de eventos repetidos, falsa alarma por mantenimiento.

### ERF 4.6 Cambio de rol en un usuario

- **Propósito:** Modificar rol asignado a un usuario existente.
- **Plataforma:** Web.
- **Reglas de negocio:**
  - Requiere operador autorizado.
  - Debe registrarse motivo y vigencia.
  - Puede revocar sesiones o forzar refresco de permisos.
- **Criterios de aceptación:**
  - El nuevo rol se aplica según política inmediata o programada.
  - El usuario pierde o gana acceso conforme a permisos.
  - Queda auditoría completa del cambio.
- **Casos borde:** Cambio de rol propio, rol inexistente, usuario con sesiones múltiples, cambio durante proceso crítico.

### ERF 4.7 Retardos y justificaciones (Parametrización)

- **Propósito:** Configurar reglas para clasificar retardos, inasistencias y aceptación de justificaciones.
- **Parámetros sugeridos:** Minutos de tolerancia, hora límite de marcación, tipo de inasistencia, plazos de justificación, tipos de soporte aceptados, aprobadores, notificaciones.
- **Reglas de negocio:**
  - Los parámetros deben versionarse y tener fecha de vigencia.
  - Cambios no deben alterar históricos ya cerrados, salvo corrección autorizada.
  - Debe definirse si el retardo se calcula por jornada, ficha o ambiente.
- **Criterios de aceptación:**
  - El sistema clasifica correctamente asistencia, retardo e inasistencia según parámetros vigentes.
  - Permite consultar parámetros activos y su historial.
  - Impacta reportes RF 5 y RF 7 sin inconsistencias.
- **Casos borde:** Cambio de parámetro a mitad de día, jornada cruzada de medianoche, justificación presentada fuera de plazo, zona horaria inconsistente.

## RF 5 Generar históricos

**Finalidad:** Consultar y conservar antecedentes de asistencia, inasistencia y cambios de usuario.

**Plataformas:** Móvil y web.

**Dependencias principales:** RF 1, RF 2, RF 3, RF 4.7.

### ERF 5.1 Asistencias e inasistencias (Móvil) por parámetros

- **Propósito:** Consultar movimientos de asistencia filtrados por fecha, ficha, ambiente, instructor o persona.
- **Plataforma:** Móvil.
- **Filtros mínimos:** Rango de fechas, ficha, ambiente, instructor, persona, estado, método de registro.
- **Reglas de negocio:**
  - El responsable solo ve datos de su alcance.
  - El cálculo de inasistencia usa parámetros RF 4.7.
  - Debe soportar paginación y ordenamiento.
- **Criterios de aceptación:**
  - La consulta devuelve resultados consistentes con base de datos.
  - Los filtros combinados funcionan sin exponer datos no autorizados.
  - Se muestra detalle del evento: hora, método, dispositivo, estado y justificación asociada si existe.
- **Casos borde:** Rango de fechas invertido, muchos resultados, usuario sin asignaciones, datos biométricos sin match confirmado.

### ERF 5.2 Historial de cambios del usuario

- **Propósito:** Registrar bitácora de modificaciones al perfil, roles, permisos, estado y credenciales.
- **Plataforma:** Web y API.
- **Campos mínimos de auditoría:** Entidad afectada, identificador, operador, fecha/hora, acción, valores anterior y nuevo, IP, dispositivo, motivo.
- **Reglas de negocio:**
  - El historial debe ser inmutable o append-only.
  - Datos sensibles se enmascaran en vista no privilegiada.
  - Retención según política legal y operativa.
- **Criterios de aceptación:**
  - Cada cambio relevante genera registro.
  - El registro permite reconstruir línea de tiempo del usuario.
  - Solo roles autorizados consultan historial completo.
- **Casos borde:** Cambio simultáneo por dos administradores, importación CSV masiva, borrado lógico, acceso desde sesión comprometida.

## RF 6 Gestión de justificaciones

**Finalidad:** Permitir presentar, revisar, aprobar o rechazar soportes que expliquen irregularidades de asistencia.

**Plataformas:** Móvil y web.

**Dependencias principales:** RF 1, RF 3, RF 4.7, RF 5.

### ERF 6.1 Carga de soportes de justificación

- **Propósito:** Subir documento o imagen que respalde una inasistencia o retardo.
- **Plataforma:** Móvil y web.
- **Entradas:** Evento de asistencia asociado, tipo de justificación, descripción, archivo.
- **Formatos mínimos sugeridos:** PDF, JPG, PNG.
- **Reglas de negocio:**
  - Tamaño máximo configurable.
  - Solo eventos permitidos por plazo pueden justificarse.
  - El archivo debe analizarse contra malware.
- **Criterios de aceptación:**
  - Carga exitosa asocia soporte al evento.
  - Formato no permitido es rechazado con mensaje claro.
  - Queda huella digital o hash del archivo.
- **Seguridad:** Almacenamiento cifrado, control de acceso, eliminación segura si corresponde.
- **Casos borde:** Archivo corrupto, nombre con caracteres especiales, subida interrumpida, documento sensible expuesto en miniatura.

### ERF 6.2 Aprobación y rechazo

- **Propósito:** Permitir al evaluador decidir sobre la justificación.
- **Plataforma:** Web y móvil según rol.
- **Estados sugeridos:** Pendiente, en revisión, aprobada, rechazada, cerrada.
- **Reglas de negocio:**
  - Solo aprobadores designados pueden resolver.
  - El rechazo requiere comentario obligatorio.
  - No se permite doble resolución sin anulación auditada.
- **Criterios de aceptación:**
  - Aprobación actualiza estado del evento si la política lo exige.
  - Rechazo notifica al solicitante.
  - Toda decisión queda auditada con aprobador, fecha y motivo.
- **Casos borde:** Aprobador sin permisos, justificación ya resuelta, cambio de parámetros durante revisión, apelación si existe.

### ERF 6.3 Notificación automática del resultado (A/R)

- **Propósito:** Informar automáticamente al usuario sobre aprobación o rechazo.
- **Canales:** Push móvil, correo, SMS opcional.
- **Reglas de negocio:**
  - Plantilla clara y sin datos sensibles innecesarios.
  - Reintentos si el canal falla.
  - Registro de entrega.
- **Criterios de aceptación:**
  - El usuario recibe notificación tras decisión.
  - La notificación incluye enlace seguro al detalle.
  - Falla de envío genera alerta operativa y reintento.
- **Casos borde:** Notificación desactivada, correo lleno, número inválido, duplicidad de envío por reintento.

## RF 7 Visualización y reportes avanzados

**Finalidad:** Proveer consulta analítica, exportación y reportes operativos sobre asistencia, retardos, inasistencias y actividad del sistema.

**Plataformas:** Web principalmente; móvil para consulta reducida.

**Dependencias principales:** RF 2, RF 3, RF 4.7, RF 5, RF 6.

### ERF 7.1 Exportar reporte

- **Propósito:** Generar archivo descargable con datos filtrados.
- **Formatos mínimos:** CSV, XLSX, PDF.
- **Reglas de negocio:**
  - Exportaciones grandes deben ser asíncronas.
  - El enlace de descarga expira.
  - Se aplica enmascarado según rol.
  - CSV debe mitigar inyección de fórmulas.
- **Criterios de aceptación:**
  - El archivo exportado coincide con filtros vistos en pantalla.
  - Se registra auditoría de exportación: usuario, filtros, formato, fecha.
  - No exporta datos fuera del alcance autorizado.
- **Casos borde:** Dataset vacío, timeouts, sensibilidad de datos, descarga repetida, archivo corrupto.

### ERF 7.2 Dashboard analítico

- **Propósito:** Visualizar indicadores clave de asistencia, justificaciones, retardos, fallos IoT y actividad de usuarios.
- **Plataforma:** Web.
- **KPIs sugeridos:** Tasa de asistencia, tasa de retardo, inasistencias por ficha, justificaciones pendientes, aprobación/rechazo, dispositivos IoT con fallo, usuarios activos.
- **Reglas de negocio:**
  - Filtros globales por periodo, ambiente, ficha e instructor.
  - Drill-down a detalle.
  - Actualización periódica o bajo demanda.
- **Criterios de aceptación:**
  - Los indicadores calculan correctamente según parámetros vigentes.
  - Tiempos de carga aceptables para volumen esperado.
  - Acceso restringido por rol.
- **Casos borde:** Datos nulos, rangos largos, concurrencia de consultas pesadas, caché desactualizada.

### ERF 7.3 Reportes por rango de fechas

- **Propósito:** Consultar información acotada temporalmente.
- **Reglas de negocio:**
  - Fecha inicio no mayor a fecha fin.
  - Límite máximo de rango configurable.
  - Zona horaria explícita.
  - Los días son inclusivos o exclusivos según definición documentada.
- **Criterios de aceptación:**
  - El reporte devuelve solo eventos dentro del rango.
  - Errores de rango se muestran claramente.
  - Cruces de medianoche se calculan correctamente.
- **Casos borde:** Horario de verano, rango de un solo día, fecha futura, zona horaria del dispositivo distinta al servidor.

### ERF 7.4 Consulta de retardos e inasistencias por parámetros

- **Propósito:** Listar eventos clasificados como retardo o inasistencia según filtros.
- **Filtros:** Fecha, ficha, ambiente, instructor, persona, tipo de evento, estado de justificación.
- **Reglas de negocio:**
  - Usa parámetros RF 4.7.
  - Debe mostrar causa de clasificación: hora de marcación, tolerancia, ausencia de salida, etc.
- **Criterios de aceptación:**
  - La consulta es consistente con históricos RF 5.
  - Permite exportar resultado.
  - Respeta alcance del responsable.
- **Casos borde:** Evento sin jornada asignada, justificación aprobada que revierte inasistencia, cambio retroactivo de parámetros.

## RF 8 Gestión del colegio

**Finalidad:** Administrar catálogo de cursos y planes de estudio asociados a materias y fichas académicas.

**Plataformas:** Web.

**Dependencias principales:** RF 2, RF 5, RF 7.

### ERF 8.1 Agregar y eliminar cursos

- **Propósito:** Mantener el catálogo maestro de cursos ofrecidos por el colegio.
- **Entradas mínimas:** Código, nombre, descripción, duración, modalidad, estado.
- **Reglas de negocio:**
  - Código único.
  - Eliminación lógica si existen fichas, planes o históricos asociados.
  - Inactivar curso impide nuevas fichas, pero conserva reportes previos.
- **Criterios de aceptación:**
  - Crear curso exitoso lo habilita para asignación en fichas.
  - Editar curso conserva trazabilidad si cambia datos críticos.
  - Eliminar o inactivar exige confirmación y auditoría.
- **Casos borde:** Curso con fichas activas, duplicidad de código, cambio de modalidad con alumnos asignados.
