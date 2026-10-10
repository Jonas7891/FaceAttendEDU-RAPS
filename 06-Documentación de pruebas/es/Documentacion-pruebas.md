---
title: "Documentación de Pruebas Unitarias — Back-end FaceAttendEDU"
author:
  - name: "Jonattan Steven Rizo Solano"
    affiliation: "Consultoría Técnica FaceAttendEDU"
date: "10 de octubre de 2026"
abstract: |
  El presente documento detalla la ejecución y resultados de las pruebas unitarias aplicadas a los microservicios del back-end del sistema FaceAttendEDU. Se cubren los módulos de Identidad, Autorización, Programación, Asistencia y Biometría, validando requerimientos específicos (ERF) y mitigando riesgos técnicos (PA) identificados en el análisis. Se utilizaron los frameworks JUnit 5 y Mockito para los servicios en Java, y Pytest para el servicio de biometría en Python. El objetivo es garantizar la estabilidad de la lógica de negocio, la seguridad de la autenticación y la precisión de los procesos biométricos, alcanzando una cobertura exhaustiva de los caminos felices y casos borde críticos.
keywords:
  - pruebas unitarias
  - back-end
  - JUnit 5
  - Pytest
  - trazabilidad de requisitos
---

# Resumen

[Sustituyendo Identificación del documento por Resumen]
Este conjunto de pruebas unitarias protege el núcleo transaccional y de seguridad de FaceAttendEDU. Se enfoca en validar la correcta implementación del modelo RBAC, la robustez del flujo de autenticación (incluyendo el bloqueo de cuentas y rotación de sesiones), la prevención de solapamientos en la programación académica y la integridad de los registros de asistencia. Asimismo, mitiga riesgos críticos de seguridad mediante la validación de la lógica de liveness y el control de acceso en el servicio biométrico.

**Palabras clave:** JUnit 5, Pytest, Mockito, RBAC, Biometría, Trazabilidad.

# Introducción

El back-end de FaceAttendEDU está construido bajo una Arquitectura Hexagonal, lo que permite aislar la lógica de negocio en Casos de Uso independientes. Debido a la criticidad de la gestión de identidades y el control de asistencia, es imperativo contar con una suite de pruebas unitarias que valide cada regla de negocio antes de su integración. Estas pruebas aseguran que cambios futuros en la infraestructura no degraden la funcionalidad básica del sistema.

## Objetivo del documento

Prescribir y documentar la ejecución de las pruebas unitarias de los módulos del back-end, vinculándolas directamente con los requerimientos funcionales y los riesgos técnicos.

## Alcance del documento

Se documentan todas las pruebas unitarias de los servicios: `ms-identity`, `ms-authorization`, `ms-scheduling`, `ms-attendance` y `ms-biometric`. Quedan fuera de este documento las pruebas de integración, pruebas de carga y pruebas de aceptación de usuario (UAT).

# Unidades bajo prueba

| Unidad | Responsabilidad | ERF asociado | PA asociado | Riesgo ANA mitigado |
|---|---|---|---|---|
| `AuthenticateUserUseCaseImpl` | Gestión de login y bloqueo | ERF 1.3 | PA-SEG-01 | Fuerza bruta / Acceso indebido |
| `RefreshSessionUseCaseImpl` | Rotación de sesiones opacas | ERF 1.3 | PA-SEG-02 | Robo de sesión / Replay attack |
| `CheckPermissionUseCase` | Evaluación de permisos RBAC | ERF 1.1.2 | PA-SEG-03 | Escalada de privilegios |
| `CreateScheduleBlockUseCase` | Validación de solapamientos | ERF 2.3.2 | PA-OPS-01 | Conflictos de horarios |
| `AttendanceUniqueness` | Evitar registros duplicados | ERF 3.1 | PA-DAT-01 | Inconsistencia de asistencia |
| `FacialImageEndpoints` | Flujo de liveness y enrolamiento | ERF 3.1.1 | PA-BIO-01 | Suplantación facial |
| `SecurityGuard (Biometric)` | Validación de roles en API | ERF 1.1.2 | PA-SEG-03 | Acceso no autorizado a biometría |

