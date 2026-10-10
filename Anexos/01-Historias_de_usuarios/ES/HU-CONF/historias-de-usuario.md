# HU-CONF-001: Configurar alertas del sistema

## Historia

**Como** usuario autenticado  
**Quiero** configurar las alertas y notificaciones de la aplicación  
**Para que** pueda personalizar cómo el sistema me informa sobre eventos relevantes como registros de asistencia, justificaciones y alertas del sistema.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que el usuario está registrado y ha iniciado sesión en el sistema, cuando el usuario accede al módulo de configuración de alertas, entonces el sistema muestra las opciones de configuración de alertas disponibles.

- [ ] **AC2:** Dado que el usuario modifica una o más opciones de configuración de alertas, cuando se guardan los cambios, entonces el sistema almacena la nueva configuración y refleja los cambios en el comportamiento del sistema.

- [ ] **AC3:** Dado que el usuario no está registrado o no ha iniciado sesión en el sistema, cuando el usuario intenta acceder al módulo de configuración de alertas, entonces el sistema deniega el acceso y muestra un mensaje de error de autenticación claro.

- [ ] **AC4:** Dado que la configuración de alertas se guarda correctamente, cuando el sistema procesa los cambios, entonces el sistema muestra un mensaje de confirmación indicando que la configuración ha sido actualizada.

- [ ] **AC5:** Dado que la configuración de alertas falla al guardarse debido a un error del sistema, cuando se procesa la operación, entonces el sistema muestra un mensaje de error claro y no aplica los cambios.

- [ ] **AC6:** Dado que la configuración de alertas del usuario ha sido actualizada, cuando ocurre un evento relevante en el sistema, entonces el sistema envía notificaciones de acuerdo con las preferencias configuradas por el usuario.

- [ ] **AC7:** Dadas condiciones normales de red, cuando el usuario guarda la configuración de alertas, entonces el sistema responde en menos de 5 segundos, de acuerdo con el RNF 1.

- [ ] **AC8:** Dado que la configuración de alertas se ha actualizado, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el usuario, la fecha y la hora.

---

## Notas Técnicas

- El RF 4.1 indica que el sistema debe validar que el usuario esté registrado y haya iniciado sesión en el sistema antes de permitir la configuración de alertas.
- Esta historia cubre el módulo general de configuración de alertas. Las sub-opciones específicas, como habilitar/deshabilitar alertas y cambiar los tonos de notificación, se manejan en historias separadas (HU-CONF-002 y HU-CONF-003).
- La configuración de alertas debe almacenarse por usuario para permitir la personalización.
- El sistema debe considerar los siguientes tipos de alertas:
  - Registros de asistencia (entrada/salida).
  - Resultados de justificaciones (aprobadas/rechazadas).
  - Alertas del sistema (fallos de dispositivos, mantenimiento).
  - Alertas académicas (retrasos, ausencias).
- La configuración de alertas debe ser compatible con el mecanismo de entrega de notificaciones definido en la HU-JUS-003.
- El sistema debe validar que el usuario esté autenticado antes de procesar cualquier cambio de configuración.
- Los cambios de configuración deben ser auditables para garantizar la trazabilidad.
- Esta historia no cubre las alertas de fallos de dispositivos IoT. Eso se maneja en la HU-CONF-007.
- El equipo debe definir la configuración de alertas predeterminada para los nuevos usuarios.

**Servicio(s) responsable(s):** Servicio de Configuración / Servicio de Notificación, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/config/alerts` — propuesto, devuelve la configuración de alertas actual del usuario.  
- `PUT /api/v1/config/alerts` — propuesto, actualiza la configuración de alertas del usuario.  

**Eventos generados:**  
- `AlertConfigurationUpdated`  
- `AlertConfigurationFailed`  

**Permisos requeridos:**  
- Usuario autenticado (cualquier rol).  
- La configuración es personal y se aplica únicamente a las notificaciones del usuario autenticado.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] El módulo de configuración de alertas es accesible para los usuarios autenticados.
- [ ] Se ha probado la actualización exitosa de la configuración.
- [ ] Se ha probado el intento de acceso no autenticado.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] La configuración se almacena por usuario.
- [ ] Las notificaciones se envían de acuerdo con las preferencias configuradas.
- [ ] Se genera un evento de auditoría.
- [ ] El tiempo de respuesta se ha validado bajo condiciones normales y se mantiene por debajo de los 5 segundos.
- [ ] La trazabilidad al RF 4.1 está documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 3 |
| Prioridad | Media |
| Sprint objetivo | Sprint 3 |
| Dependencias | HU-IAM-001: Inicio de sesión de usuario |
