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
- `POST /api/v1/attendance/entry/alternative` — fallback, puede pertenecer a la [HU-ATT-002](#hu-att-002-inscripción-inicial-de-parámetros-faciales).  

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
> Ver: [`00-governance/definition-of-done.md`](https://github.com/code-sena/fae-docs/blob/main/00-governance/definition-of-done.md) (repositorio `fae-docs`).

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
| Dependencias | [HU-IAM-001](../HU-IAM/user-stories.md#hu-iam-001-inicio-de-sesión-de-usuario): Inicio de sesión de usuario; [HU-IAM-002](../HU-IAM/user-stories.md#hu-iam-002-registro-de-usuario-con-asignación-de-rol): Registro de usuario con asignación de rol; [HU-ATT-002](#hu-att-002-inscripción-inicial-de-parámetros-faciales): Inscripción inicial de parámetros faciales; [HU-ACAD-005](../HU-ACAD/user-stories.md#hu-acad-005-asignar-bloques-de-tiempojornadas): Asignar bloques de tiempo/jornadas |

---

# HU-ATT-002: Inscripción inicial de parámetros faciales

## Historia

**Como** estudiante/aprendiz registrado  
**Quiero** registrar mi parámetro facial en FaceAttend EDU  
**Para que** pueda registrar posteriormente mi asistencia mediante reconocimiento facial sin depender del llamado manual.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

- [ ] **AC1:** Dado que el usuario está registrado y ha iniciado sesión en FaceAttend EDU, cuando el usuario inicia el proceso de inscripción facial, entonces el sistema activa la cámara y se prepara para la captura del rostro, según el Caso de Uso 4.

- [ ] **AC2:** Dado que la cámara está activa y el usuario posiciona correctamente su rostro, cuando el sistema captura la imagen facial bajo condiciones adecuadas de iluminación y posicionamiento, entonces el sistema procesa la captura y asocia el parámetro facial resultante con el usuario autenticado.

- [ ] **AC3:** Dado que la captura facial es exitosa, cuando el proceso de inscripción finaliza, entonces el sistema almacena el parámetro facial de forma segura, muestra un mensaje de confirmación y registra un evento de auditoría.

- [ ] **AC4:** Dado que el rostro no es detectado, la calidad de la imagen es insuficiente o la cámara falla, cuando el intento de inscripción falla, entonces el sistema muestra un mensaje de error claro y permite al usuario reintentar.

- [ ] **AC5:** Dado que el usuario ya posee un parámetro facial activo, cuando el usuario intenta realizar la inscripción inicial nuevamente, entonces el sistema informa que ya existe un parámetro facial y redirige al usuario al flujo de actualización, a menos que el reemplazo esté explícitamente permitido y confirmado.

- [ ] **AC6:** Dado que los datos biométricos son procesados, cuando el parámetro facial es almacenado, entonces el sistema protege los datos sensibles según el RNF 4 y no expone información biométrica sin procesar en logs ni en los mensajes de la interfaz de usuario.

- [ ] **AC7:** Dadas condiciones normales de dispositivo, cámara y red, cuando el usuario completa la inscripción facial, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

---

## Notas Técnicas

- Esta historia corresponde a la inscripción facial inicial requerida antes de utilizar el registro de asistencia facial de la [HU-ATT-001](#hu-att-001-registro-de-asistencia-facial).
- El SRS indica que el reconocimiento facial puede fallar por la iluminación o el movimiento del usuario, por lo que la interfaz debe proporcionar orientación clara.
- La inscripción facial puede depender de un servicio externo de reconocimiento facial en la nube, según el supuesto 2.4.5 del SRS. Si el servicio externo falla, el sistema debe mostrar un error claro y permitir reintentar.
- El sistema debe almacenar plantillas o parámetros faciales en lugar de imágenes faciales brutas, a menos que las imágenes brutas sean explícitamente requeridas y aprobadas.
- Los datos biométricos deben cumplir con las regulaciones de protección de datos aplicables, como la Ley 1581 de 2012 de Colombia.
- El usuario debe estar autenticado antes de asociar un parámetro facial a su cuenta.
- Los permisos de cámara deben solicitarse y manejarse adecuadamente si son denegados.
- Las actualizaciones del parámetro facial se cubren por separado en la [HU-CONF-006](../HU-CONF/user-stories.md#hu-conf-006-actualizar-parámetros-faciales-del-usuario).
- Los datos biométricos sensibles no deben escribirse en logs ni mostrarse en la interfaz.

**Servicio(s) responsable(s):** Servicio de Asistencia / Servicio de Inscripción Biométrica, propuesto.  
**Endpoint(s) implementados:**  
- `POST /api/v1/biometrics/facial-enrollment` — propuesto.  
- `GET /api/v1/users/me/facial-status` — propuesto.  

**Eventos generados:**  
- `FacialParameterRegistered`  
- `FacialRegistrationFailed`  
- `FacialParameterAlreadyExists`  

**Permisos requeridos:**  
- Estudiante/aprendiz autenticado.  
- El Administrador o Supervisor puede asistir con la inscripción únicamente si lo permite la política institucional.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Inscripción facial exitosa probada.
- [ ] Escenario de rostro no detectado probado.
- [ ] Escenario de mala iluminación o movimiento probado.
- [ ] Escenario de cámara no disponible o permiso denegado probado.
- [ ] Escenario de parámetro facial existente probado.
- [ ] El parámetro facial se asocia correctamente con el usuario autenticado.
- [ ] Los datos biométricos sensibles no se exponen en logs, respuestas ni UI.
- [ ] Se genera un evento de auditoría.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se valida bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Trazabilidad al Caso de Uso 4 y los RF relacionados documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 8 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 1 / MVP |
| Dependencias | [HU-IAM-001](../HU-IAM/user-stories.md#hu-iam-001-inicio-de-sesión-de-usuario): Inicio de sesión de usuario; [HU-IAM-002](../HU-IAM/user-stories.md#hu-iam-002-registro-de-usuario-con-asignación-de-rol): Registro de usuario con asignación de rol |

---

# HU-ATT-003: Registro alternativo para usuarios registrados con fallo en el escaneo facial

## Historia

**Como** estudiante/aprendiz registrado cuyo escaneo facial falla  
**Quiero** completar un proceso de registro alternativo con tiempo limitado  
**Para que** mi asistencia pueda registrarse aun cuando el reconocimiento facial no pueda validarme.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

- [ ] **AC1:** Dado que un usuario registrado intentó el registro de asistencia facial y el escaneo facial falló, cuando el usuario selecciona la opción de registro alternativo, entonces el sistema presenta una encuesta con tiempo limitado que solicita los datos personales requeridos para la validación, según el RF 3.1.2.

- [ ] **AC2:** Dado que el usuario proporciona sus datos personales dentro del límite de tiempo configurado, cuando se envía el registro alternativo, entonces el sistema compara los datos proporcionados con los datos registrados en el sistema.

- [ ] **AC3:** Dado que los datos proporcionados coinciden con los datos registrados, cuando se completa la comparación, entonces el sistema registra la asistencia, muestra un mensaje de confirmación, almacena el registro con el método marcado como “alternativo” y registra un evento de auditoría.

- [ ] **AC4:** Dado que los datos proporcionados no coinciden con los datos registrados, cuando se completa la comparación, entonces el sistema rechaza el registro alternativo y muestra un mensaje de error claro.

- [ ] **AC5:** Dado que el límite de tiempo configurado expira antes de que el usuario envíe la encuesta, cuando el usuario intenta continuar, entonces el sistema cancela el intento de registro alternativo y muestra un mensaje de tiempo agotado.

- [ ] **AC6:** Dado que la persona que intenta el registro alternativo no es un usuario registrado, cuando se validan los datos del registro alternativo, entonces el sistema rechaza el proceso y dirige a la persona al flujo de usuarios no registrados, si aplica.

- [ ] **AC7:** Dadas condiciones normales del sistema, cuando se procesa la solicitud de registro alternativo, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

---

## Notas Técnicas

- El SRS describe el registro alternativo como una encuesta con límite de tiempo, donde el usuario ingresa datos personales para ser comparados con los datos registrados.
- El valor del límite de tiempo debe ser definido por el equipo. Un valor inicial propuesto es de 60 segundos, pero debe validarse.
- Los campos de datos personales utilizados para la validación deben ser definidos por el equipo y deben seguir los principios de minimización de datos.
- Esta historia es un mecanismo de respaldo de la [HU-ATT-001](#hu-att-001-registro-de-asistencia-facial) cuando el reconocimiento facial falla por la iluminación, el movimiento, una falla de cámara o parámetros faciales que no coinciden.
- El sistema debe validar que el usuario esté registrado en el sistema, según el RF 3.1.2.
- El registro alternativo debe estar vinculado al bloque de asistencia, la fecha y el usuario correspondientes.
- El sistema debe prevenir el abuso del flujo de registro alternativo mediante limitación de tasa (rate limiting) o controles de intentos.
- Los datos personales utilizados para la validación no deben escribirse en logs ni mostrarse en la interfaz.
- Si el flujo alternativo se utiliza en un quiosco o dispositivo compartido, el sistema no debe exponer información de usuarios ajenos.
- Esta historia no cubre a los usuarios no registrados o NN. Ese caso se maneja en la [HU-ATT-004](#hu-att-004-control-de-entrada-para-usuarios-no-registrados-o-nn) y la [HU-ATT-005](../Index_HU.md#historias-planificadas-pendientes-de-documentar).

**Servicio(s) responsable(s):** Servicio de Asistencia / Servicio de Validación Alternativa, propuesto.  
**Endpoint(s) implementados:**  
- `POST /api/v1/attendance/alternative/validate` — propuesto.  
- `POST /api/v1/attendance/alternative/register` — propuesto.  

**Eventos generados:**  
- `AlternativeAttendanceRegistered`  
- `AlternativeRegistrationFailed`  
- `AlternativeRegistrationTimedOut`  
- `UnregisteredUserDetected`  

**Permisos requeridos:**  
- Usuario registrado cuyos datos puedan ser validados.  
- Si se utiliza en un quiosco o terminal de asistencia, el endpoint debe restringirse al contexto de asistencia y protegerse con limitación de tasa.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] El flujo de registro alternativo es accesible después de un fallo en el escaneo facial.
- [ ] Registro alternativo exitoso probado.
- [ ] Escenario de datos que no coinciden probado.
- [ ] Escenario de expiración del límite de tiempo probado.
- [ ] Escenario de usuario no registrado probado.
- [ ] El registro de asistencia se almacena con el método marcado como alternativo.
- [ ] Los datos personales utilizados para la validación no se exponen en logs ni en la UI.
- [ ] Se genera un evento de auditoría.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se valida bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Trazabilidad al RF 3.1.2 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 1 / MVP |
| Dependencias | [HU-IAM-002](../HU-IAM/user-stories.md#hu-iam-002-registro-de-usuario-con-asignación-de-rol): Registro de usuario con asignación de rol; [HU-ATT-001](#hu-att-001-registro-de-asistencia-facial): Registro de asistencia facial; [HU-ACAD-005](../HU-ACAD/user-stories.md#hu-acad-005-asignar-bloques-de-tiempojornadas): Asignar bloques de tiempo/jornadas |

---

# HU-ATT-004: Control de entrada para usuarios no registrados o NN

## Historia

**Como** administrador, supervisor o usuario de control de asistencia  
**Quiero** que el sistema clasifique a las personas que no forman parte del control oficial de asistencia como usuarios NN o casuales  
**Para que** el sistema pueda distinguir a los usuarios oficiales registrados de los visitantes o personas no registradas.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

- [ ] **AC1:** Dado que una persona intenta ser registrada en el proceso de control de asistencia, cuando el sistema verifica los datos de identificación de la persona, entonces el sistema determina si la persona es un usuario registrado o un usuario no registrado/NN.

- [ ] **AC2:** Dado que la persona está registrada en FaceAttend EDU, cuando se ejecuta el proceso de clasificación, entonces el sistema asigna a la persona el tipo de usuario registrado correspondiente y dirige el proceso al flujo normal o alternativo de usuarios registrados.

- [ ] **AC3:** Dado que la persona no está registrada en FaceAttend EDU, cuando se ejecuta el proceso de clasificación, entonces el sistema asigna a la persona el tipo de usuario NN o casual y habilita el flujo correspondiente de usuarios no registrados.

- [ ] **AC4:** Dado que los datos de identificación están incompletos, son inválidos o no pueden ser procesados, cuando se envía la solicitud de clasificación, entonces el sistema rechaza la clasificación y muestra un mensaje de validación claro.

- [ ] **AC5:** Dado que una persona es clasificada como usuario NN o casual, cuando se completa la clasificación, entonces el sistema almacena la clasificación por separado de los registros oficiales de asistencia de estudiantes y registra un evento de auditoría.

- [ ] **AC6:** Dadas condiciones normales del sistema, cuando se procesa la solicitud de clasificación, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

---

## Notas Técnicas

- El RF 3.2 del SRS describe este requisito como la determinación de qué usuarios son objetivo del control de asistencia y cuáles son usuarios casuales.
- La salida definida por el SRS es la asignación del tipo de usuario: NN/usuario.
- El criterio de aceptación del SRS indica que el sistema debe validar si el usuario está registrado. Para esta historia, dicha validación se interpreta como verificar si la persona existe en el registro oficial de usuarios.
- Esta historia cubre la clasificación y el control. El registro alternativo propiamente dicho para usuarios NN se maneja en la [HU-ATT-005](../Index_HU.md#historias-planificadas-pendientes-de-documentar).
- El sistema debe evitar mezclar los registros oficiales de asistencia con los registros casuales o NN, a menos que la institución requiera explícitamente reportes consolidados.
- Los datos personales utilizados para clasificar a una persona deben minimizarse y protegerse.
- El proceso de clasificación puede utilizarse en escenarios de quiosco, recepción o terminal de asistencia.
- Se recomienda la limitación de tasa y la auditabilidad para prevenir abusos o consultas no autorizadas.
- El equipo debe definir qué campos de identificación son requeridos para clasificar a una persona.

**Servicio(s) responsable(s):** Servicio de Asistencia / Servicio de Clasificación de Personas, propuesto.  
**Endpoint(s) implementados:**  
- `POST /api/v1/attendance/person-classification` — propuesto.  
- `GET /api/v1/attendance/classifications` — propuesto para revisión administrativa.  

**Eventos generados:**  
- `PersonClassifiedAsRegisteredUser`  
- `PersonClassifiedAsNN`  
- `PersonClassificationFailed`  

**Permisos requeridos:**  
- Proceso del sistema para la clasificación automática.  
- Administrador o Supervisor para la revisión de clasificaciones.  
- El acceso desde quiosco o terminal de asistencia debe estar restringido y con limitación de tasa.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Clasificación de usuario registrado probada.
- [ ] Clasificación de usuario no registrado/NN probada.
- [ ] Datos de identificación inválidos o incompletos probados.
- [ ] La clasificación se almacena por separado de los registros oficiales de asistencia.
- [ ] La clasificación NN habilita el flujo correspondiente de usuarios no registrados.
- [ ] La clasificación de usuario registrado redirige al flujo normal o alternativo de usuarios registrados.
- [ ] Se genera un evento de auditoría.
- [ ] Los datos personales no se exponen en logs ni en la UI.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se valida bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Trazabilidad al RF 3.2 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 2 |
| Dependencias | [HU-IAM-002](../HU-IAM/user-stories.md#hu-iam-002-registro-de-usuario-con-asignación-de-rol): Registro de usuario con asignación de rol; [HU-ATT-001](#hu-att-001-registro-de-asistencia-facial): Registro de asistencia facial; relacionada con [HU-ATT-005](../Index_HU.md#historias-planificadas-pendientes-de-documentar) |
