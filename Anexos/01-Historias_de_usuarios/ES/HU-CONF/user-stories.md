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

---

# HU-CONF-002: Habilitar o deshabilitar alertas

## Historia

**Como** usuario autenticado  
**Quiero** habilitar o deshabilitar alertas y notificaciones específicas  
**Para que** pueda controlar qué notificaciones recibo y reducir las interrupciones innecesarias.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que el usuario está registrado y ha iniciado sesión en el sistema, cuando el usuario accede al módulo de habilitación/deshabilitación de alertas, entonces el sistema muestra la lista de alertas disponibles con su estado actual de habilitado/deshabilitado.

- [ ] **AC2:** Dado que el usuario selecciona una alerta para habilitar, cuando se guarda el cambio, entonces el sistema habilita la alerta seleccionada y refleja el cambio en el comportamiento del sistema.

- [ ] **AC3:** Dado que el usuario selecciona una alerta para deshabilitar, cuando se guarda el cambio, entonces el sistema deshabilita la alerta seleccionada y limita las notificaciones que el usuario recibe para ese tipo de alerta.

- [ ] **AC4:** Dado que el usuario no está registrado o no ha iniciado sesión en el sistema, cuando el usuario intenta habilitar o deshabilitar alertas, entonces el sistema deniega el acceso y muestra un mensaje de error de autenticación claro.

- [ ] **AC5:** Dado que el estado de la alerta se cambia correctamente, cuando el sistema procesa el cambio, entonces el sistema muestra un mensaje de confirmación indicando que la alerta fue habilitada o deshabilitada.

- [ ] **AC6:** Dado que una alerta está deshabilitada, cuando ocurre el evento correspondiente en el sistema, entonces el sistema no envía notificaciones de ese tipo de alerta al usuario.

- [ ] **AC7:** Dado que una alerta está habilitada, cuando ocurre el evento correspondiente en el sistema, entonces el sistema envía notificaciones de ese tipo de alerta al usuario de acuerdo con sus preferencias configuradas.

- [ ] **AC8:** Dadas condiciones normales de red, cuando el usuario habilita o deshabilita una alerta, entonces el sistema responde en menos de 5 segundos, de acuerdo con el RNF 1.

- [ ] **AC9:** Dado que el estado de la alerta ha cambiado, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el usuario, el tipo de alerta, el nuevo estado, la fecha y la hora.

---

## Notas Técnicas

- El RF 4.1.1 indica que el sistema debe validar que el usuario esté registrado y haya iniciado sesión en el sistema antes de permitir las operaciones de habilitación/deshabilitación de alertas.
- Esta historia cubre específicamente el interruptor de habilitar/deshabilitar para alertas individuales, complementando el módulo de configuración general en la HU-CONF-001.
- Los tipos de alertas que pueden habilitarse o deshabilitarse deben alinearse con las categorías de alertas definidas en la HU-CONF-001:
  - Registros de asistencia (entrada/salida).
  - Resultados de justificaciones (aprobadas/rechazadas).
  - Alertas del sistema (fallos de dispositivos, mantenimiento).
  - Alertas académicas (retrasos, ausencias).
- El sistema debe considerar un interruptor maestro para habilitar o deshabilitar todas las alertas a la vez, así como interruptores individuales para cada tipo de alerta.
- Cuando una alerta está deshabilitada, el sistema no debe enviar notificaciones para ese tipo de alerta, pero debe seguir registrando el evento para fines de auditoría.
- El estado de la alerta debe almacenarse por usuario para permitir la personalización.
- El equipo debe definir si las alertas críticas (como incidentes de seguridad) pueden ser deshabilitadas por los usuarios o si deben permanecer siempre habilitadas.
- Esta historia no cubre el cambio de tonos de notificación. Eso se maneja en la HU-CONF-003.