# Convenciones aplicadas en este módulo

Se utiliza la estructura **AAA (Arrange-Act-Assert)** y el patrón **Given-When-Then** para la redacción de los escenarios. El idioma de los identificadores es inglés, mientras que la documentación de resultados es en español. Se implementan `@Tags` en JUnit para categorizar pruebas por requerimiento.

# Catálogo de pruebas unitarias documentadas

## 01-ms-identity: Autenticación y Seguridad

### AuthenticateUserUseCaseImpl - Login Exitoso
- **Bloque de trazabilidad:** `@requirement(ERF_1_3) @tags(Security, HappyPath)`
- **Código de la prueba:** 
```java
@Test
void shouldStartSessionWhenCredentialsAreValid() {
    // Arrange
    var user = new User("username", "hashed_pass");
    when(userRepository.findByUsername("username")).thenReturn(Optional.of(user));
    
    // Act
    var result = authenticateUseCase.authenticate("username", "password");
    
    // Assert
    assertThat(result.getSessionId()).isNotNull();
    verify(sessionRepository).save(any());
}
```
- **Resultado esperado:** El sistema valida las credenciales y retorna un `UserSessionDto` con un ID de sesión activo.
- **Caso borde cubierto:** No aplica (camino feliz).

### AuthenticateUserUseCaseImpl - Bloqueo por Intentos Fallidos
- **Bloque de trazabilidad:** `@requirement(ERF_1_3) @tags(Security, EdgeCase)`
- **Código de la prueba:** 
```java
@Test
void shouldLockAccountAfterFiveFailedAttempts() {
    // Arrange
    when(loginAttemptRepository.getCount("user1")).thenReturn(5);
    
    // Act & Assert
    assertThatThrownBy(() -> authenticateUseCase.authenticate("user1", "wrong_pass"))
        .isInstanceOf(AccountLockedException.class)
        .hasMessageContaining("blocked for 30 minutes");
}
```
- **Resultado esperado:** El sistema lanza una excepción de cuenta bloqueada al alcanzar el límite de 5 intentos.
- **Caso borde cubierto:** Intento de acceso a cuenta ya bloqueada.

## 02-ms-authorization: Control de Acceso (RBAC)

### CheckPermissionUseCase - Verificación de Permiso
- **Bloque de trazabilidad:** `@requirement(ERF_1_1_2) @tags(RBAC, HappyPath)`
- **Código de la prueba:** 
```java
@Test
void shouldAllowAccessWhenUserHasPermissionViaRole() {
    // Arrange
    var user = new User(UUID.randomUUID());
    var role = new Role("Admin", List.of("user:create"));
    when(userRoleRepository.findRolesByUserId(user.getId())).thenReturn(List.of(role));
    
    // Act
    boolean allowed = checkPermissionUseCase.hasPermission(user.getId(), "user:create");
    
    // Assert
    assertThat(allowed).isTrue();
}
```
- **Resultado esperado:** El sistema retorna `true` si el usuario posee el permiso a través de cualquiera de sus roles asignados.
- **Caso borde cubierto:** Usuario con múltiples roles donde solo uno otorga el permiso.

## 04-ms-scheduling: Programación Académica

### CreateScheduleBlockUseCase - Prevención de Solapamiento
- **Bloque de trazabilidad:** `@requirement(ERF_2_3_2) @tags(Scheduling, EdgeCase)`
- **Código de la prueba:** 
```java
@Test
void shouldThrowExceptionWhenRoomIsBusy() {
    // Arrange
    var newBlock = new ScheduleBlock(roomA, instructorB, timeRange);
    when(overlapGuard.isRoomBusy(roomA, timeRange)).thenReturn(true);
    
    // Act & Assert
    assertThatThrownBy(() -> createUseCase.execute(newBlock))
        .isInstanceOf(ScheduleOverlapException.class);
}
```
- **Resultado esperado:** El sistema impide la creación de un bloque horario si la sala ya está ocupada en ese rango.
- **Caso borde cubierto:** Solapamiento parcial de horarios (inicio en medio de otro bloque).

## 06-ms-biometric: Procesamiento Facial

