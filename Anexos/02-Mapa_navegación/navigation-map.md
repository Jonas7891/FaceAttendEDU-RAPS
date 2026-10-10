# Mapa de Navegación — Especificación de Rutas y Navegación (FaceAttend EDU)

> Define la estructura de pantallas del sistema FaceAttend EDU, cómo se conectan entre sí y qué rutas existen.
> Es la referencia cuando frontend y backend discuten qué endpoints existen o cómo llegar a una función.

**Plataformas:** Web + Mobile (ambas construidas con React Navigation).  
**Roles:** Administrador, Docente/Instructor, Estudiante/Alumno.  
**Validación:** Este documento ha sido validado contra la implementación actual en `C:\Users\jonat\OneDrive\Documentos\FaceAttendEDU\ProyectoFaceAttendEDU`.

---

## 1. Nota de Arquitectura (basada en la implementación actual)

- **Web**: Utiliza un stack de navegación de dos niveles: `AppNavigator` (público) $\rightarrow$ `AuthenticatedNavigator` (privado). La barra lateral (`PersistentSidebar`) es un componente persistente fuera del stack interno para mantener el estado durante la navegación.
  - Patrón aplicado: **Navigator $\rightarrow$ Screen $\rightarrow$ View**. Las rutas registran Screens; las Views se encargan de la representación y no gestionan la navegación.

- **Mobile**: Emplea un stack raíz que alterna entre ramas pública y privada basándose en el estado de autenticación (`isAuthenticated`), validando el token en `AsyncStorage` y el `userRole`.

- **Deep linking en Web**: Rutas expuestas `/`, `/login`, `/register`, `/dashboard`, con fallback SPA mediante `nginx try_files`.

---

## 2. Estructura de Rutas del Frontend

### 2.1 Rutas Web (Acceso filtrado por rol)

```
/ ................................. Página de aterrizaje (pública)
                                     [ruta "FaceAttendEDU" → LandingScreen]
├── /login ........................ Autenticación [FaceAttendEDU-Login]
├── /register ..................... Registro [FaceAttendEDU-Register]
│
└── /dashboard .................... Área autenticada [FaceAttendEDU-Dashboard]
    │  = AuthenticatedNavigator: CollapsibleSidebar persistente + stack interno:
    ├── Dashboard ................. KPIs, Métricas y Listas de Alerta
    ├── Users ..................... Gestión de Usuarios (Admin) / Alumnos (Docente) / Compañeros (Estudiante)
    ├── Courses ................... Gestión de Cohortes y Cursos
    ├── Environments .............. Gestión de Ambientes/Salones (SÓLO ADMIN)
    ├── Reports ................... Reportes de Asistencia y Exportación (Admin/Docente)
    └── Settings .................. Ajustes de Usuario y Sistema (según rol)
```

### 2.2 Rutas Mobile (Native Stack)

**Rama desautenticada:**
- `HomesScreen` $\rightarrow$ Login principal.
- `ForgotPasswordScreen` $\rightarrow$ Recuperación de cuenta $\rightarrow$ `VerifyCodeScreen` $\rightarrow$ Cambio de contraseña.

**Rama autenticada:**
- `DashboardScreen`: Panel principal con acciones rápidas segmentadas por rol.
- `Menu`: Menú lateral (Perfil, Actualización Facial, Justificaciones, Configuración).
- `Profile`: Perfil de usuario y cierre de sesión.
- `Novedades`: Feed de noticias y alertas.
- `RegisterFace` $\rightarrow$ `SuccessScreen`: Flujo de inscripción facial.
- `UpdatePhoto`: Solicitud de actualización de rostro.
- `FacialFail`: Pantalla de error de reconocimiento $\rightarrow$ Reintento o Justificación.
- `DisplayingAttendance`: Vista de asistencia personal.
- `AttendanceReportScreen`: Reportes rápidos (Admin).
- `MenuJustify`:
    - `AddJustification`: Envío de nueva justificación.
    - `ConsultJustify`: Historial de envíos.
    - `PendingJustificationScreen`: Cola de validación (Docente).
    - `AddValidJustification`: Aprobación/Rechazo de justificación (Docente).
    - `ValidJustifications`: Historial de resoluciones.
- `Ajustes`: `LanguageSettingsScreen` y `AppearanceSettingsScreen`.
- `Pantallas Admin`: `ManageUsersScreen`, `ManageEnviromentScreen`, `SchoolConfigurationScreen`.