**Servicio(s) responsable(s):** Servicio de Configuración / Servicio de Notificación, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/config/alerts/status` — propuesto, devuelve el estado de habilitado/deshabilitado de cada alerta.  
- `PATCH /api/v1/config/alerts/{alertType}/status` — propuesto, habilita o deshabilita una alerta específica.  

**Eventos generados:**  
- `AlertEnabled`  
- `AlertDisabled`  
- `AlertStatusChangeFailed`  

**Permisos requeridos:**  
- Usuario autenticado (cualquier rol).  
- La configuración es personal y se aplica únicamente a las notificaciones del usuario autenticado.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] El módulo de habilitación/deshabilitación de alertas es accesible para los usuarios autenticados.
- [ ] Se ha probado la habilitación exitosa de una alerta.
- [ ] Se ha probado la deshabilitación exitosa de una alerta.
- [ ] Se ha probado el intento de acceso no autenticado.
- [ ] No se envían notificaciones para alertas deshabilitadas.
- [ ] Se envían notificaciones para alertas habilitadas.
- [ ] El interruptor maestro (si se implementa) ha sido probado.
- [ ] Se genera un evento de auditoría.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se ha validado bajo condiciones normales y se mantiene por debajo de los 5 segundos.
- [ ] La trazabilidad al RF 4.1.1 está documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 2 |
| Prioridad | Baja |
| Sprint objetivo | Sprint 3 |
| Dependencias | HU-IAM-001: Inicio de sesión de usuario; HU-CONF-001: Configurar alertas del sistema |

---

# HU-CONF-003: Cambiar tono de notificación

## Historia

**Como** usuario autenticado  
**Quiero** cambiar el tono o sonido de las notificaciones de la aplicación  
**Para que** pueda personalizar la retroalimentación auditiva que recibo cuando el sistema me alerta sobre eventos relevantes.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que el usuario está registrado y ha iniciado sesión en el sistema, cuando el usuario accede al módulo de configuración de tonos de notificación, entonces el sistema muestra la lista de tonos de notificación disponibles.

- [ ] **AC2:** Dado que el usuario selecciona un tono de notificación de las opciones disponibles, cuando se guarda el cambio, entonces el sistema almacena el tono seleccionado y lo aplica a las notificaciones del usuario.

- [ ] **AC3:** Dado que el usuario selecciona un tono de notificación, cuando el usuario desea previsualizar el sonido, entonces el sistema permite reproducir una vista previa del tono seleccionado antes de guardar.

- [ ] **AC4:** Dado que el usuario no está registrado o no ha iniciado sesión en el sistema, cuando el usuario intenta cambiar el tono de notificación, entonces el sistema deniega el acceso y muestra un mensaje de error de autenticación claro.

- [ ] **AC5:** Dado que el tono de notificación se cambia correctamente, cuando el sistema procesa el cambio, entonces el sistema muestra un mensaje de confirmación indicando que el tono ha sido actualizado.

- [ ] **AC6:** Dado que se dispara una notificación después del cambio de tono, cuando la notificación es entregada, entonces el sistema reproduce el tono recién seleccionado.

- [ ] **AC7:** Dado que el usuario no selecciona un tono o cancela la operación, cuando el módulo se cierra, entonces el sistema conserva el tono anterior sin aplicar cambios.

- [ ] **AC8:** Dadas condiciones normales de red, cuando el usuario cambia el tono de notificación, entonces el sistema responde en menos de 5 segundos, de acuerdo con el RNF 1.

- [ ] **AC9:** Dado que el tono de notificación ha cambiado, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el usuario, el tono seleccionado, la fecha y la hora.

---

## Notas Técnicas

- El RF 4.1.2 indica que el sistema debe validar que el usuario esté registrado y haya iniciado sesión en el sistema antes de permitir los cambios de tono de notificación.
- Esta historia cubre específicamente la personalización auditiva de las notificaciones, complementando el módulo de configuración general en la HU-CONF-001.
- El sistema debe proporcionar un conjunto predefinido de tonos de notificación para que el usuario elija. El equipo debe definir los tonos disponibles.
- El sistema debe considerar:
  - Un tono predeterminado para los nuevos usuarios.
  - Una opción de "silencio" o "solo vibración", si es aplicable.
  - Funcionalidad de vista previa antes de guardar.
- El ajuste del tono de notificación debe almacenarse por usuario para permitir la personalización.
- En dispositivos móviles, la aplicación debe solicitar los permisos apropiados para reproducir sonidos.
- En navegadores web, el sistema debe manejar las políticas de reproducción automática del navegador que pueden restringir la reproducción de audio.
- El cambio de tono debe aplicarse a todos los tipos de notificación, a menos que el equipo decida permitir la configuración de tonos por tipo de alerta.
- Esta historia no cubre la habilitación/deshabilitación de alertas. Eso se maneja en la HU-CONF-002.

**Servicio(s) responsable(s):** Servicio de Configuración / Servicio de Notificación, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/config/notification-tone` — propuesto, devuelve el tono de notificación actual del usuario.  
- `PUT /api/v1/config/notification-tone` — propuesto, actualiza el tono de notificación del usuario.  

**Eventos generados:**  
- `NotificationToneUpdated`  
- `NotificationToneChangeFailed`  

