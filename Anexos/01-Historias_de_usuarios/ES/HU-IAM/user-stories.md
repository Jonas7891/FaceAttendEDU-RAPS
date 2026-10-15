# HU-IAM-001: Inicio de sesión de usuario

## Historia

**Como** usuario registrado (estudiante, instructor, administrador)  
**Quiero** iniciar sesión en FaceAttend EDU utilizando mis credenciales  
**Para que** pueda acceder de forma segura a los módulos y la información permitidos por mi rol.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

- [ ] **AC1:** Dado que un usuario está registrado y activo en el sistema, cuando el usuario envía credenciales válidas (`identifier` = correo electrónico o nombre de usuario, más la contraseña), entonces el sistema autentica al usuario, crea una **sesión opaca** (fila de `identity.user_session` que devuelve un `sessionId` usado como `Authorization: Bearer`) y otorga acceso según el rol asignado.

- [ ] **AC2:** Dado que un usuario envía credenciales inválidas, cuando se procesa la solicitud de inicio de sesión, entonces el sistema deniega el acceso con `401` y el mensaje genérico **`Invalid email or password`**, sin revelar si el identificador existe.

- [ ] **AC3:** Dado que la cuenta de un usuario está inactiva o desactivada, cuando el usuario intenta iniciar sesión, entonces el sistema deniega el acceso con el mismo `401 Invalid email or password` genérico (nota de implementación: **no** se devuelve un mensaje específico de “cuenta no activa” — es deliberado, para evitar la enumeración de cuentas; `AuthenticateUserUseCaseImpl`).

- [ ] **AC4:** Dado que un usuario envía credenciales inválidas repetidamente, cuando se alcanza el umbral configurado de intentos fallidos, entonces el sistema bloquea o retrasa temporalmente los intentos posteriores y registra el evento para auditoría.
  > **Estado:** no implementado — `configuration.security_configuration` carga por defecto `max_login_attempts=5`
  > y `lockout_duration_minutes=30`, pero ningún servicio los lee aún. Registrado como trabajo pendiente.

- [ ] **AC5:** Dado que el sistema se ejecuta en un entorno de producción o piloto, cuando el usuario envía sus credenciales de inicio de sesión, entonces la comunicación se protege mediante HTTPS.

- [ ] **AC6:** Dadas condiciones normales de red, cuando el usuario envía credenciales válidas, entonces el proceso de inicio de sesión responde en menos de 5 segundos, según el RNF 1.

- [ ] **AC7:** Dado que el inicio de sesión es exitoso, cuando el usuario accede al sistema, entonces el sistema registra un evento de auditoría de inicio de sesión con fecha, hora, identificador del usuario y resultado.
  > Implementado como registro de la plataforma (platform logging) + actualización de `last_access` en `app_user` (el contexto
  > delimitado de Auditoría fue eliminado, 2026-09).

---

## Notas Técnicas

- El RF 1.3 del SRS presenta una inconsistencia: la entrada se describe como “Registro en FaceAttend EDU”, pero la entrada correcta es el **campo `identifier`** (correo electrónico **o** nombre de usuario — `@JsonAlias({"username","email"})`) más la contraseña.
- Las contraseñas no deben almacenarse ni transmitirse en texto plano; el hash es **bcrypt con costo 12, `$2b`** (ADR-008, `SecurityConfig`).
- Los intentos fallidos de inicio de sesión se registran sin exponer datos sensibles (siempre un 401 genérico).
- **No hay tokens de acceso/refresco**: las sesiones son filas en `identity.user_session` (`session_status` Active/Closed) y se cierran mediante `DELETE /api/v1/auth/logout`. Kong no tiene el plugin `jwt`; cada servicio Java valida el bearer con `AuthTokenFilter`.
- El flujo de inicio de sesión debe considerar la redirección basada en roles tras una autenticación exitosa (la Web mapea los roles mediante `backendRoles.js`: `Administrador|Instructor|Aprendiz`, se aceptan los nombres heredados).
- Los datos sensibles de autenticación no deben escribirse en logs ni mostrarse en la interfaz de usuario.

