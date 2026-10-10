---
title: "Documentación de Análisis del software"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
abstract: |
  El presente documento constituye la documentación de análisis del software para un sistema de gestión académica, administrativa y de asistencia, con componentes web y móviles. Se describen los objetivos del proyecto, el alcance, la justificación de su realización, la utilidad esperada, la finalidad estratégica, el aporte del análisis durante la creación del software y los principales beneficiarios. Asimismo, se incorporan elementos de análisis de contexto, partes interesadas, viabilidad, riesgos, datos, calidad de software, criterios de éxito y trazabilidad con otros artefactos del ciclo de vida. El documento busca servir como base técnica, gerencial y de auditoría para la especificación de requisitos, el diseño, la implementación, las pruebas y la mejora continua del sistema.
keywords:
  - análisis de software
  - ingeniería de requisitos
  - objetivos del proyecto
  - alcance funcional
  - justificación
  - beneficiarios
  - calidad de software
  - sistema académico
---

# Introducción

La documentación de análisis del software para el sistema FaceAttendEDU permite comprender el problema, el contexto y las condiciones necesarias para que la solución tecnológica genere valor real. En este proyecto, el análisis es crítico ya que integra procesos humanos, restricciones normativas, seguridad de la información y trazabilidad de asistencia.

El sistema se orienta a la gestión de usuarios, ambientes, fichas, registros de asistencia y reportes avanzados, partiendo de una situación de procesos fragmentados y riesgos de seguridad. Este documento establece los objetivos, delimita el alcance y proporciona una base verificable para la especificación de requisitos y el diseño arquitectónico, reduciendo la ambigüedad y alineando las expectativas de los stakeholders.

## Objetivo del documento
Analizar formalmente el contexto, los objetivos, el alcance y la viabilidad del proyecto para orientar la especificación de requisitos, el diseño, la implementación y la mejora continua.

## Alcance del documento
Este documento cubre el análisis estratégico del sistema (problema, objetivos, riesgos y criterios de éxito). No sustituye el informe de especificación de requisitos ni el diseño arquitectónico detallado.

# Contexto del proyecto

## Descripción general del sistema

El sistema es una plataforma compuesta por una aplicación web administrativa y una aplicación móvil operativa. Su propósito es digitalizar y controlar procesos relacionados con la identidad digital de los usuarios, la asignación de roles y permisos, la gestión de ambientes académicos, el registro de asistencia, el control de de personal no registrado, la parametrización de reglas de retardos y justificaciones, la generación de históricos, la producción de reportes y la administración del catálogo académico del colegio.

La solución se apoya en componentes de autenticación, biometría, notificaciones, gestión documental, auditoría, reportes analíticos y monitoreo de dispositivos IoT. Por su naturaleza, requiere controles robustos de seguridad, privacidad, usabilidad, trazabilidad y disponibilidad.

## Situación actual

Antes de la implementación del sistema, o durante su fase inicial de desarrollo, se identifican condiciones como las siguientes:

- Procesos de registro de asistencia manuales, semimanuales o dispersos en herramientas no integradas.
- Dificultad para consultar históricos de manera inmediata y confiable.
- Riesgo de errores humanos en digitación, duplicidad, omisión o manipulación de registros.
- Falta de visibilidad integral sobre retardos, inasistencias y justificaciones.
- Inconsistencias entre la interfaz móvil, los mockups aprobados y el avance real del desarrollo.
- Presencia de defectos funcionales en navegación móvil y selección de fechas.
- Gestión insuficiente de roles, permisos y segregación de responsabilidades.
- Necesidad de fortalecer protocolos de seguridad para prevenir falsificación, suplantación o acceso indebido.
- Ausencia o debilidad en mecanismos de auditoría trazable.
- Dificultad para generar reportes confiables destinados a la toma de decisiones.

## Problema central

El problema central puede formularse así:

> La organización carece de un sistema integrado, seguro, trazable y usable para gestionar usuarios, ambientes académicos, asistencia, justificaciones, reportes y configuración operativa, lo que incrementa el riesgo de errores, inconsistencias, accesos indebidos, demoras administrativas y baja calidad en la toma de decisiones.

## Situación deseada

La situación deseada consiste en contar con una plataforma que permita:

- Registrar y administrar usuarios con roles y permisos claramente definidos.
- Autenticar de forma segura en web y móvil.
- Controlar ambientes, fichas, responsables y jornadas.
- Registrar entradas y salidas con soporte biométrico y métodos alternos controlados.
- Atender personal no registrado mediante procesos supervisados y temporales.
- Configurar alertas, idioma, paletas y parámetros personales en móvil.
- Monitorear fallos de dispositivos IoT.
- Parametrizar reglas de retardos y justificaciones con versionamiento.
- Consultar históricos y auditoría de cambios.
- Cargar, aprobar o rechazar justificaciones con notificación automática.
- Generar reportes avanzados, dashboards y exportaciones seguras.
- Administrar cursos y planes de estudio del colegio.