**Permisos requeridos:**  
- Usuario autenticado (cualquier rol).  
- La configuración es personal y se aplica únicamente a las notificaciones del usuario autenticado.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] El módulo de tonos de notificación es accesible para los usuarios autenticados.
- [ ] Los tonos disponibles se muestran correctamente.
- [ ] Se ha probado el cambio exitoso de tono.
- [ ] Se ha probado la funcionalidad de vista previa.
- [ ] Se ha probado el intento de acceso no autenticado.
- [ ] El nuevo tono se reproduce en las notificaciones posteriores.
- [ ] La operación de cancelar conserva el tono anterior.
- [ ] Se ha probado el manejo de permisos en móviles.
- [ ] Se ha probado el manejo de la política de reproducción automática en navegadores web.
- [ ] Se genera un evento de auditoría.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se ha validado bajo condiciones normales y se mantiene por debajo de los 5 segundos.
- [ ] La trazabilidad al RF 4.1.2 está documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 2 |
| Prioridad | Baja |
| Sprint objetivo | Sprint 3 |
| Dependencias | HU-IAM-001: Inicio de sesión de usuario; HU-CONF-001: Configurar alertas del sistema |

---

# HU-CONF-004: Configurar idioma de la aplicación

## Historia

**Como** usuario autenticado  
**Quiero** configurar el idioma de la aplicación  
**Para que** pueda interactuar con el sistema en mi idioma preferido y mejorar mi experiencia de usuario.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que el usuario está registrado y ha iniciado sesión en el sistema, cuando el usuario accede al módulo de configuración de idioma, entonces el sistema muestra la lista de idiomas disponibles.

- [ ] **AC2:** Dado que el usuario selecciona un idioma de las opciones disponibles, cuando se guarda el cambio, entonces el sistema almacena el idioma seleccionado y lo aplica a la interfaz del usuario.

- [ ] **AC3:** Dado que el idioma se cambia correctamente, cuando el sistema procesa el cambio, entonces el sistema muestra un mensaje de confirmación en el idioma recién seleccionado.

- [ ] **AC4:** Dado que el usuario no está registrado o no ha iniciado sesión en el sistema, cuando el usuario intenta cambiar el idioma, entonces el sistema deniega el acceso y muestra un mensaje de error de autenticación claro.

- [ ] **AC5:** Dado que el usuario selecciona un idioma, cuando se muestran las pantallas y mensajes posteriores, entonces todos los textos, etiquetas y mensajes de la interfaz se muestran en el idioma seleccionado.

- [ ] **AC6:** Dado que el usuario no selecciona un idioma o cancela la operación, cuando el módulo se cierra, entonces el sistema conserva el idioma anterior sin aplicar cambios.

- [ ] **AC7:** Dado que un nuevo usuario se registra en el sistema, cuando no se ha establecido una preferencia de idioma, entonces el sistema utiliza el idioma predeterminado definido por el equipo.

- [ ] **AC8:** Dadas condiciones normales de red, cuando el usuario cambia el idioma, entonces el sistema responde en menos de 5 segundos, de acuerdo con el RNF 1.

- [ ] **AC9:** Dado que el idioma ha cambiado, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el usuario, el idioma seleccionado, la fecha y la hora.

---

## Notas Técnicas

- El RF 4.2 indica que el sistema debe validar que el usuario esté registrado y haya iniciado sesión en el sistema antes de permitir la configuración del idioma.
- Esta historia apoya el requisito no funcional RNF 2 (Disponibilidad), que establece que la interfaz debe soportar diferentes idiomas.
- El equipo debe definir los idiomas disponibles. Idiomas iniciales propuestos:
  - Español (predeterminado).
  - Inglés.
- El sistema debe considerar la implementación de las mejores prácticas de internacionalización (i18n):
  - Externalizar todas las cadenas orientadas al usuario en archivos de traducción.
  - Utilizar formatos basados en la localización para fechas, horas y números.
  - Soportar idiomas de derecha a izquierda (RTL) si fuera aplicable en el futuro.
- La preferencia de idioma debe almacenarse por usuario para permitir la personalización.
- El sistema debe aplicar el cambio de idioma inmediatamente sin requerir un ciclo de cierre/inicio de sesión, si es factible.
- Los mensajes de error, los mensajes de validación y los textos de notificación también deben traducirse.
- El sistema debe considerar un idioma de respaldo (predeterminado) en caso de que falte una traducción.
- Esta historia no cubre la configuración de la paleta de colores. Eso se maneja en la HU-CONF-005.
- El equipo debe definir si la configuración del idioma se aplica solo a la interfaz web o también a las notificaciones móviles y correos electrónicos.