---

## 3. Mapa de Pantallas y Servicios

### 3.1 Pantallas Web $\rightarrow$ Backend (Kong `:8080`)

| Pantalla | Ruta | Rol Mínimo | Servicio Backend |
|----------|------|------------|------------------|
| Landing Page | `/` | Público | — |
| Login | `/login` | Público | `01-ms-identity` |
| Signup | `/register` | Público | `01-ms-identity` |
| Dashboard | `/dashboard` | Estudiante+ | `05-ms-attendance`, `04-ms-scheduling` |
| Users | Pestaña `users` | Estudiante+ | `01-ms-identity`, `02-ms-authorization`, `06-ms-biometric` |
| Courses | Pestaña `courses` | Estudiante+ | `03-ms-academic` |
| Environments | Pestaña `environments`| **ADMIN** | `03-ms-academic`, `04-ms-scheduling` |
| Reports | Pestaña `reports` | Docente+ | `05-ms-attendance`, `08-ms-notification` |
| Settings | Pestaña `settings` | Estudiante+ | `07-ms-configuration`, `09-ms-quality` |

### 3.2 Pantallas Mobile $\rightarrow$ Backend

| Pantalla | Componente | Rol | Servicio |
|----------|------------|-----|----------|
| Login | `Login.js` | Público | `01-ms-identity` |
| Forgot Password | `Forgotpasswordscreen.js` | Público | `01-ms-identity` |
| Dashboard | `DashboardScreen.js` | Todos | `05`, `04`, `03` |
| News | `NewsScreen.js` | Todos | `08-ms-notification` |
| Profile | `ProfileScreen.js` | Todos | `01-ms-identity` |
| Register Face | `RegisterFace.js` | Estudiante, Docente | `06-ms-biometric` |
| Update Face | `UpdatePhotoScreen.js` | Estudiante, Docente | `06-ms-biometric` |
| My Attendance | `DisplayingAttendance.js` | Todos | `05-ms-attendance` |
| Justifications | `MenuJustifyScreen.js` | Estudiante, Docente | `05-ms-attendance` |
| Manage Users | `ManageUsersScreen.js` | **ADMIN** | `01-ms-identity`, `02-ms-authorization` |
| Manage Envir. | `ManageEnviromentScreen.js`| **ADMIN** | `03-ms-academic` |
| School Config | `SchoolConfigurationScreen.js`| **ADMIN** | `03-ms-academic`, `07-ms-configuration` |

---

## 4. Reglas de Navegación y Seguridad

- **Autenticación**: Todas las rutas privadas requieren un JWT válido emitido por Kong. En Web, el acceso sin sesión redirige a `NotAuthorized` o `/login`. En Mobile, se intercambia el stack raíz.
- **Control de Acceso (RBAC)**: El acceso a pestañas y menús está filtrado por `useRolePermissions.js`. Los roles no reconocidos no tienen acceso a ninguna función privada.
- **Persistencia de Estado**: La barra lateral en Web es persistente para evitar recargas de estado del menú.
- **Validaciones de Borde**: Kong Gateway aplica un *rate-limit* de 100 req/min para proteger los microservicios.

---

## 5. Referencia de Servicios Backend

| Servicio | Host Interno | Responsabilidad Principal |
|----------|--------------|--------------------------|
| `01-ms-identity` | `ms-identity:8081` | Identidad, Sesiones y Credenciales |
| `02-ms-authorization` | `ms-authorization:8082` | Control de Acceso (RBAC) |
| `03-ms-academic` | `ms-academic:8083` | Estructura Académica (Fichas, Cursos) |
| `04-ms-scheduling` | `ms-scheduling:8084` | Horarios y Sesiones de Clase |
| `05-ms-attendance` | `ms-attendance:8085` | Registros de Asistencia y Justificaciones |
| `06-ms-biometric` | `ms-biometric:8086` | Procesamiento Facial y Dactilar |
| `07-ms-configuration` | `ms-configuration:8087` | Parámetros del Sistema y Seguridad |
| `08-ms-notification` | `ms-notification:8088` | Alertas y Notificaciones Event-Driven |
| `09-ms-quality` | `quality-service` | Métricas ISO 25010 / 29110 |
| `99-api-gateway` | Kong `:8080` | Punto de entrada único y Seguridad JWT |