## Brecha analizada

| Dimensión | Estado actual | Estado deseado | Brecha principal |
|---|---|---|---|
| Gestión de identidad | Usuarios, roles y permisos incompletos o mal visibles | Modelo RBAC claro, auditado y aplicado en servidor | Falta implementación completa de roles y permisos |
| Asistencia | Registros dispersos, manuales o propensos a error | Registro biométrico o alterno con trazabilidad | Necesidad de flujo confiable, seguro y verificable |
| Móvil | Errores de fecha, navegación inconsistente y mockup desalineado | Experiencia fluida, coherente con diseño aprobado | Corrección de defectos y alineación diseño-desarrollo |
| Seguridad | Riesgo de falsificación, suplantación o acceso indebido | Controles preventivos, autenticación robusta y auditoría | Fortalecer protocolos y preguntas de seguridad |
| Reportes | Información difícil de consolidar | Dashboards, exportaciones y consultas por parámetros | Construir capa analítica confiable y segura |
| Gobernanza | Decisiones basadas en datos incompletos | Trazabilidad, indicadores y revisión periódica | Institucionalizar análisis, CAPA y mejora continua |

# Objetivos del proyecto

## Objetivo general

Desarrollar e implementar una plataforma web y móvil que permita gestionar de forma integrada, segura, trazable y eficiente los usuarios, ambientes académicos, registros de asistencia, justificaciones, reportes, configuraciones operativas y catálogo educativo del colegio, con el fin de reducir errores operativos, fortalecer la seguridad de la información y mejorar la toma de decisiones.

## Objetivos específicos

1. Digitalizar la gestión de usuarios, incluyendo registro individual, registro masivo por CSV, inicio de sesión, recuperación y cambio de contraseña, activación, desactivación, eliminación controlada y consulta de información.
2. Implementar un modelo de roles y permisos basado en RBAC, aplicado de forma consistente en plataforma web, API y aplicación móvil.
3. Administrar ambientes, fichas o cursos, responsables y jornadas, garantizando asignaciones trazables y vigencias controladas.
4. Habilitar el registro de entradas y salidas mediante reconocimiento facial, huella dactilar y métodos alternos supervisados.
5. Controlar el ingreso de personal no registrado mediante credenciales temporales, registro alternativo y límites de permanencia.
6. Permitir configuración móvil de alertas, idiomas, paletas de color, parámetros de usuario y notificaciones.
7. Gestionar alertas derivadas de fallos o anomalías en dispositivos IoT.
8. Parametrizar reglas de retardos, inasistencias y justificaciones con versionamiento y auditoría.
9. Generar históricos de asistencia, inasistencia y cambios de usuario consultables por parámetros.
10. Implementar gestión de justificaciones con carga de soportes, aprobación, rechazo y notificación automática.
11. Proveer reportes avanzados, dashboard analítico, exportación segura y consultas por rango de fechas.
12. Administrar el catálogo de cursos y los planes de estudio de las materias asociados al colegio.
13. Garantizar seguridad, privacidad, trazabilidad, usabilidad y mantenibilidad como atributos transversales del sistema.

## Objetivos de negocio

| Objetivo de negocio | Descripción | Indicador asociado |
|---|---|---|
| Reducir errores operativos | Minimizar registros incorrectos, duplicados o incompletos | Tasa de correcciones manuales |
| Mejorar la trazabilidad | Permitir reconstruir quién, cuándo, cómo y sobre qué dato actuó | Cobertura de auditoría crítica |
| Fortalecer la seguridad | Prevenir accesos indebidos, falsificación y suplantación | Incidentes de seguridad evitados |
| Agilizar decisiones | Proveer información oportuna mediante reportes y dashboards | Tiempo de generación de reporte |
| Elevar experiencia de usuario | Reducir fricción en flujos móviles y administrativos | Satisfacción usuario y tasa de abandono |
| Sostener mejora continua | Alimentar acciones correctivas, preventivas y de mejoramiento | Eficacia CAPA verificada |

# Alcance

## Alcance incluido

El proyecto incluye el análisis, especificación, desarrollo, prueba, despliegue y puesta en producción de los siguientes componentes funcionales:

- Gestión de usuarios.
- Asignación de roles y permisos.
- Registro masivo de usuarios mediante archivos CSV.
- Inicio de sesión móvil.
- Recuperación y cambio de contraseña.
- Activación, desactivación y eliminación controlada de usuarios.
- Consulta de información del usuario.
- Gestión de ambientes o salones.
- Gestión de fichas o cursos.
- Asignación de responsables, fichas y jornadas.
- Registro de entradas y salidas.
- Registro facial móvil.
- Registro alterno por huella.
- Control de ingresos de personal no registrado.
- Registro alternativo para usuarios no registrados.
- Configuración de alertas móviles.
- Activación y desactivación de alertas.
- Cambio de tono de notificación.
- Configuración de idiomas móviles.
- Actualización de paletas de color móviles.
- Actualización de parámetros del usuario en móvil.
- Gestión de alertas de fallos de dispositivos IoT.
- Cambio de rol en un usuario.
- Parametrización de retardos y justificaciones.
- Históricos de asistencias e inasistencias por parámetros.
- Historial de cambios del usuario.
- Carga de soportes de justificación.
- Aprobación y rechazo de justificaciones.
- Notificación automática de resultado.
- Exportación de reportes.
- Dashboard analítico.
- Reportes por rango de fechas.
- Consulta de retardos e inasistencias por parámetros.
- Agregar y eliminar cursos.
- Plan de estudios de las materias.

También incluye requisitos transversales de seguridad, auditoría, validación de entradas, control de acceso, protección de datos personales y biométricos, manejo de errores, trazabilidad y criterios de aceptación verificables.

## Alcance excluido

En esta versión del análisis, no se incluyen explícitamente:

- Compra, instalación o certificación física de dispositivos IoT, cámaras, sensores o terminales móviles.
- Contratación de conectividad, hosting o licencias comerciales no definidas.
- Módulos de calificaciones académicas, matrícula financiera, nómina, contabilidad o tesorería.
- Integraciones con sistemas externos no especificados, como ERP, CRM, pasarelas de pago o plataformas gubernamentales.
- Migración masiva de históricos sin aprobación de mapeo, calidad de datos y plan de respaldo.
- Certificación jurídica definitiva sobre tratamiento de datos biométricos, aunque sí se identifican requisitos de privacidad y seguridad.
- Soporte técnico 24/7, si no es contratado expresamente.
- Diseño gráfico final de marca, salvo alineación con paletas y mockups existentes.
- Pruebas de penetración externas, a menos que se aprueben como actividad complementaria.

## Límites del análisis

Este documento se basa en la información funcional proporcionada, en hallazgos operativos detectados y en buenas prácticas de ingeniería de software. No sustituye la validación legal, la auditoría forense, el diseño arquitectónico detallado ni la aprobación formal de presupuesto. Algunos criterios cuantitativos, como tiempos de respuesta, volúmenes máximos de usuarios o disponibilidad exacta, deberán concretarse con datos reales de operación.

# Justificación y Utilidad del Proyecto

## Necesidad detectada
El proyecto surge de la necesidad crítica de control, seguridad y eficiencia administrativa. La gestión dispersa de asistencia y reportes generaba riesgos operativos y dificultaba la auditoría. Además, se detectaron fallos técnicos durante el desarrollo inicial:
- Errores en campos de fecha y navegación en la aplicación móvil.
- Ausencia de preguntas de seguridad y protocolos antifalsificación.
- Desalineación entre los mockups y la implementación real.
- Visualización limitada de roles.

## Utilidad y Finalidad
El sistema proporciona una herramienta operativa para registrar, consultar y auditar procesos críticos del colegio. Su finalidad es modernizar los procesos académicos, reducir la dependencia de controles manuales y proteger los datos biométricos.

| Ámbito | Utilidad | Resultado esperado |
|---|---|---|
| Operativo | Digitalizar asistencia y justificaciones | Menor error y mayor rapidez |
| Administrativo | Centralizar usuarios y roles | Mejor control organizacional |
| Técnico | Definir requisitos y riesgos | Desarrollo predecible |
| Seguridad | Implementar RBAC y antifalsificación | Menor exposición al riesgo |
| Analítico | Proveer dashboards y reportes | Decisiones basadas en datos |
| Institucional | Establecer trazabilidad y calidad | Madurez organizacional |

## Riesgos de no implementar la solución
| Riesgo | Impacto | Consecuencia |
|---|---|---|
| Procesos manuales | Alto | Pérdida de trazabilidad y errores |
| Roles indefinidos | Crítico | Accesos indebidos y escalada de privilegios |
| Biometría insegura | Crítico | Suplantación de identidad |
| Navegación móvil deficiente | Medio-Alto | Abandono operativo |
| Reportes no confiables | Alto | Decisiones basadas en datos incompletos |

# Aportes del Análisis en la Creación del Proyecto

El análisis de software transforma necesidades difusas en artefactos accionables y verificables, aportando valor en las siguientes dimensiones:

- **Reducción de ambigüedad:** Desglosa enunciados generales (ej. "gestionar usuarios") en requisitos específicos (registro, roles, autenticación), evitando interpretaciones divergentes.
- **Priorización y Criterios:** Permite distinguir funcionalidades críticas y asociarlas a condiciones objetivas de cumplimiento, facilitando las pruebas y la aceptación.
- **Identificación de Riesgos:** Expone riesgos técnicos y de privacidad tempranamente, reduciendo el costo de corrección.
- **Alineación Diseño-Desarrollo:** Corrige desviaciones entre mockups y el frontend antes de que se conviertan en deuda técnica.
- **Seguridad por Diseño:** Incorpora controles desde la concepción (RBAC server-side, cifrado biométrico, auditoría).
- **Trazabilidad y Mejora:** Vincula hallazgos con requisitos y acciones correctivas (CAPA), fundamental para la auditoría.

| Actividad | Aporte del Análisis | Resultado Esperado |
|---|---|---|
| Levantamiento | Clarifica problema y contexto | Requerimientos precisos |
| Alcance | Delimita lo incluido y excluido | Evita el "scope creep" |
| Diseño de Flujos | Identifica actores y excepciones | Procesos robustos |
| Construcción | Reduce retrabajo por ambigüedad | Mayor productividad |
| Seguridad | Incorpora controles preventivos | Menor exposición al riesgo |
| Operación | Habilita monitoreo y auditoría | Sostenibilidad |

# Quiénes son los más beneficiados

## Beneficiarios principales

| Beneficiario | Interés principal | Beneficio esperado | Evidencia de beneficio |
|---|---|---|---|
| Dirección del colegio | Gobernanza, control y decisiones | Información confiable, reducción de riesgo, visibilidad operativa | Dashboards, reportes, indicadores de asistencia y justificaciones |
| Responsables académicos e instructores | Gestión de fichas, jornadas y asistencia | Ahorro de tiempo, consulta rápida, control de su alcance | Históricos móviles, aprobaciones, reportes por parámetros |
| Personal registrado | Accesos, perfil, asistencia y justificaciones | Experiencia clara, recuperación segura, trazabilidad personal | Inicio de sesión, registro biométrico, notificaciones, historial propio |
| Área administrativa | Usuarios, roles, cursos y parámetros | Administración centralizada y auditada | Gestión web, importación CSV, auditoría de cambios |
| Área de TI y desarrollo | Base técnica para construir y mantener | Requisitos claros, menor ambigüedad, trazabilidad | SRS, matriz de trazabilidad, tickets, pruebas, deploys |
| Área de calidad y auditoría | Verificación y cumplimiento | Evidencia objetiva para revisión | Expedientes CAPA, logs, informes de validación |
| Seguridad y privacidad | Protección de datos e identidades | Controles técnicos y procesales | RBAC, cifrado, MFA, logs, threat model |

## Beneficiarios secundarios

| Beneficiario | Beneficio indirecto |
|---|---|
| Visitantes o personal no registrado | Proceso de ingreso ordenado, temporal y supervisado |
| Familias o acudientes, si aplica | Mayor confianza en controles de asistencia y seguridad |
| Proveedores tecnológicos | Especificaciones claras para integración o soporte |
| Comunidad educativa | Mejora general de eficiencia, transparencia y orden institucional |
| Futuros proyectos organizacionales | Base documental reutilizable para escalamiento |

## Análisis de valor por beneficiario

| Beneficiario | Valor instrumental | Valor estratégico | Valor de confianza |
|---|---|---|---|
| Dirección | Reportes y control | Toma de decisiones basada en datos | Transparencia institucional |
| Instructores | Ahorro de tiempo operativo | Mejor seguimiento académico | Reducción de carga administrativa |
| Personal registrado | Autogestión de perfil y asistencia | Experiencia digital consistente | Seguridad de credenciales |
| TI y desarrollo | Requisitos y criterios claros | Menor retrabajo | Trazabilidad técnica |
| Calidad | Evidencia verificable | Mejora continua | Auditoría sostenible |
| Seguridad | Controles implementados | Prevención de incidentes | Protección de datos |

# Partes interesadas y responsabilidades

## Mapa de partes interesadas

| Parte interesada | Rol en el análisis | Aporte esperado | Nivel de influencia |
|---|---|---|---|
| Patrocinador del proyecto | Aprueba visión y recursos | Priorización estratégica | Alta |
| Product Owner o líder de negocio | Define necesidades y acepta entregables | Reglas de negocio | Alta |
| Usuarios clave | Validan flujos operativos | Retroalimentación de usabilidad | Media-Alta |
| Equipo de desarrollo | Analiza viabilidad técnica | Estimaciones y diseño | Alta |
| QA | Deriva casos de prueba | Criterios de verificación | Alta |
| Seguridad | Evalúa riesgos y controles | Ameazas y mitigaciones | Alta |
| Privacidad o cumplimiento | Revisa datos personales y biométricos | Restricciones legales | Alta |
| DevOps o infraestructura | Define despliegue y monitoreo | Operabilidad | Media |
| Auditoría interna | Revisa trazabilidad | Conformidad | Media |