**Servicio(s) responsable(s):** `ms-identity` (la autenticación está integrada en Identity; no existe un servicio IAM separado).  
**Endpoint(s) implementados:**  
- `POST /api/v1/auth/login` — implementado (`AuthController`).  
- `POST /api/v1/auth/logout` — implementado.  
- `GET /api/v1/auth/me` — implementado (devuelve el `UserDto` completo).  

**Eventos generados:**  
- `UserLoggedIn`  
- `LoginFailed`  
- `UserTemporarilyBlocked`  

**Permisos requeridos:**  
- Endpoint público para el inicio de sesión.  
- Se requiere una sesión autenticada tras un inicio de sesión exitoso.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Escenario de inicio de sesión válido probado.
- [ ] Escenario de inicio de sesión inválido probado.
- [ ] Escenario de usuario inactivo probado.
- [ ] Escenario de intentos fallidos repetidos probado.
- [ ] La contraseña no se expone en logs, respuestas ni UI.
- [ ] HTTPS se aplica de forma obligatoria en producción/piloto.
- [ ] Se genera el evento de auditoría de inicio de sesión.
- [ ] Los mensajes de error son claros pero no revelan información sensible.
- [ ] El tiempo de respuesta del inicio de sesión se valida bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Trazabilidad al RF 1.3 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 1 / MVP |
| Dependencias | HU-IAM-002: Registro de usuario, o un usuario administrador inicial precargado |

---

# HU-IAM-002: Registro de usuario con asignación de rol

## Historia

**Como** administrador o usuario institucional autorizado  
**Quiero** registrar un nuevo usuario y asignarle un rol  
**Para que** el usuario pueda existir en el sistema y acceder a los módulos correspondientes a su función institucional.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

- [ ] **AC1:** Dado que un administrador proporciona datos de usuario válidos, cuando se envía el registro del usuario, entonces el sistema valida que el usuario no exista previamente, crea el usuario, asigna el rol seleccionado, almacena los datos de forma segura y muestra un mensaje de confirmación.

- [ ] **AC2:** Dado que los datos del usuario están incompletos o son inválidos, cuando se envía el registro, entonces el sistema rechaza la operación y muestra errores de validación específicos para los campos faltantes o incorrectos.

- [ ] **AC3:** Dado que ya existe un usuario con el mismo correo institucional, identificador o nombre de usuario, cuando el administrador intenta registrar un nuevo usuario, entonces el sistema evita el registro duplicado y muestra un mensaje de error claro.

- [ ] **AC4:** Dado que se selecciona un rol durante el registro, cuando se crea el usuario, entonces el sistema asocia el identificador del usuario con el identificador de la persona y el rol asignado, según el RF 1.1.1.

- [ ] **AC5:** Dado que el rol seleccionado no existe o no está permitido para el nuevo usuario, cuando se envía el registro, entonces el sistema rechaza la operación y muestra un error de validación.

- [ ] **AC6:** Dado que el registro es exitoso, cuando el administrador consulta la auditoría o el historial, entonces el sistema registra los eventos `UserRegistered` y `RoleAssigned`.

- [ ] **AC7:** Dadas condiciones normales de red, cuando el administrador completa el registro del usuario, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

---

## Notas Técnicas

- Esta historia cubre el registro básico de usuarios y la asignación de roles. La importación masiva de usuarios mediante CSV se cubre por separado en la HU-IAM-004.
- El sistema debe validar que el usuario no exista antes del registro, según los criterios de aceptación del RF 1.1.
- El sistema debe validar los datos ingresados, según los criterios de aceptación del RF 1.1.
- La asignación de roles debe incluir la relación entre el identificador del usuario y el identificador de la persona, según el RF 1.1.1.
- Los roles de usuario considerados en el SRS incluyen:
  - Supervisor.
  - Administrador.
  - Docente / Instructor.
  - Estudiante.
  - Padre de familia o acudiente, si aplica.