**Servicio(s) responsable(s):** Servicio de Configuración / Servicio de Internacionalización, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/config/language` — propuesto, devuelve el idioma actual del usuario.  
- `PUT /api/v1/config/language` — propuesto, actualiza el idioma del usuario.  
- `GET /api/v1/config/available-languages` — propuesto, devuelve la lista de idiomas disponibles.  

**Eventos generados:**  
- `LanguageUpdated`  
- `LanguageChangeFailed`  

**Permisos requeridos:**  
- Usuario autenticado (cualquier rol).  
- La configuración es personal y se aplica únicamente a la interfaz del usuario autenticado.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] El módulo de configuración de idioma es accesible para los usuarios autenticados.
- [ ] Los idiomas disponibles se muestran correctamente.
- [ ] Se ha probado el cambio exitoso de idioma.
- [ ] Los textos de la interfaz se muestran en el idioma seleccionado.
- [ ] El mensaje de confirmación se muestra en el nuevo idioma.
- [ ] Se ha probado el intento de acceso no autenticado.
- [ ] La operación de cancelar conserva el idioma anterior.
- [ ] El idioma predeterminado se aplica a los nuevos usuarios.
- [ ] Los mensajes de error y validación están traducidos.
- [ ] El formato de fecha y hora se adapta a la localización seleccionada.
- [ ] Se genera un evento de auditoría.
- [ ] El tiempo de respuesta se ha validado bajo condiciones normales y se mantiene por debajo de los 5 segundos.
- [ ] La trazabilidad al RF 4.2 está documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 3 |
| Prioridad | Media |
| Sprint objetivo | Sprint 3 |
| Dependencias | HU-IAM-001: Inicio de sesión de usuario |

---

# HU-CONF-005: Actualizar paletas de colores

## Historia

**Como** administrador  
**Quiero** configurar y agregar paletas de colores para la interfaz de la aplicación  
**Para que** la identidad visual de la institución se refleje en el sistema y la experiencia del usuario se personalice según los criterios institucionales.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que un administrador autenticado accede al módulo de configuración de paletas de colores, cuando el sistema carga el módulo, entonces muestra las paletas de colores actuales y la opción de agregar o modificar paletas.

- [ ] **AC2:** Dado que el administrador selecciona o crea una nueva paleta de colores, cuando se guardan los cambios, entonces el sistema valida los valores de color, almacena la nueva paleta y refleja los cambios en el comportamiento visual de la aplicación.

- [ ] **AC3:** Dado que el administrador ingresa valores de color inválidos (códigos mal formados, valores fuera de rango), cuando se envía la operación de guardado, entonces el sistema rechaza la operación y muestra mensajes de validación específicos.

- [ ] **AC4:** Dado que el usuario que intenta configurar las paletas de colores no tiene permisos de administrador, cuando se procesa la solicitud, entonces el sistema deniega el acceso y registra un intento de acceso no autorizado.

- [ ] **AC5:** Dado que la paleta de colores se actualiza correctamente, cuando el sistema aplica los cambios, entonces las pantallas y componentes de la interfaz posteriores reflejan el nuevo esquema de colores.

- [ ] **AC6:** Dado que el administrador cancela la operación sin guardar, cuando el módulo se cierra, entonces el sistema conserva la paleta de colores anterior sin aplicar cambios.

- [ ] **AC7:** Dadas condiciones normales de red, cuando el administrador guarda la configuración de la paleta de colores, entonces el sistema responde en menos de 5 segundos, de acuerdo con el RNF 1.

- [ ] **AC8:** Dado que la paleta de colores se ha actualizado, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el administrador, la fecha y la hora.

---

## Notas Técnicas

- El RF 4.3 indica que el sistema debe validar que el usuario esté registrado y haya iniciado sesión en el sistema antes de permitir la configuración de la paleta de colores.
- El RF 4.3 especifica que esta es una operación **exclusiva para administradores**. Los usuarios regulares, instructores y estudiantes no deben poder modificar las paletas de colores.
- La configuración de la paleta de colores debe soportar formatos de color estándar, como códigos hexadecimales (por ejemplo, `#RRGGBB`) o valores RGB.
- El sistema debe considerar:
  - Una funcionalidad de vista previa antes de guardar los cambios.
  - Validación de contraste para asegurar el cumplimiento de la accesibilidad (guías WCAG).
  - Una paleta predeterminada que se aplica si no hay una paleta personalizada configurada.
- Los cambios de la paleta de colores deben aplicarse globalmente en toda la aplicación o por ámbito institucional, dependiendo de la decisión del equipo.
- El equipo debe definir si pueden coexistir múltiples paletas y cómo se aplican (por institución, por rol o globalmente).
- Esta historia no cubre la configuración del idioma. Eso se maneja en la HU-CONF-004.
- Esta historia no cubre la configuración de alertas. Eso se maneja en la HU-CONF-001.