## Modelo RACI mínimo para el análisis

| Actividad | Patrocinador | Product Owner | Analista | Desarrollo | QA | Seguridad | Privacidad |
|---|---:|---:|---:|---:|---:|---:|---:|
| Validar problema y objetivos | A | R | R | C | C | C | C |
| Definir alcance | I | A | R | C | C | C | C |
| Identificar stakeholders | I | A | R | C | I | C | C |
| Analizar riesgos | I | C | R | C | C | A | C |
| Revisar requisitos funcionales | I | A | R | R | R | C | C |
| Validar restricciones de privacidad | I | C | C | I | I | C | A |
| Aprobar documento de análisis | A | R | R | C | C | C | C |

Notas:

- **R**: responsable de ejecutar.
- **A**: aprobador o accountable.
- **C**: consultado.
- **I**: informado.

# Metodología de análisis aplicada

## Recopilación de información

Se emplearon técnicas complementarias:

- Revisión de la lista de requerimientos funcionales proporcionada.
- Análisis de hallazgos operativos y de desarrollo.
- Revisión de mockups, pantallas móviles y avances web.
- Identificación de actores y procesos críticos.
- Aplicación de criterios de calidad de software y gestión de riesgos.

## Análisis de procesos

Se modeló de forma narrativa la secuencia operativa esperada:

1. Registrar usuarios y asignar roles.
2. Configurar ambientes, fichas, responsables y jornadas.
3. Autenticar usuarios en web o móvil.
4. Registrar entradas y salidas por biometría o método alterno.
5. Controlar ingresos de personal no registrado.
6. Parametrizar reglas de retardos y justificaciones.
7. Consultar históricos y auditoría.
8. Cargar, aprobar o rechazar justificaciones.
9. Generar reportes y dashboards.
10. Administrar cursos y planes de estudio.
11. Monitorear alertas IoT y aplicar acciones correctivas o preventivas.

## Análisis de requisitos

Los requisitos funcionales se clasificaron por módulo y se vincularon con dependencias, plataformas, riesgos y criterios de aceptación. Este análisis alimenta directamente el Informe de Especificación de Requisitos.

## Análisis de datos

Se identificaron entidades principales y sus relaciones básicas.

| Entidad | Descripción | Relaciones relevantes | Regla clave |
|---|---|---|---|
| Usuario | Persona con identidad digital en el sistema | Tiene roles, permisos, asistencias, justificaciones | Documento o correo único según política |
| Rol | Agrupación de responsabilidades | Contiene permisos, se asigna a usuarios | Debe estar auditado y versionado si aplica |
| Permiso | Capacidad técnica para ejecutar acción | Pertenece a roles, se evalúa en API | Denegación por defecto |
| Ambiente | Espacio físico o lógico | Se asocia a fichas, jornadas, responsables | Código único |
| Ficha | Grupo académico u operativo | Pertenece a curso, tiene responsables y jornadas | Única por periodo |
| Jornada | Franja horaria o turno | Se asocia a ambiente, ficha y responsable | Debe validar zona horaria |
| Responsable | Persona autorizada sobre fichas o jornadas | Vinculado a usuarios y roles | Solo usuarios activos |
| Registro de asistencia | Evento de entrada o salida | Asociado a usuario, método, dispositivo, fecha | Idempotente y auditado |
| Justificación | Soporte de irregularidad | Vinculada a registro de asistencia | Requiere aprobación o rechazo |
| Soporte | Archivo adjunto a justificación | Pertenece a justificación | Validar tipo, tamaño y malware |
| Curso | Catálogo académico | Tiene fichas y planes de estudio | Código único |
| Plan de estudios | Estructura de materias | Pertenece a curso, se asocia a fichas | Versionado |
| Dispositivo IoT | Elemento conectado | Genera alertas y telemetría | Autenticación única |
| Auditoría | Registro de cambios | Aplica a entidades críticas | Inmutable o append-only |

## Análisis de viabilidad

| Tipo de viabilidad | Evaluación | Condición necesaria |
|---|---|---|
| Técnica | Viable con arquitectura modular, API segura, móvil compatible y servicios biométricos controlados | Definir proveedores biométricos, soporte iOS/Android y estrategia offline |
| Operacional | Viable si existe capacitación, adopción y soporte | Gestión del cambio y roles claros |
| Económica | Positivamente justificable si reduce errores, tiempo administrativo y riesgo | Presupuesto para desarrollo, licencias, infraestructura y seguridad |
| Legal y privacidad | Condicionada al tratamiento adecuado de datos personales y biométricos | Consentimiento, retención, cifrado y revisión jurídica |
| Cronológica | Viable mediante entregas incrementales | Priorizar módulos críticos primero |
| Organizacional | Viable si hay patrocinio y compromiso de usuarios clave | Comité de proyecto y gobernanza |