- **Catálogo efectivo de roles en la base de datos:** `Administrador | Instructor | Aprendiz`
  (changeset `auth-004-unify-mobile-roles`); los nombres heredados `SUPER_ADMIN / SCHOOL_ADMIN / INSTRUCTOR /
  STUDENT` se fusionan en esos tres y el cliente Web aún los acepta
  (`backendRoles.js`) para bases de datos no migradas.
- El manejo de contraseñas debe cumplir con los requisitos de seguridad. Si el registro genera una contraseña inicial o un flujo de invitación, esto debe ser definido por el equipo.
- Los datos sensibles deben almacenarse de forma segura y no deben aparecer en logs.
- El registro debe ser auditable para fines de trazabilidad.

**Servicio(s) responsable(s):** `ms-identity` (persona + usuario) y `ms-authorization` (asignación de roles).  
**Endpoint(s) implementados:**  
- `POST /api/v1/persons` — implementado (`PersonController`).  
- `POST /api/v1/users` — implementado (`UserController`).  
- `POST /api/v1/users/{userId}/roles` — implementado en `ms-authorization` (`UserRoleController`), invocado después de crear el usuario.  
- `POST /api/v1/users/{id}/activate` · `PATCH /api/v1/users/{id}/status` — implementado (`UserController`).  

**Eventos generados:**  
- `UserRegistered`  
- `RoleAssigned`  
- `UserRegistrationFailed`  

**Permisos requeridos:**  
- Administrador o Supervisor, según la política institucional.  
- El autorregistro solo debe permitirse si la institución lo aprueba explícitamente.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Registro de usuario exitoso probado.
- [ ] Escenario de datos inválidos o incompletos probado.
- [ ] Escenario de usuario duplicado probado.
- [ ] Asignación de rol probada.
- [ ] Escenario de rol inválido probado.
- [ ] La relación usuario-persona-rol se almacena correctamente.
- [ ] Los datos sensibles no se exponen en logs ni en la UI.
- [ ] Se generan los eventos de auditoría.
- [ ] Los mensajes de confirmación y error son claros.
- [ ] El tiempo de respuesta se valida bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Trazabilidad al RF 1.1 y RF 1.1.1 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 8 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 1 / MVP |
| Dependencias | Catálogo inicial de roles; se recomienda la HU-IAM-003 para la matriz de permisos |

---

# HU-IAM-003: Asignación de permisos basada en roles

## Historia

**Como** administrador  
**Quiero** asignar permisos a cada rol  
**Para que** los usuarios solo puedan acceder a los módulos, vistas y acciones permitidos por su rol activo.

---

## Criterios de Aceptación

> Formato: “Dado [contexto/estado inicial], cuando [acción del usuario], entonces [resultado esperado y verificable].”

- [ ] **AC1:** Dado que un administrador está autenticado, cuando el administrador asigna permisos a un rol, entonces el sistema almacena la relación rol-permiso y la aplica a los usuarios asignados a ese rol.

- [ ] **AC2:** Dado que un usuario inicia sesión con un rol asignado, cuando el sistema carga la interfaz de usuario o los permisos de la API, entonces el usuario solo ve y puede acceder a los módulos y acciones permitidos para ese rol.

- [ ] **AC3:** Dado que un usuario intenta acceder a un módulo, vista o acción no permitido por su rol, cuando se procesa la solicitud, entonces el sistema deniega el acceso y registra el intento no autorizado.

- [ ] **AC4:** Dado que un administrador actualiza los permisos de un rol, cuando se guardan los cambios, entonces los nuevos permisos se aplican en las evaluaciones de permisos posteriores (`GET /api/v1/auth/evaluate`) y se genera un evento de auditoría.

