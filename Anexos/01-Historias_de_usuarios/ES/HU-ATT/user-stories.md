# HU-ATT-001: Registro de asistencia facial

## Historia

**Como** estudiante/aprendiz  
**Quiero** registrar mi entrada utilizando el reconocimiento facial  
**Para que** mi asistencia se registre de forma rápida, segura y sin depender del llamado manual del instructor.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

- [ ] **AC1:** Dado que el estudiante está registrado, activo, posee un parámetro facial activo y tiene un bloque de formación asignada, cuando el estudiante escanea su rostro durante el período de entrada permitido, entonces el sistema registra la entrada, muestra un mensaje de confirmación y almacena la fecha, hora, bloque de formación y método de registro como “facial”.

- [ ] **AC2:** Dado que el escaneo facial coincide con un usuario registrado, cuando el sistema evalúa la hora de entrada frente al bloque de formación asignada, entonces el registro de asistencia se marca como presente o retardado según las reglas de asistencia configuradas en el sistema.

- [ ] **AC3:** Dado que el estudiante ya posee un registro de entrada para el mismo bloque de formación y fecha, cuando el estudiante intenta registrar la entrada nuevamente, entonces el sistema evita el registro duplicado y muestra un mensaje claro indicando que la entrada ya fue registrada.

- [ ] **AC4:** Dado que el estudiante no está registrado, está inactivo o no posee un parámetro facial activo, cuando el estudiante intenta el registro de asistencia facial, entonces el sistema deniega el registro y muestra un mensaje de error claro.

- [ ] **AC5:** Dado que el rostro no es detectado, no coincide con un usuario registrado o la cámara falla, cuando el escaneo facial falla, entonces el sistema muestra un mensaje de error y ofrece el flujo alternativo correspondiente según RF 3.1.2 o RF 3.2.1.

- [ ] **AC6:** Dadas condiciones normales de red, cámara e iluminación, cuando el estudiante completa el registro de asistencia facial, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

- [ ] **AC7:** Dado que los datos biométricos son procesados, cuando el registro se completa, entonces el sistema registra un evento de auditoría y no expone datos biométricos sensibles en logs, alertas o la interfaz de usuario.

- [ ] **AC8:** Dado que el estudiante sale de la sesión de formación, cuando el estudiante escanea su rostro durante el período de salida permitido, entonces el sistema registra la salida, muestra un mensaje de confirmación y almacena la hora de salida sin duplicar el registro de entrada.

---

## Notas Técnicas

- El reconocimiento facial puede depender de un servicio en la nube externo, según el supuesto 2.4.5 del SRS. Si el servicio externo falla, el sistema debe mostrar un error claro y permitir el flujo alternativo correspondiente.
- El sistema requiere acceso a internet, cámara funcional, iluminación adecuada y posicionamiento correcto del rostro, según las restricciones y supuestos del SRS.
- El sistema debe validar:
  - Que el usuario exista y esté activo.
  - Que el usuario posea un parámetro facial activo.
  - Que el usuario tenga un bloque de formación o horario asignado.
  - Que la hora de entrada o salida sea apropiada para el bloque asignado.
  - Que el usuario no tenga ya un registro de entrada o salida duplicado para el mismo bloque y fecha.
- El registro alternativo para usuarios registrados cuyo escaneo facial falla está cubierto por el RF 3.1.2 y debe estar vinculado desde el estado de fallo.
- Los usuarios no registrados o casuales están cubiertos por el RF 3.2 y RF 3.2.1, pero no deben mezclarse con los registros de asistencia regulares de estudiantes sin una clasificación explícita.
- Los datos biométricos deben tratarse como datos sensibles y deben cumplir con las regulaciones de protección de datos aplicables, como la Ley 1581 de 2012 de Colombia.
- El sistema debe preferir plantillas o embeddings faciales sobre imágenes faciales brutas, a menos que las imágenes brutas sean explícitamente requeridas y aprobadas.
- Los datos sensibles no deben escribirse en logs ni mostrarse en la interfaz.
- El flujo de registro de asistencia debe generar información de auditoría para la trazabilidad.
- Requisito de rendimiento: tiempo de respuesta bajo 5 segundos, según el RNF 1.

**Servicio(s) responsable(s):** Servicio de Asistencia / Servicio de Verificación Facial, propuesto.  
**Endpoint(s) implementados:**  
- `POST /api/v1/attendance/entry/facial` — propuesto.  
- `POST /api/v1/attendance/exit/facial` — propuesto.  
- `POST /api/v1/attendance/entry/alternative` — fallback, puede pertenecer a la HU-ATT-002.  

**Eventos generados:**  
- `AttendanceEntryRegistered`  
- `AttendanceExitRegistered`  
- `LateDetected`  
- `FacialScanFailed`  
- `AlternativeRegistrationRequested`  

**Permisos requeridos:**  
- Estudiante/aprendiz autenticado con cuenta activa.
- El sistema debe validar adicionalmente el parámetro facial activo y el bloque de formación asignada.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.  
> Ver: [`00-governance/definition-of-done.md`](../../../00-governance/definition-of-done.md)

**Verificaciones adicionales específicas para esta HU:**

- [ ] Flujo de éxito de escaneo facial probado con un usuario registrado y activo.
- [ ] Flujo de fallo de escaneo facial probado cuando el rostro no es detectado.
- [ ] Flujo de fallo de escaneo facial probado cuando el rostro no coincide con un usuario registrado.
- [ ] Escenario de cámara no disponible o permiso denegado probado.
- [ ] Prevención de entrada duplicada probada.
- [ ] Prevención de salida duplicada probada.
- [ ] Detección de retardo probada cuando la entrada ocurre después del tiempo permitido.
- [ ] Flujo de registro alternativo accesible cuando el escaneo facial falla.
- [ ] Tiempo de respuesta validado bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Datos biométricos sensibles no expuestos en logs, respuestas o UI.
- [ ] Evento de auditoría generado para intentos de registro exitosos y fallidos.
- [ ] Mensajes de confirmación y error son claros, visibles y comprensibles.
- [ ] Trazabilidad al RF 3.1, RF 3.1.1, CU 1 y CU 2 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 8 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 1 / MVP |
| Dependencias | HU-IAM-001: Inicio de sesión; HU-USR-001: Registro de usuario; HU-ATT-000: Inscripción de parámetro facial; HU-ACAD-001: Bloque de formación/horario asignado |