## Análisis de riesgos

| Riesgo | Causa probable | Impacto | Mitigación |
|---|---|---|---|
| Falso negativo biométrico | Iluminación, lesiones, sensor sucio | Negación de servicio | Método alterno, soporte operativo, umbrales configurables |
| Suplantación facial | Foto, pantalla, ataque de presentación | Acceso indebido | Liveness, auditoría, MFA, monitoreo |
| Escalada de privilegios | Roles mal configurados | Compromiso administrativo | RBAC server-side, segregación de funciones |
| Exposición de datos CSV | Importación o exportación insegura | Violación de privacidad | Sanitización, cifrado, control de acceso |
| Errores de navegación móvil | Diseño inconsistente o defectos | Abandono operativo | Pruebas UX, mapa de navegación, correcciones CAPA |
| Mockup desactualizado | Falta de gobernanza de diseño | Retrabajo | Línea base aprobada, comparativo diseño-desarrollo |
| Fallos IoT no detectados | Heartbeat insuficiente | Operación ciega | Alertas, escalamiento, acknowledgment |
| Cambios retroactivos de parámetros | Mala versionamiento | Inconsistencia histórica | Vigencias, cierre de periodos, auditoría |
| Pérdida de conexión móvil | Redes inestables | Pérdida de registro | Cola local segura, sincronización idempotente |
| Fatiga de alertas | Muchas falsas alarmas | Ignorar eventos críticos | Supresión, umbrales, clasificación por severidad |

## Análisis de calidad de software

Se utilizó como referencia el modelo de calidad de sistema y software (International Organization for Standardization & International Electrotechnical Commission [ISO & IEC], 2011).

| Característica | Relevancia en el proyecto | Requisito derivado |
|---|---|---|
| Adecuación funcional | Debe cubrir gestión de usuarios, asistencia, justificaciones y reportes | Cada ERF con criterios de aceptación |
| Eficiencia de desempeño | Registro móvil y consultas no deben degradar operación | Tiempos objetivo y paginación |
| Compatibilidad | Web, móvil, IoT y servicios biométricos | Interfaces estables y versionadas |
| Usabilidad | Crítica en móvil y flujos de recuperación | Menos pasos, mensajes claros, navegación coherente |
| Confiabilidad | Asistencia y auditoría no pueden fallar silenciosamente | Idempotencia, reintentos, monitoreo |
| Seguridad | Datos personales, biométricos y roles sensibles | RBAC, cifrado, MFA, auditoría |
| Mantenibilidad | Sistema destinado a evolucionar | Modularidad, documentación, pruebas |
| Portabilidad | Soporte en distintos dispositivos móviles | Versiones mínimas y abstracción de plataforma |

# Resultados del análisis

## Hallazgos clave

1. El proyecto es necesario y estratégicamente pertinente, porque ataca problemas de control, seguridad, trazabilidad y eficiencia administrativa.
2. La gestión de roles y permisos debe tratarse como requisito crítico, no como funcionalidad secundaria.
3. El registro biométrico exige controles de privacidad, liveness, métodos alternos y auditoría robusta.
4. La aplicación móvil requiere corrección prioritaria de navegación, campos de fecha y alineación con mockups.
5. La seguridad debe incluir preguntas de recuperación, protocolos antifalsificación, protección de sesiones y validación server-side.
6. Los reportes y dashboards deben respetar alcance por responsable para evitar filtración de información.
7. La parametrización de retardos y justificaciones debe versionarse para no corromper históricos.
8. La importación CSV representa riesgo de seguridad y privacidad si no se sanitiza y controla adecuadamente.
9. El análisis debe conectar con acciones correctivas, preventivas y de mejoramiento para cerrar el ciclo de calidad.
10. La documentación analítica reduce ambigüedad y sirve como base contractual, técnica y de auditoría.

## Conclusiones preliminares

- El sistema es viable técnica y operacionalmente, siempre que se gestionen riesgos de seguridad, privacidad, usabilidad móvil e integración biométrica.
- El mayor valor del proyecto no está solo en automatizar, sino en institucionalizar trazabilidad, control y mejora continua.
- Los beneficiarios principales son dirección, responsables académicos, personal registrado, áreas administrativas, TI, calidad y seguridad.
- El análisis realizado justifica continuar hacia la especificación formal de requisitos, el diseño arquitectónico y la construcción incremental.

## Recomendaciones