**Servicio(s) responsable(s):** Servicio de Configuración / Servicio de temas de UI, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/config/color-palettes` — propuesto, devuelve las paletas de colores actuales.  
- `POST /api/v1/config/color-palettes` — propuesto, agrega una nueva paleta de colores.  
- `PUT /api/v1/config/color-palettes/{paletteId}` — propuesto, actualiza una paleta existente.  

**Eventos generados:**  
- `ColorPaletteUpdated`  
- `ColorPaletteChangeFailed`  
- `UnauthorizedPaletteAccessAttempt`  

**Permisos requeridos:**  
- Solo Administrador.  
- Los usuarios regulares, instructores y estudiantes no deben acceder a esta configuración.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] El módulo de paletas de colores es accesible solo para administradores.
- [ ] Se ha probado la creación exitosa de una paleta.
- [ ] Se ha probado la actualización exitosa de una paleta.
- [ ] Se ha probado el escenario de valores de color inválidos.
- [ ] Se ha probado el intento de acceso no autorizado.
- [ ] La funcionalidad de vista previa ha sido probada (si se implementa).
- [ ] La interfaz refleja el nuevo esquema de colores después de guardar.
- [ ] La operación de cancelar conserva la paleta anterior.
- [ ] Se genera un evento de auditoría.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se ha validado bajo condiciones normales y se mantiene por debajo de los 5 segundos.
- [ ] La trazabilidad al RF 4.3 está documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 3 |
| Prioridad | Media |
| Sprint objetivo | Sprint 3 |
| Dependencias | HU-IAM-001: Inicio de sesión de usuario; HU-IAM-003: Asignación de permisos basados en roles |

---

# HU-CONF-006: Actualizar parámetros faciales del usuario

## Historia

**Como** usuario autenticado o administrador  
**Quiero** actualizar mis parámetros faciales en el sistema  
**Para que** mis datos de reconocimiento facial permanezcan precisos y actualizados para un registro de asistencia confiable.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que el usuario está registrado y ha iniciado sesión en el sistema, cuando el usuario solicita una actualización de parámetros faciales, entonces el sistema activa la cámara e inicia el proceso de captura facial.

- [ ] **AC2:** Dado que la cámara está activa y el usuario posiciona su rostro correctamente, cuando el sistema captura la nueva imagen facial, entonces el sistema procesa la captura y valida los nuevos parámetros faciales.

- [ ] **AC3:** Dado que los nuevos parámetros faciales son válidos, cuando se guarda la actualización, entonces el sistema reemplaza los parámetros faciales anteriores por los nuevos y refleja los cambios en el comportamiento del sistema.

- [ ] **AC4:** Dado que la captura facial falla debido a la iluminación, el movimiento o problemas de la cámara, cuando se procesa el intento de actualización, entonces el sistema muestra un mensaje de error claro y permite al usuario reintentar.

- [ ] **AC5:** Dado que el usuario no está registrado o no ha iniciado sesión en el sistema, cuando el usuario intenta actualizar los parámetros faciales, entonces el sistema deniega el acceso y muestra un mensaje de error de autenticación claro.

- [ ] **AC6:** Dado que la actualización de los parámetros faciales es exitosa, cuando el sistema procesa el cambio, entonces el sistema muestra un mensaje de confirmación y registra un evento de auditoría que contiene el usuario, la fecha y la hora.

- [ ] **AC7:** Dado que los parámetros faciales han sido actualizados, cuando el usuario realiza un registro de asistencia posterior, entonces el sistema utiliza los nuevos parámetros faciales para el reconocimiento.

- [ ] **AC8:** Dado que un administrador solicita una actualización de parámetros faciales en nombre de otro usuario, cuando el administrador tiene los permisos apropiados, entonces el sistema permite la actualización y registra la identidad del administrador en el evento de auditoría.

- [ ] **AC9:** Dadas condiciones normales de dispositivo, cámara y red, cuando el usuario completa la actualización de parámetros faciales, entonces el sistema responde en menos de 5 segundos, de acuerdo con el RNF 1.

---

## Notas Técnicas

- El RF 4.4 indica que el sistema debe validar que el usuario esté registrado y haya iniciado sesión en el sistema antes de permitir las actualizaciones de parámetros faciales.
- El RF 4.4 especifica que esta operación puede ser solicitada por el propio usuario o por un administrador.
- Esta historia complementa la HU-ATT-002 (inscripción inicial de parámetros faciales). La historia de inscripción cubre el registro por primera vez, mientras que esta historia cubre las actualizaciones de los parámetros existentes.
- La actualización de parámetros faciales debe seguir los mismos requisitos de seguridad y protección de datos que la inscripción inicial:
  - Los datos biométricos deben almacenarse de forma segura.
  - Se deben preferir plantillas o embeddings faciales sobre imágenes faciales brutas.
  - Cumplimiento con la Ley 1581 de 2012 de Colombia (Habeas Data).
- El sistema debe considerar:
  - Requerir la confirmación explícita del usuario antes de reemplazar los parámetros existentes.
  - Mantener una copia de seguridad de los parámetros anteriores durante un período de retención definido.
  - Invalidar los parámetros anteriores inmediatamente después de la actualización.
- La actualización de los parámetros faciales puede requerir una re-autenticación para confirmar la identidad del usuario antes de proceder.
- El sistema no debe exponer parámetros faciales o datos biométricos en logs o mensajes de la interfaz.
- Esta historia no cubre la inscripción facial inicial. Eso se maneja en la HU-ATT-002.
- Esta historia no cubre la activación/desactivación de usuarios. Eso se maneja en la HU-IAM-007 o HU-CONF-007 dependiendo de la interpretación del SRS.

**Servicio(s) responsable(s):** Servicio de Configuración / Servicio Biométrico, propuesto.  
**Endpoint(s) implementados:**  
- `POST /api/v1/users/me/facial-parameters/update` — propuesto, para actualizaciones de autoservicio.  
- `POST /api/v1/users/{userId}/facial-parameters/update` — propuesto, para actualizaciones iniciadas por el administrador.  

**Eventos generados:**  
- `FacialParameterUpdated`  
- `FacialParameterUpdateFailed`  
- `UnauthorizedFacialParameterUpdateAttempt`  

**Permisos requeridos:**  
- Usuario autenticado para actualizaciones de autoservicio.
- Administrador o Supervisor para actualizaciones en nombre de otros usuarios.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Se ha probado la actualización exitosa de parámetros faciales por autoservicio.
- [ ] Se ha probado la actualización exitosa iniciada por el administrador.
- [ ] Se ha probado el escenario de fallo en la captura facial.
- [ ] Se ha probado el intento de acceso no autenticado.
- [ ] Los nuevos parámetros se utilizan para el registro de asistencia posterior.
- [ ] Los parámetros anteriores se invalidan después de la actualización.
- [ ] Se requiere la confirmación del usuario antes de reemplazar los parámetros.
- [ ] Los datos biométricos no se exponen en logs ni en la UI.
- [ ] Se genera un evento de auditoría.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se ha validado bajo condiciones normales y se mantiene por debajo de los 5 segundos.
- [ ] La trazabilidad al RF 4.4 está documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Media |
| Sprint objetivo | Sprint 2 |
| Dependencias | HU-IAM-001: Inicio de sesión de usuario; HU-ATT-002: Inscripción inicial de parámetros faciales |

---

# HU-CONF-007: Gestión de alertas de fallos de dispositivos IoT

## Historia

**Como** administrador o supervisor  
**Quiero** recibir alertas cuando los dispositivos IoT utilizados para el registro de asistencia experimenten fallos  
**Para que** pueda tomar acciones correctivas oportunas y mantener la disponibilidad del sistema de registro de asistencia.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que un dispositivo IoT registrado en el sistema detecta un fallo (mal funcionamiento de la cámara, pérdida de conectividad, error de hardware), cuando se detecta el fallo, entonces el sistema genera una alerta y notifica a los administradores o supervisores configurados.

- [ ] **AC2:** Dado que se genera una alerta de fallo de dispositivo IoT, cuando el administrador accede al módulo de gestión de alertas, entonces el sistema muestra la alerta con la información del dispositivo, el tipo de fallo, la fecha y la hora.

- [ ] **AC3:** Dado que el administrador reconoce la alerta, cuando se procesa el reconocimiento, entonces el sistema marca la alerta como reconocida y registra la identidad del administrador y la marca de tiempo.

- [ ] **AC4:** Dado que el administrador resuelve el fallo del dispositivo, cuando se registra la resolución, entonces el sistema marca la alerta como resuelta y actualiza el estado del dispositivo en consecuencia.

- [ ] **AC5:** Dado que un dispositivo IoT se reconecta o se recupera automáticamente, cuando el sistema detecta la recuperación, entonces el sistema actualiza el estado del dispositivo y cierra cualquier alerta abierta asociada a ese dispositivo.

- [ ] **AC6:** Dado que el usuario que intenta acceder al módulo de gestión de alertas no tiene permisos de administrador o supervisor, cuando se procesa la solicitud, entonces el sistema deniega el acceso y registra un intento de acceso no autorizado.

- [ ] **AC7:** Dadas condiciones normales de red, cuando el administrador accede o gestiona las alertas de dispositivos IoT, entonces el sistema responde en menos de 5 segundos, de acuerdo con el RNF 1.

- [ ] **AC8:** Dado que se genera una alerta de fallo de dispositivo IoT, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el dispositivo, el tipo de fallo, la fecha y la hora.

---

## Notas Técnicas

- ⚠️ **Nota de Inconsistencia del SRS:** El índice del SRS enumera el RF 4.5 como "Gestión de alertas de fallos de dispositivos IoT", pero la sección detallada del RF 4.5 describe "Desactivar y reactivar usuarios". Esta historia está documentada basándose en la **descripción del índice** (alertas de fallos de dispositivos IoT). La funcionalidad de activación/desactivación de usuarios está cubierta en la HU-IAM-007 (RF 1.6). Esta inconsistencia debe ser validada con el instructor.
- La gestión de alertas de fallos de dispositivos IoT debe considerar:
  - Tipos de dispositivos: cámaras, sensores u otro hardware utilizado para el registro de asistencia.
  - Tipos de fallos: pérdida de conectividad, mal funcionamiento del hardware, fallo de la cámara, batería baja (si es aplicable).
  - Niveles de severidad de la alerta: informativa, advertencia, crítica.
- El mecanismo de notificación de alertas debe integrarse con el servicio de notificaciones definido en la HU-CONF-001 y la HU-JUS-003.
- El sistema debe considerar:
  - Verificaciones de salud automáticas para los dispositivos IoT registrados.
  - Umbrales de alerta configurables (por ejemplo, alertar después de N fallos consecutivos).
  - Escalamiento de alertas para fallos críticos no resueltos.
- El estado del dispositivo debe actualizarse en tiempo real o casi real.
- El sistema debe mantener un registro de dispositivos con información como el ID del dispositivo, ubicación/entorno, estado y marca de tiempo de la última actividad.
- Esta historia no cubre la activación/desactivación de usuarios. Eso se maneja en la HU-IAM-007.
- El equipo debe definir los tipos específicos de dispositivos IoT y los mecanismos de detección de fallos.

**Servicio(s) responsable(s):** Servicio de Configuración / Servicio de monitoreo de IoT, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/iot-devices/alerts` — propuesto, devuelve las alertas de fallos de dispositivos IoT.  
- `PATCH /api/v1/iot-devices/alerts/{alertId}/acknowledge` — propuesto, reconoce una alerta.  
- `PATCH /api/v1/iot-devices/alerts/{alertId}/resolve` — propuesto, resuelve una alerta.  
- `GET /api/v1/iot-devices` — propuesto, devuelve el registro de dispositivos con su estado.  