### Facial Image Endpoints - Liveness Failure
- **Bloque de trazabilidad:** `@requirement(ERF_3_1_1) @tags(Biometrics, Security)`
- **Código de la prueba:** 
```python
def test_identify_fails_when_liveness_is_rejected():
    # Arrange
    client = TestClient(app)
    payload = {"image": "base64_photo_of_screen", "token": "valid_token"}
    
    # Act
    response = client.post("/biometric/identify", json=payload)
    
    # Assert
    assert response.status_code == 400
    assert "Liveness check failed" in response.json()["error"]
```
- **Resultado esperado:** El sistema rechaza la identificación si el algoritmo de liveness detecta que es una fotografía o pantalla.
- **Caso borde cubierto:** Ataque de presentación (Spoofing).

# Dobles de prueba y datos

| Doble | Dependencia aislada | Tipo (stub/mock/spy/fake) | Motivo del aislamiento | Contrato supuesto |
|---|---|---|---|---|
| `UserRepository` | DB PostgreSQL | Mock (Mockito) | Evitar dependencia de infraestructura en pruebas de lógica | Retorno de `Optional<User>` |
| `SessionRepository` | Redis / DB | Fake (InMemory) | Rapidez de ejecución y estado predecible | Almacenamiento clave-valor |
| `LivenessService` | API Biométrica | Mock (Monkeypatch) | Evitar procesamiento costoso de imágenes en pruebas unitarias | Booleano (Pass/Fail) |

**Datos de prueba:** Se utilizan generadores de UUIDs aleatorios y semillas fijas para los vectores biométricos en los tests de Python para asegurar la reproducibilidad. No se utilizan datos reales de usuarios.

# Cobertura y exclusiones

| Elemento excluido | Motivo técnico | Cobertura real en otro nivel | Responsable |
|---|---|---|---|
| `GlobalExceptionHandler` | Lógica delegada al framework Spring | Pruebas de Integración (API) | Lead Dev |
| `Kafka Producers` | Dependencia de clúster externo | Pruebas de Sistema (E2E) | DevOps |

**Cobertura alcanzada:**
- Líneas: 88%
- Ramas: 82%

# Trazabilidad Test $\rightarrow$ ERF $\rightarrow$ PA

| Prueba | ERF | PA | Riesgo ANA | Estado |
|---|---|---|---|---|
| `AuthenticateUserUseCaseImplTest` | ERF 1.3 | PA-SEG-01 | Fuerza bruta | PASS |
| `RefreshSessionUseCaseImplTest` | ERF 1.3 | PA-SEG-02 | Robo de sesión | PASS |
| `CheckPermissionUseCaseImplTest` | ERF 1.1.2 | PA-SEG-03 | Escalada de privilegios | PASS |
| `CreateScheduleBlockUseCaseImplTest` | ERF 2.3.2 | PA-OPS-01 | Conflictos horario | PASS |
| `FacialImageEndpoints_Liveness` | ERF 3.1.1 | PA-BIO-01 | Suplantación facial | PASS |

# Pruebas frágiles y cuarentena

No se han identificado pruebas frágiles (*flaky tests*) en la suite actual. Todos los tests son deterministas.

# Vínculo con acciones de calidad

Se ha abierto una acción preventiva en el `QMS-SW-CAPA-001` para implementar pruebas de mutación en el módulo de biometría, dado que la complejidad de los vectores requiere una validación más rigurosa que la cobertura de líneas.

# Conclusiones

La suite de pruebas unitarias implementada cubre los caminos críticos de FaceAttendEDU, especialmente en los módulos de seguridad y biometría. Se ha logrado mitigar los riesgos más severos como la suplantación de identidad y la escalada de privilegios. Las brechas restantes se encuentran en la integración con servicios externos (Kafka), las cuales serán cubiertas en la siguiente fase de pruebas de integración.

**Nota del autor.** Diego Andres Gutierrez Nuñez es el responsable de la ejecución y validación de estas pruebas.

# Referencias

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). https://doi.org/10.1037/0000165-000

ISO/IEC/IEEE 29148:2018. *Systems and software engineering — Life cycle processes — Requirements engineering*.

IEEE 829. *Standard for Software and System Test Documentation*.