1. Aprobar este documento como base del análisis antes de congelar alcance.
2. Priorizar implementación de roles, permisos, autenticación segura y registro de asistencia.
3. Corregir defectos móviles críticos: fechas, navegación y coherencia con mockup.
4. Implementar preguntas de seguridad y protocolos antifalsificación con enfoque de riesgo.
5. Definir política de datos biométricos con área legal o de cumplimiento.
6. Establecer matriz de trazabilidad entre RF, ERF, casos de prueba, tickets y evidencia de despliegue.
7. Crear expediente CAPA para cada hallazgo relevante.
8. Realizar validación con usuarios clave antes de producción.
9. Diseñar entregas incrementales con métricas de adopción y calidad.
10. Programar revisión periódica del análisis y de los requisitos derivados.

# Criterios de éxito

| Criterio | Meta sugerida | Forma de medición |
|---|---|---|
| Cobertura de análisis | 100% de RF y ERF documentados con propósito, reglas y criterios | Revisión documental |
| Riesgos críticos mitigados | 100% con plan de mitigación aprobado | Registro de riesgos |
| Ambigüedad reducida | Cero requisitos sin criterio de aceptación verificable | Revisión QA y negocio |
| Corrección móvil | Defectos de fecha y navegación cerrados con evidencia | Tickets y pruebas |
| Seguridad | Controles RBAC, autenticación y antifalsificación implementados | Pruebas de seguridad |
| Adopción usuaria | Satisfacción mínima 4 de 5 en UAT | Encuesta o validación |
| Trazabilidad | Cada cambio técnico vinculado a requisito y evidencia | Repositorio y pipeline |
| Calidad de datos | Reportes consistentes con históricos y auditoría | Comparativo de pruebas |
| Mejora continua | Acciones CAPA eficaces verificadas | Validación de eficacia |

# Limitaciones y supuestos

## Supuestos

- Los requerimientos funcionales proporcionados representan una base válida del negocio.
- Existe disposición de partes interesadas para validar flujos y criterios de aceptación.
- Los dispositivos móviles soportan cámara y, opcionalmente, sensor de huella.
- Se cuenta con infraestructura mínima para web, API, base de datos y notificaciones.
- Los datos biométricos podrán tratarse conforme a política de privacidad aprobada.
- Los dispositivos IoT pueden emitir telemetría o señales de fallo.

## Limitaciones

- Este análisis no incluye diseño arquitectónico detallado.
- No define modelo físico completo de base de datos.
- No constituye opinión jurídica vinculante sobre protección de datos.
- No estima costos definitivos ni cronograma contractual.
- No valida tecnológicamente proveedores biométricos específicos.
- No sustituye pruebas de seguridad externas ni auditoría forense.

# Trazabilidad con otros artefactos

| Artefacto | Relación con este documento | Uso esperado |
|---|---|---|
| Informe de Especificación de Requisitos | Deriva de los objetivos, alcance y hallazgos analizados | Convertir análisis en requisitos verificables |
| Tabla de acciones correctivas, preventivas y de mejoramiento | Recibe hallazgos y riesgos identificados | Gestionar mejora continua |
| Plan de pruebas | Usa criterios de aceptación y casos borde | Verificar cumplimiento |
| Registro de riesgos | Alimenta mitigaciones y seguimiento | Gobernanza de proyecto |
| Matriz RBAC | Deriva de necesidad de roles y permisos | Control de acceso |
| Política de privacidad | Deriva de tratamiento de datos biométricos y personales | Cumplimiento |
| Release notes y backlog | Convierten análisis en entregables priorizados | Ejecución ágil |

# Análisis de Valor Esperado

El sistema genera valor en cuatro dimensiones principales:

1. **Operativo:** Reduce el tiempo de registro y consolidación de información, disminuyendo la fricción diaria y los errores manuales.
2. **Riesgo:** Previene accesos indebidos, suplantaciones y pérdida de trazabilidad. El valor es preventivo y se evidencia en la reducción de incidentes.
3. **Decisorio:** Permite pasar de decisiones intuitivas a decisiones basadas en datos mediante dashboards y reportes analíticos.
4. **Institucional:** Fortalece la madurez organizacional al institucionalizar la calidad de software y la trazabilidad como prácticas recurrentes.

# Enfoque recomendado para la siguiente fase

La siguiente fase debe ser la especificación formal de requisitos, ya iniciada en el documento Informe de Especificación de Requisitos. Para mantener coherencia, se recomienda:

1. Congelar alcance mínimo viable basado en riesgos críticos.
2. Priorizar RF 1, RF 3, RF 4.7, RF 5 y RF 6 como núcleo operativo.
3. Tratar RF 7 como capa analítica dependiente de calidad de datos.
4. Implementar RF 8 como catálogo maestro necesario para fichas y planes.
5. Vincular cada ERF con casos de prueba, responsable, fecha compromiso y evidencia.
6. Abrir expediente CAPA para hallazgos móviles, de seguridad y de diseño.
7. Validar con usuarios clave antes de despliegue masivo.

# Consideraciones éticas, de privacidad y responsabilidad