**Eventos generados:**  
- `IoTDeviceFailureDetected`  
- `IoTDeviceAlertAcknowledged`  
- `IoTDeviceAlertResolved`  
- `IoTDeviceRecovered`  
- `UnauthorizedIoTAlertAccessAttempt`  

**Permisos requeridos:**  
- Administrador o Supervisor para la gestión de alertas.  
- Los usuarios regulares, instructores y estudiantes no deben acceder a las alertas de dispositivos IoT.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Se ha probado la detección de fallos de dispositivos IoT.
- [ ] Se ha probado la generación de alertas para diferentes tipos de fallos.
- [ ] Se ha probado el reconocimiento de alertas.
- [ ] Se ha probado la resolución de alertas.
- [ ] Se ha probado el escenario de recuperación automática del dispositivo.
- [ ] Se ha probado el intento de acceso no autorizado.
- [ ] El registro de dispositivos muestra la información de estado correcta.
- [ ] Se genera un evento de auditoría para todas las operaciones de alerta.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se ha validado bajo condiciones normales y se mantiene por debajo de los 5 segundos.
- [ ] La inconsistencia del SRS está documentada y validada con el instructor.
- [ ] La trazabilidad al RF 4.5 está documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 3 |
| Dependencias | HU-IAM-001: Inicio de sesión de usuario; HU-IAM-003: Asignación de permisos basados en roles; HU-CONF-001: Configurar alertas del sistema |

