---
title: "Documentación de Acciones Correctivas, Preventivas y de Mejoramiento del Software"
author: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
date: "10 de octubre de 2026"
abstract: |
  El presente documento define el marco documental para la gestión de acciones correctivas, preventivas y de mejoramiento en el ciclo de vida del software. Establece principios, roles, flujo de trabajo, criterios de riesgo, evidencias, indicadores y mecanismos de cierre basados en la mejora continua. Su propósito es garantizar trazabilidad, eficacia verificable y alineación con normas de gestión de calidad, ingeniería de sistemas y criterios de presentación documental profesional.
keywords:
  - acciones correctivas
  - acciones preventivas
  - mejora continua
  - calidad de software
---

# Introducción

La gestión de la calidad del software no puede limitarse a la detección de defectos, incidentes o hallazgos de auditoría. Requiere un mecanismo estructurado para transformar esos eventos en acciones verificables, trazables y efectivas. En este contexto, las acciones correctivas, preventivas y de mejoramiento constituyen un ciclo de retroalimentación técnica y organizacional que permite reducir la variabilidad, mejorar la confiabilidad del producto y fortalecer la madurez de los procesos de desarrollo.

El presente documento se apoya en la norma de sistemas de gestión de la calidad (International Organization for Standardization [ISO], 2015), en los procesos de ciclo de vida de sistemas y software (International Organization for Standardization & International Electrotechnical Commission [ISO & IEC], 2017), en la gestión de riesgos (ISO, 2018) y en el modelo de calidad de sistema y software (ISO & IEC, 2011). Asimismo, su estructura documental busca alinearse con criterios de presentación profesional y académica reconocidos internacionalmente, incluyendo los principios de claridad, jerarquía de encabezados, citación autor-fecha y listado de referencias propios de la séptima edición de las Normas APA (American Psychological Association [APA], 2020).

Desde la perspectiva de ingeniería de software, una acción correctiva no debe confundirse con una corrección superficial. La corrección elimina el efecto visible de un problema, mientras que la acción correctiva ataca la causa raíz para evitar recurrencia. De manera análoga, la acción preventiva no espera a que el fallo se materialice, sino que actúa sobre riesgos identificados, tendencias débiles, cuasiincidentes u oportunidades de fortalecimiento controlado. La acción de mejoramiento, por su parte, no necesariamente responde a una no conformidad, sino a la intención deliberada de incrementar eficacia, eficiencia, mantenibilidad, seguridad o experiencia de usuario.

## Objetivo

Establecer el marco documental, los criterios operativos y las responsabilidades asociadas a la identificación, registro, análisis, implementación, verificación, validación y cierre de acciones correctivas, preventivas y de mejoramiento aplicables al ciclo de vida del software.

## Alcance

Este documento aplica a todos los productos de software desarrollados, mantenidos, integrados u operados por la organización, incluyendo aplicaciones web, servicios backend, bibliotecas internas, componentes embebidos, pipelines de integración y despliegue continuos, infraestructura como código, documentação técnica, procesos de garantía de calidad y actividades de soporte postdespliegue.

Aplica igualmente a hallazgos provenientes de auditorías internas, revisiones de código, pruebas funcionales y no funcionales, incidentes de producción, quejas de usuarios, monitoreo de servicio, análisis de deuda técnica, evaluaciones de seguridad, revisiones de arquitectura y oportunidades de mejora identificadas por equipos multidisciplinarios.

No sustituye los procedimientos específicos de respuesta a incidentes de seguridad, continuidad operativa o manejo de datos personales; sin embargo, debe integrarse con ellos cuando un evento requiera análisis de causa raíz y acciones sistémicas.

## Definiciones

Para efectos de este documento, se adoptan las siguientes definiciones operativas:

- **Acción correctiva**: acción emprendida para eliminar la causa raíz de una no conformidad detectada y evitar su recurrencia.
- **Acción preventiva**: acción emprendida para eliminar la causa de una no conformidad potencial, con el fin de impedir su ocurrencia.
- **Acción de mejoramiento**: acción orientada a incrementar la eficacia, eficiencia, calidad, seguridad, mantenibilidad o valor percrito del producto o proceso, sin que exista necesariamente una no conformidad activa.
- **No conformidad**: incumplimiento de un requisito especificado, ya sea funcional, regulatorio, de proceso, de seguridad, de rendimiento o de calidad.
- **Riesgo**: efecto de la incertidumbre sobre los objetivos del proyecto, producto o servicio, expresado en términos de probabilidad e impacto.
- **Oportunidad**: riesgo con efecto positivo, susceptible de ser aprovechado para mejorar desempeño, reducir costo, aumentar confiabilidad o fortalecer la experiencia del usuario.
- **Causa raíz**: factor fundamental cuya eliminación impide la repetición del problema. No debe confundirse con el síntoma, el evento detonante o la responsabilidad individual.
- **Eficacia**: grado en que la acción implementada logra el resultado previsto, medido mediante evidencias objetivas y criterios definidos previamente.
- **Verificación**: confirmación de que la acción fue implementada conforme al plan aprobado.
- **Validación**: confirmación de que la acción implementada produce el efecto deseado en el contexto real de uso.
- **Registro**: documento, dato o evidencia que conserva historia del proceso y permite reconstruir decisiones, responsables, fechas y resultados.

# Marco normativo y conceptual

## Gestión de calidad

La ISO 9001:2015 establece que la organización debe determinar y actuar sobre no conformidades, evaluar la necesidad de actuar para eliminar causas y evitar recurrencias, además de mantener información documentada como evidencia de la naturaleza de las no conformidades y de los resultados de las acciones tomadas (ISO, 2015, cláusula 10.2). Igualmente, promueve la mejora continua del desempeño mediante el análisis de datos, la evaluación de riesgos y oportunidades, y la revisión por la dirección (ISO, 2015, cláusula 10.3).

Un punto relevante para organizaciones que migraron desde versiones anteriores de ISO 9001 es que la edición 2015 ya no exige una cláusula independiente denominada “acción preventiva”. En su lugar, incorpora el pensamiento basado en riesgos como principio transversal. No obstante, en ingeniería de software mantiene plena validez operacional distinguir entre acciones reactivas, proactivas y de mejora, porque los tipos de evidencia, temporización y criterios de eficacia difieren significativamente.

## Ciclo de vida del software

La norma ISO/IEC/IEEE 12207:2017 describe procesos para la definición, implementación, mantenimiento y mejora de ciclos de vida de sistemas y software (ISO & IEC, 2017). Desde esta perspectiva, las acciones correctivas, preventivas y de mejoramiento no pertenecen exclusivamente a la fase de pruebas o operación, sino que deben integrarse en requisitos, diseño, implementación, verificación, validación, despliegue, soporte y retiro.

En entornos ágiles, DevOps o DevSecOps, este integración implica que cada acción debe estar vinculada a artefactos vivos: backlog, historias de usuario, tareas técnicas, pull requests, pipelines, métricas de servicio, dashboards de observabilidad y repositorios documentales. La trazabilidad no debe depender únicamente de documentos estáticos, sino de enlaces verificables entre evidencia técnica y decisión de negocio.

## Riesgos y oportunidades

La gestión de acciones preventivas y de mejoramiento debe articularse con ISO 31000:2018, que proporciona principios y directrices para integrar la gestión de riesgos en la gobernanza, planificación, operaciones y reporte de la organización (ISO, 2018). En software, esto significa tratar riesgos técnicos, operacionales, de seguridad, de cumplimiento, de reputación y de continuidad como insumos legítimos para disparar acciones antes de que se materialicen como incidentes.

## Calidad de producto y en uso

El modelo ISO/IEC 25010:2011 permite clasificar características de calidad como adecuación funcional, eficiencia de desempeño, compatibilidad, usabilidad, confiabilidad, seguridad, mantenibilidad y portabilidad, así como calidad en uso (ISO & IEC, 2011). Las acciones documentadas en este marco deben poder mapearse a dichas características, de modo que la organización no solo cierre hallazgos, sino que demuestre mejora medible en atributos relevantes del producto.

## Presentación documental