- [ ] **AC5:** Dado que el administrador intenta asignar un módulo, vista o acción inválidos, cuando se envía la asignación de permisos, entonces el sistema rechaza la operación y muestra un error de validación.

- [ ] **AC6:** Dado que se han asignado permisos a los roles, cuando un administrador consulta la matriz de permisos, entonces el sistema muestra los permisos asociados a cada rol.

- [ ] **AC7:** Dadas condiciones normales de red, cuando el administrador consulta o actualiza los permisos de un rol, entonces el sistema responde en menos de 5 segundos, según el RNF 1.

---

## Notas Técnicas

- Esta historia implementa la base del Control de Acceso Basado en Roles (RBAC), según el RF 1.1.2.
- La validación de permisos debe aplicarse en el lado del servidor. Ocultar opciones en el cliente no es suficiente.
- El sistema debe soportar una matriz rol-permiso o un modelo de autorización equivalente.
- Los roles considerados en el SRS incluyen:
  - Supervisor.
  - Administrador.
  - Docente / Instructor.
  - Estudiante.
  - Padre de familia o acudiente, si aplica.
  > Catálogo efectivo en la base de datos: `Administrador | Instructor | Aprendiz`
  > (`auth-004-unify-mobile-roles`); los roles son **globales** — `authorization.role` no tiene `school_id`.
- El modelo de permisos debe considerar módulos, vistas y acciones si el sistema requiere un control de acceso granular.
- Los intentos de acceso no autorizado deben registrarse con fines de auditoría.
- Los cambios de permisos deben ser trazables y deben aparecer en el historial de cambios del usuario o en los logs de auditoría.
- El sistema debe evitar la escalada de privilegios, es decir, los usuarios no deben poder otorgarse permisos a sí mismos.

**Servicio(s) responsable(s):** `ms-authorization`.  
**Endpoint(s) implementados:**  
- `GET /api/v1/roles` · `POST /api/v1/roles` · `GET/PUT/DELETE /api/v1/roles/{roleId}` — implementado (`RoleController`).  
- `GET /api/v1/roles/{roleId}/permissions` · `POST /api/v1/roles/{roleId}/permissions` · `DELETE /api/v1/roles/{roleId}/permissions/{permissionId}` — implementado.  
- `GET /api/v1/permissions` — implementado (`PermissionController`).  
- `GET /api/v1/users/{userId}/roles` · `GET /api/v1/auth/evaluate?userId=&permission=` — implementado (`UserRoleController`); **no** existe un endpoint `/users/me/permissions` (los clientes derivan los permisos de la UI desde `/users/{id}/roles`).  

**Eventos generados:**  
- `RolePermissionsUpdated`  
- `UnauthorizedAccessAttempt`  

**Permisos requeridos:**  
- Administrador o Supervisor para la gestión de permisos por rol.  
- Los usuarios regulares solo pueden consultar sus propios permisos efectivos.

---

## Definición de Terminado (DoD)

> Esta HU solo puede cerrarse cuando cumpla con el DoD completo del equipo.

**Verificaciones adicionales específicas para esta HU:**

- [ ] Asignación de permisos por rol probada.
- [ ] Actualización de permisos probada.
- [ ] Intento de acceso no autorizado probado.
- [ ] Consulta de la matriz de permisos probada.
- [ ] Validación de autorización en el servidor probada.
- [ ] Evento de auditoría por cambios de permisos probado.
- [ ] Escenario de asignación de permisos inválida probado.
- [ ] Los datos sensibles de autorización no se exponen en logs.
- [ ] El tiempo de respuesta se valida bajo condiciones normales y se mantiene bajo 5 segundos.
- [ ] Trazabilidad al RF 1.1.2 documentada.

---

## Estimación y Prioridad

| Campo | Valor |
|---|---|
| Story Points | 5 |
| Prioridad | Alta |
| Sprint objetivo | Sprint 1 / MVP |
| Dependencias | HU-IAM-002: Registro de usuario con asignación de rol |