---

# HU-CONF-008: Parametrización de retardos y justificaciones

## Historia

**Como** administrador  
**Quiero** configurar qué excusas o justificaciones son válidas para soportar ausencias o retardos  
**Para que** el proceso de evaluación de justificaciones siga los criterios institucionales y asegure un tratamiento consistente de todos los casos de estudiantes.

---

## Criterios de Aceptación

> Formato: "Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable]."

- [ ] **AC1:** Dado que un administrador autenticado accede al módulo de parametrización de retardos y justificaciones, cuando el sistema carga el módulo, entonces muestra la lista de justificaciones válidas configuradas actualmente y la opción de agregar, modificar o eliminar parámetros.

- [ ] **AC2:** Dado que el administrador agrega un nuevo parámetro de justificación válida, cuando se envía la operación de guardado, entonces el sistema valida que el parámetro no exista ya en el sistema y almacena el nuevo tipo de justificación.

- [ ] **AC3:** Dado que el parámetro de justificación ya existe en el sistema, cuando el administrador intenta agregarlo nuevamente, entonces el sistema rechaza la operación y muestra un mensaje de error claro indicando que el parámetro ya está definido.

- [ ] **AC4:** Dado que el administrador modifica un parámetro de justificación existente, cuando se guardan los cambios, entonces el sistema actualiza el parámetro y refleja los cambios en las evaluaciones de justificación posteriores.