| Tipo de acción | Origen / hallazgo detectado | Propósito central | Evidencia mínima y acción verificable |
|---|---|---|---|
| Mejoramiento | Poco avance del aplicativo web y móvil respecto a lo planificado para el proyecto. | Realizar avances sustanciales en próximas revisiones, priorizando funcionalidades críticas, pantallas faltantes y construcción del producto mínimo viable. | Backlog priorizado, sprint planning, sprint review, métricas de avance, release notes, capturas de nuevas pantallas implementadas y evidencia de despliegue o build. |
| Mejoramiento | Diferencias entre la base de datos, el frontend desarrollado, los mockups existentes y la identidad visual aplicada, incluyendo colores sólidos sin degradado y pantallas no representadas en diseño. | Corregir la inconsistencia entre modelo de datos, interfaz gráfica y mockups, mejorando la coherencia visual, la trazabilidad de diseño y la correcta implementación del aplicativo web y móvil. | Comparativo base de datos/frontend/mockup, guía de estilos actualizada, archivos de diseño versionados en Figma o herramienta equivalente, pull requests de ajuste visual, evidencia de revisión UX/UI y documento de trazabilidad de pantallas. |
| Correctiva | Deficiencia en el flujo de “Olvido de contraseña”, incluyendo vistas, validaciones, navegación o experiencia de usuario inadecuadas. | Corregir el proceso de recuperación de contraseña para garantizar un flujo claro, seguro, funcional y consistente entre aplicativo web y móvil. | Ticket de defecto, video o captura del error, casos de prueba funcionales y de seguridad, pull request asociado, pruebas de regresión, evidencia de despliegue y métrica de abandono o éxito del flujo antes/después. |
| Correctiva | Los campos de fecha no permiten selección correcta o presentan error en la aplicación móvil. | Restaurar la funcionalidad de selección de fechas en dispositivos móviles, asegurando compatibilidad con iOS/Android, validación adecuada y experiencia de usuario correcta. | Reporte de bug móvil, captura o video del error, commit o pull request corregido, pruebas manuales o automatizadas en emuladores/dispositivos reales, evidencia de build móvil y registro de cierre verificado. |
| Correctiva | Problemas de navegación en la aplicación móvil, como rutas confusas, botones que no funcionan, falta de retorno o flujos interrumpidos. | Corregir la navegación móvil para garantizar desplazamiento consistente, accesible y sin bloqueos entre pantallas principales y secundarias. | Mapa de navegación actualizado, lista de defects de navegación, pull request corregido, casos de prueba de usabilidad, evidencia de pruebas en dispositivo móvil, registro de despliegue y acta de verificación. |
| Preventiva | Falta agregar la sección de preguntas de seguridad en el módulo de cuenta, recuperación o verificación de identidad. | Implementar un control adicional de autenticación que reduzca el riesgo de acceso no autorizado, suplantación de identidad o recuperación indebida de cuentas. | Requerimiento de seguridad aprobado, diseño de interfaz y API, matriz de riesgos actualizada, almacenamiento seguro de respuestas hasheadas o cifradas, límites de intentos, logs de auditoría, casos de prueba y evidencia de revisión de seguridad. |
| Preventiva | Necesidad de revisar protocolos de seguridad para evitar falsificación, suplantación, manipulación de solicitudes o acceso indebido. | Definir e implementar controles técnicos y procesales que prevengan falsificación de identidades, tokens, peticiones, sesiones o información sensible. | Threat model o análisis de amenazas, checklist OWASP ASVS o similar, configuración de HTTPS/HSTS, tokens firmados y con expiración, protección CSRF, rate limiting, validación de entrada, pruebas de seguridad, informe de revisión y evidencia de mitigación. |
| Correctiva | Solo se visualiza el rol administrador; faltan los demás roles definidos para el sistema. | Implementar y verificar la gestión correcta de roles y permisos según la matriz RBAC aprobada, asegurando que cada perfil acceda únicamente a sus funcionalidades permitidas. | Matriz RBAC actualizada, datos semilla o migraciones de roles, pull request implementado, pruebas de autorización por rol, capturas de interfaz por perfil, evidencia de despliegue y registro de validación de permisos. |
| Correctiva | El mockup móvil no concuerda con el avance real del desarrollo. | Restablecer la trazabilidad entre diseño aprobado e implementación móvil, corrigiendo ya sea el desarrollo, el mockup o ambos, según la decisión técnica y funcional aprobada. | Comparativo mockup/aplicación móvil, acta de decisión sobre línea base de diseño, Figma o mockup actualizado, backlog corregido, pull request de alineación, evidencia de revisión UX/UI y registro de cierre. |

# Proceso de documentación y gestión de acciones

El proceso recomendado consta de nueve etapas secuenciales, con retroalimentación en caso de ineficacia. Cada etapa debe generar evidencia objetiva y trazable.

## Identificación y registro