El uso de biometría, datos personales y controles de acceso exige un enfoque ético y legal. El sistema no debe implementarse como mecanismo de vigilancia desproporcionada ni como herramienta de sanción arbitraria. Debe respetar principios de legitimidad, finalidad, minimización, exactitud, seguridad, transparencia y responsabilidad proactiva.

En particular:

- La biometría debe usarse solo cuando exista necesidad real y base jurídica adecuada.
- Debe informarse claramente a los usuarios sobre el tratamiento de sus datos.
- Deben existir alternativas razonables cuando la biometría falle o no sea deseable.
- Los registros deben conservarse por periodos definidos y eliminarse o anonimizarse cuando corresponda.
- Los accesos a información sensible deben auditarse y limitarse por rol.
- Las alertas y reportes no deben utilizarse para discriminación o control abusivo.

# Glossary operativo

| Término | Definición |
|---|---|
| Análisis de software | Proceso de comprensión del problema, contexto, requisitos, riesgos y criterios de éxito antes o durante el desarrollo |
| Alcance | Delimitación de lo incluido, excluido y limitado en el proyecto |
| Beneficiario | Persona, área o sistema que recibe valor directo o indirecto del proyecto |
| Finalidad | Propósito estratégico de largo plazo que justifica la existencia del proyecto |
| Justificación | Razón de ser del proyecto frente a un problema, riesgo u oportunidad |
| Utilidad | Aplicación práctica e inmediata del sistema para resolver necesidades |
| RBAC | Control de acceso basado en roles |
| CAPA | Acciones correctivas, preventivas y de mejoramiento |
| Stakeholder | Parte interesada con influencia o afectación en el proyecto |
| Trazabilidad | Capacidad de vincular requisitos, decisiones, evidencias y resultados |

# Conclusiones

La Documentación de Análisis del software cumple una función estructural en el ciclo de vida del proyecto: transforma una necesidad operativa en un marco comprensible, priorizado, verificable y gobernable. En este caso, el análisis confirma que el sistema es pertinente, viable y de alto valor, siempre que se gestionen adecuadamente la seguridad, la privacidad, la usabilidad móvil, la trazabilidad y la calidad de datos.

Los objetivos del proyecto se orientan a digitalizar, controlar y auditar procesos críticos de gestión académica y administrativa. El alcance queda delimitado por módulos funcionales claros, exclusiones explícitas y límites analíticos. El proyecto se hizo porque existía necesidad de reducir errores, fortalecer seguridad y mejorar decisiones; sirve para automatizar procesos, proveer trazabilidad y habilitar reportes; y tiene la finalidad estratégica de institucionalizar calidad, confianza y mejora continua.

Asimismo, el análisis ayuda durante la creación del proyecto al reducir ambigüedad, priorizar esfuerzos, prevenir riesgos, alinear stakeholders, fundamentar pruebas y sostener la trazabilidad documental. Los principales beneficiarios son la dirección del colegio, los responsables académicos, el personal registrado, las áreas administrativas, TI, calidad, seguridad y privacidad, sin dejar de lado beneficios indirectos para visitantes, comunidad educativa y futuros escalamientos del sistema.

Este documento debe considerarse una base viva. Su valor aumenta cuando se conecta con la especificación de requisitos, el backlog, los casos de prueba, el registro de riesgos y las acciones correctivas, preventivas y de mejoramiento. De ese modo, el análisis no queda como un artefacto estático, sino como un mecanismo de ingeniería y gobernanza para construir software robusto, seguro y útil.

**Nota del autor.** Jonas es el autor y responsable técnico del documento. La correspondencia relacionada con este análisis puede dirigirse a jonas@consultoria.example. El autor declara la ausencia de conflictos de interés y de financiación externa para la elaboración de este documento.

# Referencias

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). https://doi.org/10.1037/0000165-000

International Organization for Standardization. (2015). *Quality management systems — Requirements* (ISO 9001:2015). https://www.iso.org/standard/62085.html

International Organization for Standardization & International Electrotechnical Commission. (2011). *Systems and software engineering — Systems and software quality requirements and evaluation (SQuaRE) — System and software quality models* (ISO/IEC 25010:2011). https://www.iso.org/standard/35733.html

International Organization for Standardization, International Electrotechnical Commission, & Institute of Electrical and Electronics Engineers. (2017). *Systems and software engineering — Software life cycle processes* (ISO/IEC/IEEE 12207:2017). https://www.iso.org/standard/63712.html

International Organization for Standardization, International Electrotechnical Commission, & Institute of Electrical and Electronics Engineers. (2018). *Systems and software engineering — Life cycle processes — Requirements engineering* (ISO/IEC/IEEE 29148:2018). https://www.iso.org/standard/72089.html

OWASP Foundation. (2021). *OWASP Application Security Verification Standard 4.0*. https://owasp.org/www-project-application-security-verification-standard/