- [ ] **AC5:** Dado que el administrador elimina un parámetro de justificación, cuando se confirma la eliminación, entonces el sistema desactiva el parámetro y este ya no se considera válido para nuevas evaluaciones de justificación.

- [ ] **AC6:** Dado que el usuario que intenta acceder al módulo de parametrización no tiene permisos de administrador, cuando se procesa la solicitud, entonces el sistema deniega el acceso y registra un intento de acceso no autorizado.

- [ ] **AC7:** Dado que se ha configurado un parámetro de justificación, cuando un estudiante envía una justificación en la HU-JUS-001, entonces el sistema utiliza los parámetros configurados para determinar si la justificación es válida durante la evaluación en la HU-JUS-002.

- [ ] **AC8:** Dadas condiciones normales de red, cuando el administrador guarda los cambios de parametrización, entonces el sistema responde en menos de 5 segundos, de acuerdo con el RNF 1.

- [ ] **AC9:** Dado que se agrega, modifica o elimina un parámetro de justificación, cuando se completa la operación, entonces el sistema registra un evento de auditoría que contiene el administrador, el tipo de acción, los detalles del parámetro, la fecha y la hora.

---

## Notas Técnicas

- El RF 4.7 indica que el sistema debe validar que el parámetro configurado no esté ya definido en el sistema.
- El RF 4.7 especifica que esta es una operación **exclusiva para administradores** para definir justificaciones válidas.
- La parametrización de justificaciones debe considerar:
  - Nombre del tipo de justificación (por ejemplo, certificado médico, emergencia familiar, actividad institucional).
  - Si se requiere un documento de soporte.
  - Si la justificación aplica a ausencias, retardos o ambos.
  - Período máximo de validez (por ejemplo, la justificación debe enviarse dentro de N días).
  - Si la justificación requiere la aprobación de un rol específico.
- Los parámetros configurados son utilizados por:
  - HU-JUS-001: Para validar que la justificación enviada coincida con un tipo válido.
  - HU-JUS-002: Para determinar si la justificación es válida durante la evaluación.
  - HU-REP-004: Para filtrar y reportar retardos y ausencias por estado de justificación.
- El sistema debe considerar:
  - Evitar la eliminación de parámetros de justificación que estén siendo utilizados activamente en justificaciones pendientes.
  - Mantener un registro histórico de los cambios de parámetros para fines de auditoría.
  - Proporcionar tipos de justificación predeterminados para la configuración inicial.
- El equipo debe definir si los parámetros de justificación son específicos de la institución o globales.
- Esta historia no cubre el proceso de envío de justificaciones. Eso se maneja en la HU-JUS-001.
- Esta historia no cubre el proceso de evaluación de justificaciones. Eso se maneja en la HU-JUS-002.
- Esta historia no cubre la detección de retardos. Eso se maneja en la HU-ATT-001.

**Servicio(s) responsable(s):** Servicio de Configuración / Servicio de parametrización de justificaciones, propuesto.  
**Endpoint(s) implementados:**  
- `GET /api/v1/config/justifications` — propuesto, devuelve los parámetros de justificación configurados.  
- `POST /api/v1/config/justifications` — propuesto, agrega un nuevo parámetro de justificación.  
- `PUT /api/v1/config/justifications/{paramId}` — propuesto, modifica un parámetro existente.  
- `DELETE /api/v1/config/justifications/{paramId}` — propuesto, desactiva un parámetro.  

**Eventos generados:**  
- `JustificationParameterAdded`  
- `JustificationParameterModified`  
- `JustificationParameterDeactivated`  
- `JustificationParametrizationFailed`  
- `UnauthorizedParametrizationAccessAttempt`  

**Permisos requeridos:**  
- Solo Administrador.  
- Los usuarios regulares, instructores y estudiantes no deben acceder a la parametrización de justificaciones.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] El módulo de parametrización de justificaciones es accesible solo para administradores.
- [ ] Se ha probado la adición exitosa de un parámetro.
- [ ] Se ha probado el escenario de parámetro duplicado.
- [ ] Se ha probado la modificación exitosa de un parámetro.
- [ ] Se ha probado la desactivación exitosa de un parámetro.
- [ ] Se ha probado el intento de acceso no autorizado.
- [ ] Los parámetros configurados se utilizan en el flujo de evaluación de justificaciones.
- [ ] Se genera un evento de auditoría para todas las operaciones de parametrización.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se ha validado bajo condiciones normales y se mantiene por debajo de los 5 segundos.
- [ ] La trazabilidad al RF 4.7 está documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 2 |
| Dependencias | HU-IAM-001: Inicio de sesión de usuario; HU-IAM-003: Asignación de permisos basados en roles |