Toda acción debe iniciarse con un registro único. El identificador debe ser persistente, legible por humanos y preferiblemente correlacionable con sistemas de gestión de incidencias, repositorio, pipeline o herramienta de calidad.

Campos mínimos del registro:

- Identificador único de la acción.
- Fecha de detección.
- Fecha de registro.
- Solicitante o detector.
- Producto, servicio, módulo o componente afectado.
- Versión o release afectada.
- Entorno: desarrollo, pruebas, preproducción, producción.
- Descripción objetiva del hecho observado.
- Impacto inicial estimado.
- Urgencia percibida.
- Clasificación preliminar: correctiva, preventiva o de mejoramiento.
- Enlaces a evidencia: logs, capturas, informes de prueba, tickets, auditorías, métricas.
- Responsable asignado.
- Estado inicial: abierta.

La descripción debe ser fáctica. Debe evitarse lenguaje acusatorio, suposiciones no verificadas o conclusiones anticipadas sobre culpabilidad. El foco está en el sistema, el proceso y la evidencia, no en personas.

## Triaje y clasificación

El triaje determina si el evento requiere una acción formal o si puede gestionarse como tarea ordinaria. Criterios para escalar a acción formal incluyen:

- Impacto en usuarios externos.
- Riesgo de seguridad o privacidad.
- Incumplimiento regulatorio o contractual.
- Afectación a disponibilidad, integridad o confidencialidad.
- Recurrencia histórica.
- Costo elevado de corrección diferida.
- Detección tardía respecto de la fase óptima de prevención.
- Necesidad de cambio en arquitectura, proceso o gobernanza.

Durante el triaje se confirma la clasificación provisional y se asigna prioridad. Si un mismo evento contiene múltiples causas o efectos, puede desglosarse en varias acciones vinculadas, manteniendo trazabilidad padre-hijo.

## Análisis de causa raíz

El análisis de causa raíz es obligatorio para acciones correctivas y recomendable para preventivas de alto impacto. No basta con describir el síntoma. Debe identificarse el factor sistémico que permitió la ocurrencia o la no detección temprana.

Métodos admisibles:

- Los cinco porqués.
- Diagrama de Ishikawa o espina de pescado.
- Análisis de barreras.
- Árbol de fallas.
- Análisis de cambios.
- Revisión de línea de tiempo.
- Análisis de causa raíz estructurado para incidentes mayores.

Dimensiones mínimas a examinar en software:

- Requisitos ambiguos, incompletos o cambiantes.
- Diseño arquitectónico insuficiente.
- Implementación defectuosa.
- Cobertura de pruebas inadecuada.
- Datos de prueba no representativos.
- Configuración ambiental inconsistente.
- Gestión de dependencias vulnerable.
- Control de cambios deficiente.
- Observabilidad limitada.
- Factores humanos: fatiga, carga cognitiva, comunicación, capacitación.
- Procesos organizacionales: presión de plazo, falta de revisión técnica, incentivos equivocados.

El resultado debe incluir:

- Declaración de causa raíz.
- Contribuyentes secundarios.
- Razón por la cual el problema no fue detectado antes.
- Alcance potencial de afectación.
- Nivel de confianza del análisis.
- Evidencias que soportan cada conclusión.

Si no se identifica una causa raíz única, debe documentarse un conjunto de causas contribuyentes y priorizarse aquellas con mayor influencia controlable. Si la causa permanece desconocida tras esfuerzo razonable, deben implementarse controles compensatorios, definirse monitoreo reforzado y reabrirse el análisis si ocurre recurrencia.

## Evaluación de impacto y riesgo

Antes de aprobar el plan, debe evaluarse el riesgo residual. Esta evaluación considera probabilidad, impacto, detectabilidad y velocidad de propagación.

Criterios de evaluación:

- Número de usuarios afectados o potencialmente afectados.
- Criticidad del servicio.
- Exposición de datos personales o sensibles.
- Impacto financiero.
- Impacto reputacional.
- Incumplimiento normativo.
- Dependencias externas afectadas.
- Ventana temporal de exposición.
- Capacidad de recuperación.
- Costo y tiempo de implementación de la acción.

La evaluación debe permitir decidir si se requiere contención inmediata, corrección programada, cambio arquitectónico, actualización documental o revisión por comité.

## Planificación de acciones

El plan debe ser específico, medible, alcanzable, relevante y temporalizado. Cada acción debe tener responsable único, fecha compromiso, criterios de aceptación y evidencia esperada.

Componentes del plan:

- Acciones de contención, si aplican.
- Acciones de corrección del efecto inmediato.
- Acciones sobre causa raíz.
- Acciones preventivas asociadas.
- Acciones de mejoramiento derivadas.
- Actualización de documentación técnica.
- Actualización de casos de prueba.
- Actualización de monitoreo y alertas.
- Capacitación, si cambia el proceso.
- Comunicación a partes interesadas.
- Plan de rollback.
- Estimación de esfuerzo y recursos.
- Dependencias técnicas u organizacionales.
- Criterios de éxito.
- Periodo de validación de eficacia.

Las acciones deben evitar generalidades como “mejorar pruebas” o “capacitar al equipo”. Formulación adecuada: “Agregar suite de pruebas de regressión para el flujo de pago con cobertura de escenarios de timeout, reintentos idempotentes y validación de consistencia contable, antes del despliegue a producción”.

## Implementación controlada

La implementación debe respetar los controles de ingeniería de software de la organización:

- Ramas o commits vinculados al identificador de la acción.
- Revisión de código por pares.
- Ejecución de pipeline de integración continua.
- Pruebas unitarias, de integración, de sistema y, cuando aplique, de aceptación.
- Análisis estático y de seguridad.
- Gestión de artefactos y versiones.
- Despliegue con feature flags, canary release o blue-green, según riesgo.
- Registro de cambios en release notes.
- Actualización de diagramas, APIs, modelos de datos y runbooks.
- Conservación de evidencia de aprobación y ejecución.

En entornos regulados o críticos, la implementación puede requerir aprobación previa de un comité de cambios. En entornos ágiles, puede realizarse mediante pull request con revisores designados y automatización verificable. Lo esencial es que la acción no se pierda en el backlog sin trazabilidad.

## Verificación de implementación

La verificación responde a la pregunta: ¿se hizo lo planeado?

Evidencias típicas:

- Enlace a commit, merge request o pull request.
- Resultado de pipeline.
- Informe de pruebas.
- Registro de despliegue.
- Captura de configuración modificada.
- Documento actualizado.
- Acta de capacitación.
- Cambio en política o procedimiento.

Si la verificación falla, la acción no avanza a validación. Debe corregirse el plan o la ejecución, según el caso.

## Validación de eficacia

La validación responde a la pregunta: ¿la acción produjo el efecto esperado?

Para acciones correctivas, la eficacia se confirma cuando:

- No recurre el mismo modo de falla dentro del periodo definido.
- Los indicadores afectados retornan a umbrales aceptables.
- Las pruebas específicas pasan de forma consistente.
- Los controles implementados son observables y sostenibles.

Para acciones preventivas, la eficacia se confirma cuando:

- El riesgo tratado disminuye a nivel aceptado.
- No se materializan incidentes asociados durante el periodo de monitoreo.
- Los controles preventivos se ejecutan automáticamente o según frecuencia definida.

Para acciones de mejoramiento, la eficacia se confirma cuando:

- Se alcanza la métrica objetivo.
- No se degradan otras características de calidad.
- El beneficio es sostenible operativo y económicamente.

El periodo de validación debe definirse antes del cierre. Puede ser 30, 60, 90 o 180 días, dependiendo de criticidad, frecuencia de ocurrencia y naturaleza del cambio. Si la acción no es eficaz, debe reabrirse, ampliarse el análisis de causa raíz o diseñarse una nueva intervención.

## Cierre y retroalimentación

El cierre requiere aprobación del responsable de calidad o del rol equivalente. El expediente cerrado debe contener:

- Registro original.
- Análisis de causa raíz.
- Evaluación de riesgo.
- Plan aprobado.
- Evidencias de implementación.
- Verificación.
- Validación de eficacia.
- Lecciones aprendidas.
- Actualizaciones derivadas a procesos, estándares, riesgos, pruebas y capacitación.
- Fecha de cierre.
- Firma o registro electrónico del responsable.

El cierre no es un fin administrativo. Debe alimentar el sistema de conocimiento organizacional: bases de datos de defectos, checklists de revisión, reglas de análisis estático, casos de prueba, runbooks, matrices de riesgo y programas de formación.


En organizaciones pequeñas, un mismo rol puede asumir varias responsabilidades, siempre que se preserve segregación mínima entre quien implementa y quien valida eficacia, especialmente en cambios críticos.