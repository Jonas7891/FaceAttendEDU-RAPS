# Manual de Administrador

**FaceAttend EDU**

Integrantes:

Juan David Arboleda Perdomo

Diego Andrés Gutiérrez Nuñez

Jonathan Steven Rizo Solano

Centro de la Industria, la Empresa y los Servicios - SENA

Tecnólogo en Análisis y Desarrollo de Software — Ficha 3145556

Karol Daniela Correa

6 de octubre de 2026

Versión del documento 1.0 — Versión del software documentada: 1.0.0

# **Control de Versiones**

Toda modificación a este manual debe quedar registrada antes de volver a entregar el documento. La versión del manual debe corresponder siempre con la versión del software que describe.

**Tabla 1**

*Historial de versiones del documento*

| **Versión** | **Fecha**  | **Descripción del cambio**                                             | **Responsable**      |
|-------------|------------|------------------------------------------------------------------------|----------------------|
| 1.0         | 06/10/2026 | Creación inicial del manual de administrador.                          | Equipo desarrollador |
| 1.1         | 06/10/2026 | Aplicación de normas APA séptima edición y supresión del uso de color. | Equipo desarrollador |
| [ ]       | [ ]      | [Registrar aquí el siguiente cambio.]                                | [ ]                |

*Nota.* Los cambios menores, como ajustes de redacción o incorporación de capturas, incrementan el segundo dígito. Los cambios funcionales, como la incorporación de un módulo administrativo o la modificación de permisos, incrementan el primer dígito.

## **Regla de Actualización**

- El manual se revisa obligatoriamente después de cada despliegue que modifique pantallas, roles o reportes.

- Cada versión entregada debe quedar registrada en la tabla anterior con su responsable.

# **Tabla de Contenido**

[**Control de Versiones**](#_heading=) **2**

> [Regla de Actualización](#_heading=) 2

[**Tabla de Contenido**](#_heading=) **3**

[**Introducción**](#_heading=) **6**

[**Objetivo**](#_heading=) **8**

> [Objetivos Específicos](#_heading=) 8

[**Alcance**](#_heading=) **9**

> [Contenido Cubierto por el Manual](#_heading=) 9
>
> [Contenido No Cubierto por el Manual](#_heading=) 9

[**Perfil del Administrador**](#_heading=) **11**

> [Responsabilidades del Administrador](#_heading=) 11
>
> [Responsabilidades Diarias](#_heading=) 11
>
> [Responsabilidades Semanales](#_heading=) 11
>
> [Responsabilidades Mensuales](#_heading=) 12
>
> [Responsabilidades por Evento](#_heading=) 12
>
> [Límites del Rol](#_heading=) 12

[**Acceso al Panel de Administración**](#_heading=) **13**

> [Procedimiento de Ingreso](#_heading=) 13

[**Gestión de Usuarios**](#_heading=) **15**

> [Crear un Usuario](#_heading=) 15
>
> [Editar un Usuario](#_heading=) 16
>
> [Activar o Desactivar un Usuario](#_heading=) 16
>
> [Restablecer la Contraseña de un Usuario](#_heading=) 17
>
> [Consultar y Filtrar Usuarios](#_heading=) 17

[**Gestión de Roles y Permisos**](#_heading=) **19**

> [Asignar o Cambiar un Rol](#_heading=) 19
>
> [Matriz de Permisos por Operación](#_heading=) 20

[**Configuración del Sistema**](#_heading=) **21**

> [Procedimiento para Modificar un Parámetro](#_heading=) 21
>
> [Parámetros Externos al Panel](#_heading=) 22

[**Gestión de la Información Principal**](#_heading=) **23**

> [Administrar Fichas](#_heading=) 23
>
> [Crear una Ficha](#_heading=) 23
>
> [Reglas de Administración de Fichas](#_heading=) 23
>
> [Administrar Cursos](#_heading=) 24
>
> [Administrar Horarios](#_heading=) 24
>
> [Gestión de Novedades de Asistencia](#_heading=) 25
>
> [Módulos Pendientes de Documentar](#_heading=) 26

[**Reportes Administrativos**](#_heading=) **27**

> [Generar y Exportar un Reporte](#_heading=) 27
>
> [Métricas de Seguimiento](#_heading=) 28
>
> [Buenas Prácticas en el Manejo de Reportes](#_heading=) 29

[**Copias de Seguridad**](#_heading=) **30**

> [Procedimiento desde el Panel](#_heading=) 30
>
> [Procedimiento Alterno por Línea de Comandos](#_heading=) 31
>
> [Almacenamiento de los Respaldos](#_heading=) 32

[**Restauración de Información**](#_heading=) **33**

> [Procedimiento de Restauración](#_heading=) 33
>
> [Verificación Posterior a la Restauración](#_heading=) 34
>
> [Procedimiento Alterno por Línea de Comandos](#_heading=) 34

[**Auditoría e Historial de Acciones**](#_heading=) **36**

> [Consultar el Historial](#_heading=) 36
>
> [Revisión Periódica](#_heading=) 37
>
> [Acciones No Registradas por el Sistema](#_heading=) 37

[**Recomendaciones de Seguridad y Procedimientos de Apoyo**](#_heading=) **38**

> [Seguridad de las Cuentas](#_heading=) 38
>
> [Seguridad de la Información](#_heading=) 38
>
> [Comunicación con los Usuarios](#_heading=) 38
>
> [Gestión de Incidentes y Escalamiento](#_heading=) 39
>
> [Gestión de Cambios](#_heading=) 40
>
> [Riesgos Conocidos y Plan de Contingencia](#_heading=) 41

[**Glosario**](#_heading=) **42**

[**Referencias**](#_heading=) **43**

[**Apéndice A**](#_heading=) **44**

[**Apéndice B**](#_heading=) **45**

[**Apéndice C**](#_heading=) **46**

[**Apéndice D**](#_heading=) **47**

[**Apéndice E**](#_heading=) **48**

[**Apéndice F**](#_heading=) **49**

# **Introducción**

FaceAttend EDU es un sistema construido para registrar y controlar la asistencia dentro de un entorno de formación. El sistema reemplaza el registro manual de asistencia por un flujo digital, de manera que la información queda centralizada, consultable y exportable.

Este manual explica cómo administrar el sistema. No explica cómo usarlo como aprendiz ni cómo está escrito el código fuente; esa información se encuentra en el manual de usuario y en el manual técnico, respectivamente. La estructura del documento sigue la guía institucional para la elaboración de manuales de software (Servicio Nacional de Aprendizaje, 2026).

El documento está redactado para que una persona que no participó en el desarrollo pueda asumir la administración del sistema leyéndolo completo. Cada procedimiento indica dónde se encuentra la opción, qué pasos seguir y qué resultado debe producirse. El manual no contiene contraseñas, llaves privadas, tokens ni credenciales de producción.

**Tabla 2**

*Destinatarios del manual y secciones aplicables*

| **Lector**                                | **Qué encuentra en este manual**                                                                          |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| Super Administrador                       | La totalidad del documento, incluidas las secciones de configuración, copias de seguridad y restauración. |
| Administrador                             | Gestión de usuarios, fichas, cursos, horarios, reportes y auditoría.                                      |
| Instructor con funciones delegadas        | Las secciones 9, 12 y 13, únicamente para los grupos que tiene asignados.                                 |
| Persona que recibe el relevo del proyecto | El documento completo, comenzando por las secciones 7 y 8.                                                |

**Tabla 3**

*Documentos complementarios del proyecto*

| **Documento**           | **Pregunta que responde**           | **Cuándo consultarlo**                                            |
|-------------------------|-------------------------------------|-------------------------------------------------------------------|
| Manual de usuario       | ¿Cómo uso el sistema?               | Cuando un usuario final pregunta por una pantalla o un mensaje.   |
| Manual técnico          | ¿Cómo está construido el sistema?   | Cuando se requiere entender servicios, base de datos o endpoints. |
| Manual de instalación   | ¿Cómo instalo y ejecuto el sistema? | Cuando se levanta el sistema en un equipo o servidor nuevo.       |
| Manual de administrador | ¿Cómo administro el sistema?        | Este documento. Operación del sistema ya instalado.               |

# **Objetivo**

Orientar al administrador en la gestión, configuración y supervisión de FaceAttend EDU, garantizando un uso seguro y organizado de la plataforma.

## **Objetivos Específicos**

1.  Describir el perfil, las responsabilidades y los límites del rol administrador dentro del sistema.

2.  Documentar el procedimiento de acceso al panel de administración.

3.  Establecer los procedimientos de creación, modificación, desactivación y restablecimiento de cuentas de usuario.

4.  Definir los roles existentes y los permisos asociados a cada uno.

5.  Explicar la administración de fichas, cursos y horarios.

6.  Documentar la generación, consulta y exportación de reportes de asistencia.

7.  Establecer el procedimiento de copias de seguridad y de restauración para las dos bases de datos del sistema.

8.  Definir las prácticas de seguridad que el administrador debe cumplir.

# **Alcance**

## **Contenido Cubierto por el Manual**

- Administración de cuentas de usuario y asignación de roles.

- Administración de fichas, cursos y horarios.

- Consulta, filtrado y exportación de reportes de asistencia.

- Parámetros de configuración general del sistema.

- Copias de seguridad y restauración de PostgreSQL y MongoDB.

- Consulta del historial de acciones administrativas.

- Procedimientos de atención de incidentes y escalamiento.

## **Contenido No Cubierto por el Manual**

- Instalación, despliegue y configuración inicial de los microservicios, descritos en el manual de instalación.

- Arquitectura interna, modelo de datos detallado y endpoints, descritos en el manual técnico.

- Uso cotidiano del sistema por parte de aprendices e instructores, descrito en el manual de usuario.

- Administración de la infraestructura del servidor o del contenedor donde se ejecuta el sistema.

- Modificación del código fuente.

Todo procedimiento de este manual se ejecuta sobre el sistema ya instalado y en funcionamiento. Si el sistema no está instalado, se debe aplicar primero el manual de instalación.

# **Perfil del Administrador**

El administrador es la persona responsable de que el sistema esté disponible, de que cada usuario tenga el acceso que le corresponde y de que la información de asistencia sea confiable.

**Tabla 4**

*Perfil requerido para el rol de administrador*

| **Aspecto**                 | **Descripción**                                                                                                                |
|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------|
| Conocimiento del sistema    | Debe conocer los módulos de usuarios, fichas, cursos, horarios y reportes.                                                     |
| Conocimiento técnico mínimo | Manejo de navegador, descarga y almacenamiento de archivos, y nociones básicas de bases de datos para interpretar un respaldo. |
| Criterio institucional      | Debe conocer la estructura de fichas, programas y jornadas del centro de formación.                                            |
| Responsabilidad sobre datos | Maneja datos personales de aprendices e instructores, por lo que responde por su confidencialidad.                             |

## **Responsabilidades del Administrador**

### ***Responsabilidades Diarias***

- Verificar que el sistema responde y que el registro de asistencia está operando.

- Atender las solicitudes de acceso: creación de cuentas, restablecimiento de contraseña y desbloqueo.

- Revisar las novedades reportadas por instructores sobre asistencias no registradas.

### ***Responsabilidades Semanales***

- Revisar el listado de usuarios activos y desactivar las cuentas que ya no corresponden a personas vinculadas al proceso.

- Generar y archivar el reporte de asistencia del periodo.

- Verificar que la copia de seguridad de la semana se generó y se almacenó correctamente.

### ***Responsabilidades Mensuales***

- Revisar el historial de acciones administrativas en busca de operaciones no justificadas.

- Validar que la configuración del sistema sigue correspondiendo a la realidad del centro de formación.

- Verificar que este manual sigue coincidiendo con las pantallas y procedimientos reales del sistema.

### ***Responsabilidades por Evento***

- Ejecutar una copia de seguridad antes de cualquier cambio importante o despliegue.

- Coordinar la restauración de información cuando se presenta una pérdida de datos.

- Escalar al equipo técnico los incidentes que no se resuelven desde el panel de administración.

## **Límites del Rol**

El administrador no debe realizar las siguientes acciones:

- Modificar directamente los registros de asistencia para alterar un resultado académico.

- Compartir su cuenta con otra persona o usar una cuenta genérica compartida.

- Eliminar usuarios con historial de asistencia. En su lugar se desactiva la cuenta, de modo que el historial se conserve.

- Modificar el código fuente o la base de datos directamente sin autorización del líder técnico.

# **Acceso al Panel de Administración**

El panel de administración es la interfaz desde la cual se ejecutan todos los procedimientos de este manual. El acceso depende del rol asignado a la cuenta.

**Tabla 5**

*Requisitos previos para el acceso al panel de administración*

| **Requisito**          | **Detalle**                                                                                       |
|------------------------|---------------------------------------------------------------------------------------------------|
| Navegador              | Google Chrome, Microsoft Edge o Mozilla Firefox en versión actualizada.                           |
| Conexión               | Acceso a la red donde se encuentra publicado el sistema.                                          |
| Cuenta                 | Usuario con rol Super Administrador o Administrador, en estado Activo.                            |
| Dirección del sistema  | http://localhost:8090                                                    |
| Servicios en ejecución | Los microservicios del sistema y las bases de datos PostgreSQL y MongoDB deben estar disponibles. |

## **Procedimiento de Ingreso**

1.  Abrir el navegador e ingresar la dirección del sistema.

2.  En la pantalla de inicio de sesión, escribir el correo institucional y la contraseña.

3.  Hacer clic en el botón Iniciar sesión.

4.  Verificar que el sistema muestre el panel correspondiente al rol administrador.

5.  Confirmar que el menú de administración está visible.

**Resultado esperado:** el sistema presenta el panel de administración con el nombre del usuario y el rol en la parte superior.

Las capturas de pantalla correspondientes a este procedimiento se incorporan en el Apéndice E.

**Tabla 6**

*Problemas frecuentes de acceso y acciones correctivas*

| **Situación**                                           | **Causa probable**                               | **Acción del administrador**                                                                  |
|---------------------------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------------------------------|
| El sistema indica credenciales inválidas                | Correo o contraseña incorrectos.                 | Verificar el correo. Si persiste, restablecer la contraseña según la sección 9.               |
| El sistema indica que la cuenta está inactiva           | La cuenta fue desactivada.                       | Reactivar la cuenta si corresponde, según la sección 9.                                       |
| El usuario ingresa pero no ve el menú de administración | El rol asignado no es administrativo.            | Revisar el rol de la cuenta en la sección 10 y corregirlo si corresponde.                     |
| La página no carga                                      | Servicio detenido o dirección incorrecta.        | Verificar la disponibilidad de los servicios y escalar al equipo técnico según la sección 17. |
| La sesión se cierra sola                                | Se cumplió el tiempo de inactividad configurado. | Volver a iniciar sesión y revisar el parámetro de tiempo de sesión en la sección 11.          |

# **Gestión de Usuarios**

El módulo de usuarios permite crear, consultar, editar, activar, desactivar y restablecer la contraseña de las cuentas del sistema. Es el módulo de uso más frecuente para el administrador.

**Tabla 7**

*Campos que componen una cuenta de usuario*

| **Campo**              | **Obligatorio** | **Observación**                                                  |
|------------------------|-----------------|------------------------------------------------------------------|
| Nombres y apellidos    | Sí              | Deben coincidir con el documento de identidad de la persona.     |
| Número de documento    | Sí              | Es único en el sistema. No se puede registrar dos veces.         |
| Correo                 | Sí              | Es el dato con el que la persona inicia sesión. Debe ser único.  |
| Teléfono               | No              | Se usa para contacto en caso de novedad.                         |
| Rol                    | Sí              | Define qué puede hacer la persona. Véase la sección 10.          |
| Estado                 | Sí              | Activo o Inactivo. Determina si la persona puede iniciar sesión. |
| Ficha o grupo asociado | Según el rol    | Aplica para aprendices e instructores.                           |

## **Crear un Usuario**

1.  Ingresar con una cuenta de administrador.

2.  Seleccionar el menú Administración y luego la opción Usuarios.

3.  Hacer clic en el botón Nuevo usuario.

4.  Diligenciar nombres, apellidos, número de documento, correo y teléfono.

5.  Seleccionar el rol que corresponde a la función real de la persona.

6.  Asociar la ficha o grupo cuando el rol lo requiera.

7.  Definir el estado como Activo.

8.  Hacer clic en Guardar.

9.  Verificar que el usuario aparezca en el listado y que el sistema muestre el mensaje de registro exitoso.

**Resultado esperado:** el usuario queda creado y puede acceder al sistema según el rol asignado.

Antes de crear una cuenta se debe buscar el número de documento en el listado. Si la persona ya existe con estado Inactivo, se debe reactivar la cuenta existente en lugar de crear una nueva, dado que un registro duplicado divide el historial de asistencia de esa persona.

## **Editar un Usuario**

1.  Ingresar al menú Administración y luego a Usuarios.

2.  Buscar la persona por nombre, documento o correo.

3.  Hacer clic en la opción Editar de la fila correspondiente.

4.  Modificar únicamente los campos que requieren corrección.

5.  Hacer clic en Guardar y confirmar el mensaje de actualización exitosa.

El cambio de rol y el cambio de estado quedan registrados en el historial de acciones descrito en la sección 16.

## **Activar o Desactivar un Usuario**

La desactivación es el procedimiento estándar cuando una persona deja de pertenecer al proceso formativo, pues conserva el historial de asistencia y bloquea el acceso.

1.  Ingresar al listado de usuarios.

2.  Buscar la persona.

3.  Cambiar el estado a Inactivo.

4.  Guardar el cambio.

5.  Verificar que la cuenta ya no permite iniciar sesión.

**Tabla 8**

*Efectos de la activación, reactivación y eliminación de cuentas*

| **Acción** | **Cuándo se aplica**                                             | **Efecto sobre el historial**                                         |
|------------|------------------------------------------------------------------|-----------------------------------------------------------------------|
| Desactivar | La persona finalizó su proceso, se retiró o cambió de rol.       | El historial de asistencia se conserva íntegro.                       |
| Reactivar  | La persona regresa al proceso formativo.                         | Se recupera el historial anterior de la misma cuenta.                 |
| Eliminar   | Solo para registros creados por error y sin asistencia asociada. | Se pierde el registro. Requiere autorización del Super Administrador. |

## **Restablecer la Contraseña de un Usuario**

1.  Ingresar al listado de usuarios y buscar la persona.

2.  Seleccionar la opción Restablecer contraseña.

3.  Confirmar la acción.

4.  Informar a la persona, por un canal verificable, que debe establecer una contraseña nueva en su primer ingreso.

El administrador nunca debe conocer, escribir ni almacenar la contraseña definitiva de otro usuario. El restablecimiento obliga al usuario a definir su propia contraseña.

## **Consultar y Filtrar Usuarios**

- Filtrar por rol para revisar cuántas cuentas administrativas existen.

- Filtrar por estado para identificar cuentas activas que ya no deberían estarlo.

- Filtrar por ficha o grupo para validar la conformación de un grupo antes de iniciar un periodo.

- Buscar por documento o correo para atender una solicitud puntual de soporte.

# **Gestión de Roles y Permisos**

FaceAttend EDU maneja cuatro roles. Cada rol determina qué módulos ve la persona y qué acciones puede ejecutar. El principio de asignación consiste en otorgar únicamente los permisos que la función real de la persona requiere.

**Tabla 9**

*Roles del sistema, permisos principales y restricciones*

| **Rol**       | **Permisos principales**                                                                                                                                  | **Restricciones**                                                                                            |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| Super Admin   | Control total del sistema. Gestiona administradores, configuración general, copias de seguridad y restauración. Accede al historial completo de acciones. | Debe existir un número mínimo de cuentas con este rol. No se comparte entre personas.                        |
| Administrador | Gestiona usuarios, fichas, cursos, horarios y reportes. Consulta el historial de acciones.                                                                | No ejecuta restauraciones de base de datos ni modifica parámetros críticos sin autorización del Super Admin. |
| Instructor    | Consulta y gestiona la asistencia de las fichas y cursos que tiene asignados. Genera reportes de sus grupos.                                              | No crea ni elimina usuarios. No accede a información de grupos que no tiene asignados.                       |
| Aprendiz      | Consulta su propia asistencia e información personal.                                                                                                     | No accede a información de otros usuarios. No genera reportes administrativos.                               |

## **Asignar o Cambiar un Rol**

1.  Ingresar al menú Administración y luego a Usuarios.

2.  Buscar la persona y seleccionar Editar.

3.  Cambiar el campo Rol al valor que corresponde.

4.  Guardar el cambio.

5.  Pedir a la persona que cierre sesión y vuelva a ingresar para que los permisos se apliquen.

6.  Verificar con la persona que ve los módulos esperados.

Un cambio de rol modifica de inmediato lo que la persona puede ver y hacer. Antes de elevar un rol a Administrador o Super Admin debe existir una solicitud explícita del responsable del proyecto. Este cambio queda registrado en el historial de acciones.

## **Matriz de Permisos por Operación**

La siguiente matriz resume quién puede ejecutar cada operación. Debe validarse contra el comportamiento real del sistema y ajustarse si este cambia.

**Tabla 10**

*Matriz de permisos por operación y rol*

| **Operación**                       | **Super Admin** | **Administrador**     | **Instructor**  | **Aprendiz** |
|-------------------------------------|-----------------|-----------------------|-----------------|--------------|
| Crear usuario                       | Sí              | Sí                    | No              | No           |
| Cambiar rol de un usuario           | Sí              | Sí, salvo Super Admin | No              | No           |
| Desactivar usuario                  | Sí              | Sí                    | No              | No           |
| Crear ficha o curso                 | Sí              | Sí                    | No              | No           |
| Asignar horario                     | Sí              | Sí                    | No              | No           |
| Registrar o ajustar asistencia      | Sí              | Sí                    | Solo sus grupos | No           |
| Generar reporte de asistencia       | Sí              | Sí                    | Solo sus grupos | Solo propia  |
| Exportar reporte                    | Sí              | Sí                    | Solo sus grupos | No           |
| Modificar configuración del sistema | Sí              | Parcial               | No              | No           |
| Generar copia de seguridad          | Sí              | Según autorización    | No              | No           |
| Restaurar información               | Sí              | No                    | No              | No           |
| Consultar historial de acciones     | Sí              | Sí                    | No              | No           |

*Nota.* El valor Parcial indica que el rol accede a algunos parámetros y no a otros. El detalle se especifica en la sección 11.

# **Configuración del Sistema**

La configuración define los parámetros generales con los que opera el sistema. Un cambio en esta sección afecta a todos los usuarios, por lo que cada modificación debe registrarse y comunicarse.

**Tabla 11**

*Parámetros administrables del sistema*

| **Parámetro**             | **Descripción**                                                                                  | **Valor de referencia**                        |
|---------------------------|--------------------------------------------------------------------------------------------------|------------------------------------------------|
| Nombre de la institución  | Dato que aparece en encabezados y reportes.                                                      | Centro de la Industria, la Empresa y los Servicios — SENA  |
| Tiempo de sesión          | Minutos de inactividad antes de cerrar la sesión automáticamente.                                | 30 minutos                                     |
| Estados de asistencia     | Valores permitidos al registrar la asistencia.                                                   | Presente, Ausente, Tarde, Excusa               |
| Tolerancia de ingreso     | Minutos después de la hora de inicio dentro de los cuales el registro aún se considera a tiempo. | Lo define el centro de formación |
| Jornadas                  | Franjas horarias en las que opera la formación.                                                  | Mañana, Tarde, Noche                           |
| Formatos de exportación   | Formatos disponibles para descargar reportes.                                                    | PDF y Excel                                    |
| Periodo académico vigente | Rango de fechas sobre el cual se consolidan los reportes.                                        | Lo define el centro de formación              |

## **Procedimiento para Modificar un Parámetro**

1.  Generar una copia de seguridad antes del cambio, según la sección 14.

2.  Ingresar al menú Administración y luego a Configuración.

3.  Localizar el parámetro que se requiere modificar.

4.  Registrar en una nota el valor anterior, por si se debe revertir.

5.  Aplicar el nuevo valor y guardar.

6.  Verificar el efecto del cambio con una prueba concreta.

7.  Informar a los usuarios afectados según la sección 17.

## **Parámetros Externos al Panel**

Los siguientes elementos pertenecen a la configuración de despliegue y no se modifican desde el panel de administración. Cualquier cambio sobre ellos corresponde al equipo técnico y se documenta en el manual de instalación y en el manual técnico.

**Figura 1**

*Estructura de las variables de entorno del sistema*

> POSTGRES_HOST=...
>
> POSTGRES_PORT=...
>
> POSTGRES_DB=...
>
> POSTGRES_USER=...
>
> POSTGRES_PASSWORD=********
>
> MONGO_URI=...
>
> MONGO_DB=...
>
> JWT_SECRET=********
>
> SESSION_TIMEOUT=...

*Nota.* La figura presenta únicamente la estructura de las variables. El manual no debe publicar contraseñas, llaves privadas, tokens ni cadenas de conexión reales.

# **Gestión de la Información Principal**

La información principal de FaceAttend EDU está compuesta por las fichas, los cursos y los horarios. De su correcta configuración depende que el registro de asistencia se asocie al grupo y a la sesión correctos.

**Figura 2**

*Orden de configuración de la información principal*

> 1. Ficha (grupo de formación)
>
> |
>
> 2. Curso (se asocia a una ficha)
>
> |
>
> 3. Horario (se asocia a un curso y a un instructor)
>
> |
>
> 4. Aprendices (se asocian a la ficha)
>
> |
>
> 5. Registro de asistencia (se genera sobre el horario)

*Nota.* Los elementos tienen dependencias entre sí. Configurarlos en un orden distinto produce errores de asignación.

## **Administrar Fichas**

### ***Crear una Ficha***

1.  Ingresar al menú Administración y luego a Fichas.

2.  Hacer clic en Nueva ficha.

3.  Diligenciar número de ficha, nombre del programa, jornada y fechas de inicio y fin.

4.  Guardar y verificar que la ficha aparezca en el listado.

### ***Reglas de Administración de Fichas***

- El número de ficha es único y no se debe registrar dos veces.

- Una ficha con asistencia registrada no se elimina; se cierra o se marca como finalizada.

- Al finalizar una ficha se verifica que los reportes del periodo ya fueron generados y archivados.

## **Administrar Cursos**

1.  Ingresar al menú Administración y luego a Cursos.

2.  Hacer clic en Nuevo curso.

3.  Diligenciar el nombre del curso o competencia y la descripción.

4.  Asociar el curso a la ficha correspondiente.

5.  Asignar el instructor responsable.

6.  Guardar y verificar el registro en el listado.

**Resultado esperado:** el curso queda disponible para la asignación de horarios y el instructor asignado lo visualiza en su panel.

## **Administrar Horarios**

1.  Ingresar al menú Administración y luego a Horarios.

2.  Hacer clic en Nuevo horario.

3.  Seleccionar el curso.

4.  Definir los días de la semana, la hora de inicio y la hora de finalización.

5.  Confirmar el ambiente o aula cuando el sistema lo solicite.

6.  Guardar y verificar que no exista cruce con otro horario del mismo instructor o del mismo grupo.

**Tabla 12**

*Validaciones aplicables a la asignación de horarios*

| **Validación**                  | **Qué revisar**                                                  | **Acción si falla**                       |
|---------------------------------|------------------------------------------------------------------|-------------------------------------------|
| Cruce de horario del grupo      | Que la ficha no tenga dos sesiones simultáneas.                  | Ajustar la franja de una de las sesiones. |
| Cruce de horario del instructor | Que el instructor no esté asignado a dos grupos a la misma hora. | Reasignar instructor o cambiar la franja. |
| Coherencia con la jornada       | Que la franja corresponda a la jornada de la ficha.              | Corregir la jornada o la franja.          |
| Vigencia                        | Que el horario esté dentro de las fechas de la ficha.            | Ajustar fechas antes de guardar.          |

## **Gestión de Novedades de Asistencia**

Cuando un instructor o un aprendiz reporta que una asistencia no quedó registrada o quedó mal registrada, el administrador aplica el siguiente procedimiento.

1.  Recibir la solicitud por el canal oficial, con fecha, grupo, nombre del aprendiz y descripción de la novedad.

2.  Consultar el registro en el reporte de asistencia correspondiente.

3.  Verificar la novedad con el instructor responsable de la sesión.

4.  Aplicar el ajuste en el sistema únicamente si la novedad queda confirmada.

5.  Registrar en la bitácora de novedades la fecha, el responsable y la justificación del ajuste.

6.  Informar al solicitante que la novedad fue atendida.

Todo ajuste manual de asistencia debe quedar justificado y registrado. Un ajuste sin soporte compromete la confiabilidad del sistema y la del administrador.

## **Módulos Pendientes de Documentar**

Si el sistema implementa el enrolamiento de rostros, el registro de asistencia por reconocimiento facial u otros módulos administrativos adicionales, el equipo debe documentarlos en esta sección con el mismo formato empleado en las secciones anteriores: pasos numerados, resultado esperado, validaciones y captura real. No se deben documentar módulos que no estén implementados.

# **Reportes Administrativos**

Los reportes son el producto final del sistema. Permiten verificar la asistencia por grupo, por persona y por periodo, y sustentan las decisiones académicas.

**Tabla 13**

*Reportes disponibles en el sistema*

| **Reporte**               | **Contenido**                                               | **Filtros**                   | **Frecuencia** |
|---------------------------|-------------------------------------------------------------|-------------------------------|----------------|
| Asistencia por ficha      | Asistencia consolidada de todos los aprendices de un grupo. | Ficha y rango de fechas.      | Semanal        |
| Asistencia por aprendiz   | Historial individual de asistencia.                         | Aprendiz y rango de fechas.   | Por solicitud  |
| Asistencia por curso      | Asistencia agrupada por curso o competencia.                | Curso y rango de fechas.      | Semanal        |
| Asistencia por instructor | Sesiones y registros asociados a un instructor.             | Instructor y rango de fechas. | Mensual        |
| Usuarios registrados      | Cuentas del sistema por estado y por rol.                   | Rol y estado.                 | Mensual        |
| Inasistencias acumuladas  | Aprendices que superan el umbral de inasistencia.           | Ficha y periodo.              | Semanal        |

## **Generar y Exportar un Reporte**

1.  Ingresar al menú Reportes.

2.  Seleccionar el tipo de reporte.

3.  Aplicar los filtros de ficha, curso, aprendiz o instructor y el rango de fechas.

4.  Hacer clic en Generar o Consultar.

5.  Revisar en pantalla que los datos correspondan al filtro aplicado.

6.  Seleccionar el formato de exportación, PDF o Excel.

7.  Descargar el archivo y guardarlo con la convención de nombres definida.

**Resultado esperado:** el sistema genera el archivo con los datos filtrados y lo descarga en el equipo.

**Figura 3**

*Convención de nombres para los archivos de reportes*

> Reporte_<tipo>_<identificador>_<AAAA-MM-DD>.pdf
>
> Ejemplos:
>
> Reporte_asistencia_ficha_<numero>_2026-10-06.pdf
>
> Reporte_usuarios_activos_2026-10-06.xlsx

## **Métricas de Seguimiento**

El administrador hace seguimiento a los indicadores de la tabla siguiente. Los valores objetivo debe fijarlos el equipo según los acuerdos del centro de formación.

**Tabla 14**

*Indicadores de seguimiento administrativo*

| **Indicador**                       | **Fuente**                                                    | **Periodicidad** | **Umbral de alerta**          |
|-------------------------------------|---------------------------------------------------------------|------------------|-------------------------------|
| Porcentaje de asistencia por ficha  | Reporte de asistencia por ficha.                              | Semanal          | Lo define el centro de formación                 |
| Aprendices con inasistencia crítica | Reporte de inasistencias acumuladas.                          | Semanal          | Lo define el centro de formación                 |
| Sesiones sin registro de asistencia | Comparación entre horarios programados y registros generados. | Semanal          | Cualquier sesión sin registro |
| Cuentas activas sin uso             | Reporte de usuarios cruzado con el último ingreso.            | Mensual          | Lo define el centro de formación                 |
| Novedades de asistencia atendidas   | Bitácora de novedades.                                        | Mensual          | Más de 48 horas sin atender   |
| Copias de seguridad generadas       | Carpeta de respaldos.                                         | Semanal          | Cualquier semana sin respaldo |

## **Buenas Prácticas en el Manejo de Reportes**

- Verificar siempre el rango de fechas antes de entregar un reporte, dado que un filtro incorrecto produce conclusiones incorrectas.

- Archivar los reportes del periodo en una carpeta controlada y no en el escritorio del equipo personal.

- Entregar los reportes únicamente a quien tiene una función que lo justifique, pues contienen datos personales de aprendices.

- No modificar manualmente el archivo exportado. Si un dato está mal, se corrige en el sistema y se genera el reporte nuevamente.

# **Copias de Seguridad**

FaceAttend EDU utiliza dos bases de datos. Una copia de seguridad completa debe incluir ambas, pues respaldar solo una deja el sistema en un estado inconsistente al momento de restaurar.

**Tabla 15**

*Elementos incluidos en la copia de seguridad*

| **Elemento**              | **Motor**               | **Contenido**                                                                 | **Criticidad** |
|---------------------------|-------------------------|-------------------------------------------------------------------------------|----------------|
| Base de datos relacional  | PostgreSQL              | Usuarios, roles, fichas, cursos, horarios y registros de asistencia.          | Alta           |
| Base de datos documental  | MongoDB                 | Información no estructurada del sistema, según el servicio que la administra. | Alta           |
| Archivos de configuración | Archivos del despliegue | Parámetros de los microservicios, sin credenciales reales.                    | Media          |
| Reportes archivados       | Sistema de archivos     | Reportes exportados de periodos cerrados.                                     | Media          |

**Tabla 16**

*Frecuencia de las copias de seguridad*

| **Momento**                                    | **Alcance**                                         | **Responsable**             |
|------------------------------------------------|-----------------------------------------------------|-----------------------------|
| Semanal                                        | Copia completa de PostgreSQL y MongoDB.             | Administrador               |
| Antes de un despliegue o actualización         | Copia completa.                                     | Super Admin o líder técnico |
| Antes de un cambio de configuración importante | Copia completa.                                     | Administrador               |
| Al cierre de un periodo académico              | Copia completa más archivo de reportes del periodo. | Administrador               |

## **Procedimiento desde el Panel**

1.  Ingresar al panel de administración con una cuenta autorizada.

2.  Seleccionar la opción Copias de seguridad.

3.  Hacer clic en Generar backup.

4.  Esperar a que el sistema confirme la generación.

5.  Descargar el archivo generado.

6.  Guardarlo en la carpeta de respaldos con la fecha en el nombre.

7.  Registrar la copia en la bitácora de respaldos.

**Figura 4**

*Convención de nombres para los archivos de respaldo*

> backup_faceattend_postgres_2026-10-06.sql
>
> backup_faceattend_mongo_2026-10-06.archive

## **Procedimiento Alterno por Línea de Comandos**

Cuando el panel no ofrece la opción o no está disponible, el respaldo lo ejecuta el equipo técnico con las herramientas propias de cada motor. Los valores de conexión se toman de las variables de entorno del despliegue.

**Figura 5**

*Comandos de respaldo de las bases de datos*

> # PostgreSQL
>
> pg_dump -h <host> -p <puerto> -U <usuario> -d <base> \
>
> -F c -f backup_faceattend_postgres_<AAAA-MM-DD>.dump
>
> # MongoDB
>
> mongodump --uri="<cadena_de_conexion>" \
>
> --archive=backup_faceattend_mongo_<AAAA-MM-DD>.archive --gzip

*Nota.* Los comandos se ejecutan con credenciales que no deben escribirse en este manual ni compartirse por canales abiertos. Deben solicitarse al responsable técnico en el momento de usarlas.

## **Almacenamiento de los Respaldos**

- Guardar cada respaldo en al menos dos ubicaciones distintas.

- Mantener una de las ubicaciones fuera del equipo donde se ejecuta el sistema.

- Conservar como mínimo los cuatro respaldos semanales más recientes y el respaldo de cierre de cada periodo.

- Restringir el acceso a la carpeta de respaldos, dado que contiene datos personales completos.

- Verificar que el archivo descargado tenga un tamaño coherente, pues un archivo de pocos kilobytes indica un respaldo fallido.

**Tabla 17**

*Bitácora de copias de seguridad*

| **Fecha** | **Tipo**                                     | **Archivos generados** | **Ubicación** | **Responsable** | **Verificado** |
|-----------|----------------------------------------------|------------------------|---------------|-----------------|----------------|
| [ ]     | [Semanal, previa a despliegue o de cierre] | [ ]                  | [ ]         | [ ]           | [Sí o No]    |
| [ ]     | [ ]                                        | [ ]                  | [ ]         | [ ]           | [ ]          |
| [ ]     | [ ]                                        | [ ]                  | [ ]         | [ ]           | [ ]          |

# **Restauración de Información**

La restauración devuelve el sistema al estado del respaldo seleccionado. Es un procedimiento que sobrescribe información, por lo que solo lo ejecuta el Super Administrador o el líder técnico, y siempre con autorización previa.

Toda la información registrada entre la fecha del respaldo y el momento de la restauración se pierde. Antes de restaurar se debe generar un respaldo del estado actual, de modo que sea posible volver atrás si el procedimiento falla.

**Tabla 18**

*Criterios para determinar la necesidad de restauración*

| **Situación**                              | **¿Requiere restauración?** | **Acción previa**                                              |
|--------------------------------------------|-----------------------------|----------------------------------------------------------------|
| Pérdida masiva de registros de asistencia  | Sí                          | Confirmar el alcance con el equipo técnico.                    |
| Un usuario eliminado por error             | No necesariamente           | Recrear el usuario y restaurar solo si se perdió su historial. |
| Error en un registro puntual de asistencia | No                          | Corregir desde el módulo correspondiente, según la sección 12. |
| Base de datos corrupta o inaccesible       | Sí                          | Escalar al líder técnico antes de cualquier acción.            |
| Despliegue fallido que alteró los datos    | Sí                          | Restaurar el respaldo previo al despliegue.                    |

## **Procedimiento de Restauración**

1.  Obtener la autorización del responsable del proyecto y dejarla por escrito.

2.  Informar a los usuarios que el sistema no estará disponible durante el procedimiento.

3.  Generar un respaldo del estado actual de ambas bases de datos.

4.  Detener los servicios que escriben en las bases de datos.

5.  Seleccionar el archivo de respaldo correspondiente a la fecha requerida.

6.  Restaurar PostgreSQL y MongoDB con el mismo punto en el tiempo.

7.  Reiniciar los servicios.

8.  Verificar el sistema según la lista de comprobación de la sección siguiente.

9.  Informar a los usuarios que el sistema fue restablecido e indicar la fecha hasta la cual la información es válida.

10. Registrar el evento en la bitácora de incidentes.

## **Verificación Posterior a la Restauración**

- El inicio de sesión funciona con una cuenta de administrador.

- El listado de usuarios muestra la cantidad esperada de registros.

- Los roles y permisos se conservan correctamente.

- Las fichas, cursos y horarios aparecen completos.

- Un reporte de asistencia de un periodo conocido arroja los datos esperados.

- Los registros más recientes corresponden a la fecha del respaldo restaurado.

## **Procedimiento Alterno por Línea de Comandos**

**Figura 6**

*Comandos de restauración de las bases de datos*

> # PostgreSQL
>
> pg_restore -h <host> -p <puerto> -U <usuario> -d <base> \
>
> --clean --if-exists backup_faceattend_postgres_<AAAA-MM-DD>.dump
>
> # MongoDB
>
> mongorestore --uri="<cadena_de_conexion>" \
>
> --archive=backup_faceattend_mongo_<AAAA-MM-DD>.archive --gzip --drop

# **Auditoría e Historial de Acciones**

El historial de acciones permite establecer quién ejecutó cada operación y en qué momento. Es la herramienta con la que el administrador sustenta cualquier cambio sobre usuarios, permisos o registros de asistencia.

**Tabla 19**

*Acciones que deben quedar registradas en el historial*

| **Acción**                            | **Dato mínimo esperado**                                      | **Quién la revisa** |
|---------------------------------------|---------------------------------------------------------------|---------------------|
| Creación de usuario                   | Usuario creado, rol asignado, responsable, fecha y hora.      | Administrador       |
| Cambio de rol                         | Rol anterior, rol nuevo, responsable, fecha y hora.           | Super Admin         |
| Activación o desactivación de cuenta  | Estado anterior, estado nuevo, responsable y fecha.           | Administrador       |
| Restablecimiento de contraseña        | Cuenta afectada, responsable y fecha.                         | Super Admin         |
| Ajuste de un registro de asistencia   | Registro afectado, valor anterior, valor nuevo y responsable. | Super Admin         |
| Cambio de configuración               | Parámetro, valor anterior, valor nuevo y responsable.         | Super Admin         |
| Generación o restauración de respaldo | Tipo de operación, responsable y fecha.                       | Super Admin         |

## **Consultar el Historial**

1.  Ingresar al menú Administración y luego a Auditoría o Historial de acciones.

2.  Aplicar el filtro por rango de fechas.

3.  Filtrar por usuario responsable o por tipo de acción cuando se investiga un caso puntual.

4.  Revisar los registros resultantes.

5.  Exportar el resultado cuando la revisión deba quedar como evidencia.

## **Revisión Periódica**

Una vez al mes el administrador revisa el historial buscando específicamente los siguientes elementos:

- Cambios de rol hacia Administrador o Super Admin que no correspondan a una solicitud registrada.

- Ajustes de asistencia sin justificación en la bitácora de novedades.

- Accesos administrativos en horarios inusuales.

- Restablecimientos de contraseña repetidos sobre la misma cuenta.

- Eliminación de registros.

Cualquier hallazgo se documenta y se escala según el procedimiento de la sección 17.

## **Acciones No Registradas por el Sistema**

Si el sistema actual no registra alguna de las acciones señaladas, se debe dejar constancia en esta sección y reportarlo al equipo técnico como una mejora pendiente. Mientras tanto, esa acción se registra manualmente en la bitácora de cambios administrativos del Apéndice C.

# **Recomendaciones de Seguridad y Procedimientos de Apoyo**

## **Seguridad de las Cuentas**

- No compartir la contraseña de administrador con ninguna persona ni por ningún canal.

- Usar cuentas individuales, sin cuentas genéricas compartidas entre varias personas.

- Asignar los permisos según la función real de cada persona y no por conveniencia.

- Desactivar de inmediato las cuentas de personas que ya no pertenecen al proceso.

- Cerrar sesión al terminar de usar el sistema, especialmente en equipos compartidos.

- No dejar la sesión de administrador abierta en un equipo sin supervisión.

- Cambiar la contraseña propia ante cualquier sospecha de que fue conocida por otra persona.

## **Seguridad de la Información**

- Realizar una copia de seguridad antes de cualquier cambio importante.

- No publicar credenciales, llaves privadas, tokens ni cadenas de conexión en documentos, repositorios o mensajes.

- Tratar los reportes como información personal y entregarlos solo a quien tiene una función que lo justifique.

- No almacenar respaldos ni reportes en servicios personales no autorizados.

- Revisar periódicamente el historial de acciones.

## **Comunicación con los Usuarios**

**Tabla 20**

*Matriz de comunicación con los usuarios*

| **Situación**                   | **Destinatario**                  | **Momento**                       | **Canal**                    |
|---------------------------------|-----------------------------------|-----------------------------------|------------------------------|
| Mantenimiento programado        | Todos los usuarios                | Con 48 horas de anticipación      | Correo institucional |
| Caída no programada del sistema | Instructores y coordinación       | En cuanto se detecta              | Correo institucional                |
| Cambio en roles o permisos      | Usuario afectado y su responsable | Antes de aplicar el cambio        | Correo institucional                |
| Restauración de información     | Todos los usuarios                | Antes y después del procedimiento | Correo institucional                |
| Nueva versión del sistema       | Todos los usuarios                | El día del despliegue             | Correo institucional                |
| Novedad de asistencia atendida  | Quien reportó la novedad          | Al cerrar el caso                 | Correo institucional                |

## **Gestión de Incidentes y Escalamiento**

Un incidente es cualquier situación que impide el uso normal del sistema o que compromete la información. El procedimiento es el siguiente.

1.  Registrar el incidente con fecha, hora, usuario que reporta, descripción y captura del error.

2.  Clasificar el nivel según la tabla siguiente.

3.  Intentar la solución desde el panel de administración cuando el nivel lo permita.

4.  Escalar al nivel correspondiente si no se resuelve dentro del tiempo definido.

5.  Informar al usuario que reportó, tanto al recibir el caso como al cerrarlo.

6.  Registrar la solución aplicada en la bitácora de incidentes del Apéndice C.

**Tabla 21**

*Niveles de escalamiento de incidentes*

| **Nivel**     | **Ejemplos**                                                                                   | **Responsable**                                  | **Tiempo de respuesta**                                 |
|---------------|------------------------------------------------------------------------------------------------|--------------------------------------------------|---------------------------------------------------------|
| 1. Operativo | Contraseña olvidada, cuenta inactiva o duda sobre un reporte.                                  | Administrador                                    | Mismo día                                               |
| 2. Funcional | Un módulo no responde, un reporte arroja datos incorrectos o se presenta un cruce de horarios. | Administrador con apoyo del equipo desarrollador | 1 semana                                           |
| 3. Técnico   | Un microservicio caído, error de conexión a PostgreSQL o MongoDB, o sistema inaccesible.       | Líder técnico                                    | Inmediato                                               |
| 4. Crítico   | Pérdida de información, acceso no autorizado o corrupción de base de datos.                    | Super Admin y líder técnico                      | Inmediato, con notificación al responsable del proyecto |

## **Gestión de Cambios**

Un cambio es cualquier modificación planeada sobre el sistema, como una nueva versión, un cambio de configuración, la modificación de permisos o el ajuste de parámetros. El procedimiento es el siguiente.

1.  Solicitud: quien requiere el cambio lo plantea por escrito, indicando el motivo.

2.  Evaluación: el administrador y el líder técnico determinan el impacto y los usuarios afectados.

3.  Autorización: el responsable del proyecto aprueba o rechaza el cambio.

4.  Respaldo: se genera una copia de seguridad antes de aplicar el cambio.

5.  Aplicación: se ejecuta el cambio en la ventana acordada.

6.  Verificación: se comprueba que el sistema funciona y que el cambio produjo el efecto esperado.

7.  Comunicación: se informa a los usuarios afectados.

8.  Documentación: se actualiza este manual y su control de versiones cuando el cambio modifica un procedimiento.

Si un cambio aplicado produce un comportamiento no esperado, se revierte con el respaldo previo antes de intentar correcciones sucesivas, dado que acumular correcciones sobre un sistema inestable dificulta identificar la causa.

## **Riesgos Conocidos y Plan de Contingencia**

**Tabla 22**

*Riesgos conocidos y plan de contingencia*

| **Riesgo**                                    | **Impacto**                                                   | **Prevención**                                                                   | **Contingencia**                                                                          |
|-----------------------------------------------|---------------------------------------------------------------|----------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| Caída de uno de los microservicios            | Un módulo deja de funcionar mientras el resto opera.          | Monitoreo de los servicios y revisión diaria.                                    | Escalar a nivel 3 y registrar la asistencia en formato físico mientras se restablece.     |
| Pérdida de conexión con PostgreSQL            | El sistema no permite consultar ni registrar información.     | Respaldo semanal y verificación de disponibilidad.                               | Escalar a nivel 3 y restaurar desde el último respaldo si hay corrupción.                 |
| Pérdida de conexión con MongoDB               | Falla la información administrada por el servicio documental. | Respaldo semanal del mismo punto en el tiempo que PostgreSQL.                    | Escalar a nivel 3 y restaurar ambas bases al mismo punto.                                 |
| Respaldo no verificado que falla al restaurar | Pérdida definitiva de información.                            | Verificar el tamaño del archivo y probar una restauración de prueba por periodo. | Usar el respaldo inmediatamente anterior y documentar el alcance de la pérdida.           |
| Cuenta administrativa comprometida            | Acceso no autorizado a datos personales.                      | Cuentas individuales, cierre de sesión y revisión del historial.                 | Desactivar la cuenta, restablecer credenciales, revisar el historial y escalar a nivel 4. |
| Pérdida del único administrador activo        | Nadie puede gestionar el sistema.                             | Mantener al menos dos cuentas administrativas activas.                           | El Super Admin crea una nueva cuenta administrativa.                                      |
| Desfase entre el manual y el sistema          | Procedimientos que no corresponden a la realidad.             | Revisión del manual después de cada despliegue.                                  | Actualizar el manual y registrar la nueva versión en la sección 2.                        |

# **Glosario**

**Tabla 23**

*Glosario de términos del sistema*

| **Término**           | **Definición**                                                                                          |
|-----------------------|---------------------------------------------------------------------------------------------------------|
| Administrador         | Rol con permisos para gestionar usuarios, fichas, cursos, horarios y reportes del sistema.              |
| Aprendiz              | Persona en proceso de formación cuya asistencia registra el sistema.                                    |
| Auditoría             | Registro de las acciones ejecutadas en el sistema, con responsable, fecha y detalle del cambio.         |
| Copia de seguridad    | Archivo que contiene una copia de la información del sistema en un momento determinado.                 |
| Curso                 | Unidad de formación asociada a una ficha, sobre la cual se programan horarios.                          |
| Escalamiento          | Procedimiento de traslado de un incidente a un nivel de atención superior.                              |
| Estado de la cuenta   | Condición que determina si un usuario puede iniciar sesión: Activo o Inactivo.                          |
| Exportación           | Descarga de la información de un reporte en formato PDF o Excel.                                        |
| Ficha                 | Grupo de formación identificado por un número único, al cual pertenecen los aprendices.                 |
| Horario               | Franja de días y horas en la que se desarrolla un curso y sobre la cual se registra la asistencia.      |
| Incidente             | Situación que impide el uso normal del sistema o compromete la información.                             |
| Instructor            | Rol que gestiona la asistencia de las fichas y cursos que tiene asignados.                              |
| Microservicio         | Componente independiente del sistema que presta una función específica y se comunica con los demás.     |
| MongoDB               | Motor de base de datos documental utilizado por el sistema.                                             |
| Novedad de asistencia | Solicitud de corrección sobre un registro de asistencia.                                                |
| Permiso               | Autorización para ejecutar una acción determinada dentro del sistema.                                   |
| PostgreSQL            | Motor de base de datos relacional utilizado por el sistema.                                             |
| Restauración          | Procedimiento que devuelve el sistema al estado contenido en una copia de seguridad.                    |
| Rol                   | Conjunto de permisos que define qué puede hacer un usuario dentro del sistema.                          |
| Super Admin           | Rol con control total del sistema, incluidas la configuración general y la restauración de información. |
| Variable de entorno   | Parámetro de configuración externo al código con el que se ejecutan los servicios.                      |

# **Referencias**

Servicio Nacional de Aprendizaje. (2026). *Guía para elaborar manuales de software: Manual de usuario, manual técnico, manual de instalación, manual de administrador* [Material de apoyo para aprendices]. Centro de la Industria, la Empresa y los Servicios.

El equipo debe completar los datos de autoría institucional de la guía antes de la entrega final. Si durante el desarrollo del manual se consultan fuentes adicionales, estas deben citarse en el texto y relacionarse en esta sección en orden alfabético, con sangría francesa.

# **Apéndice A**

**Lista de Verificación de Actividades del Administrador**

**Tabla 24**

*Lista de verificación de actividades del administrador*

| **Actividad**                                | **Periodicidad** | **Cumple**  | **Fecha** | **Observación** |
|----------------------------------------------|------------------|-------------|-----------|-----------------|
| El sistema responde y permite iniciar sesión | Diaria           | [Sí o No] | [ ]     | [ ]           |
| Solicitudes de acceso atendidas              | Diaria           | [Sí o No] | [ ]     | [ ]           |
| Novedades de asistencia atendidas            | Diaria           | [Sí o No] | [ ]     | [ ]           |
| Usuarios activos revisados                   | Semanal          | [Sí o No] | [ ]     | [ ]           |
| Reporte de asistencia generado y archivado   | Semanal          | [Sí o No] | [ ]     | [ ]           |
| Copia de seguridad generada y verificada     | Semanal          | [Sí o No] | [ ]     | [ ]           |
| Historial de acciones revisado               | Mensual          | [Sí o No] | [ ]     | [ ]           |
| Configuración del sistema validada           | Mensual          | [Sí o No] | [ ]     | [ ]           |
| Manual contrastado con el sistema real       | Mensual          | [Sí o No] | [ ]     | [ ]           |

# **Apéndice B**

**Directorio de Contactos**

El equipo debe diligenciar este directorio antes de entregar el manual. Sin él, el procedimiento de escalamiento de la sección 17 no es aplicable.

**Tabla 25**

*Directorio de contactos del proyecto*

| **Función**                           | **Nombre**    | **Correo**    | **Motivo de contacto**                                                     |
|---------------------------------------|---------------|---------------|----------------------------------------------------------------------------|
| Super Administrador                   | [COMPLETAR] | [COMPLETAR] | Restauraciones, cambios de configuración crítica e incidentes de nivel 4.  |
| Administrador principal               | [COMPLETAR] | [COMPLETAR] | Gestión diaria de usuarios, fichas, horarios y reportes.                   |
| Administrador suplente                | [COMPLETAR] | [COMPLETAR] | Reemplazo del administrador principal.                                     |
| Líder técnico                         | [COMPLETAR] | [COMPLETAR] | Incidentes de nivel 3, despliegues y fallas de servicios o bases de datos. |
| Equipo desarrollador                  | [COMPLETAR] | [COMPLETAR] | Errores funcionales y solicitudes de mejora.                               |
| Instructor o instructora del proyecto | [COMPLETAR] | [COMPLETAR] | Decisiones académicas y aprobación de cambios.                             |
| Responsable del proyecto              | [COMPLETAR] | [COMPLETAR] | Autorización de cambios y de restauraciones.                               |

# **Apéndice C**

**Bitácora de Cambios Administrativos e Incidentes**

**Tabla 26**

*Bitácora de cambios administrativos e incidentes*

| **Fecha** | **Tipo**                        | **Descripción** | **Responsable** | **Resultado** |
|-----------|---------------------------------|-----------------|-----------------|---------------|
| [ ]     | [Cambio, incidente o novedad] | [ ]           | [ ]           | [ ]         |
| [ ]     | [ ]                           | [ ]           | [ ]           | [ ]         |
| [ ]     | [ ]                           | [ ]           | [ ]           | [ ]         |
| [ ]     | [ ]                           | [ ]           | [ ]           | [ ]         |

# **Apéndice D**

**Cronograma de Actividades Administrativas**

**Tabla 27**

*Cronograma de actividades administrativas*

| **Actividad**                              | **Frecuencia** | **Momento sugerido**           | **Responsable**      |
|--------------------------------------------|----------------|--------------------------------|----------------------|
| Verificación de disponibilidad del sistema | Diaria         | Inicio de la jornada           | Administrador        |
| Atención de solicitudes de acceso          | Diaria         | Durante la jornada             | Administrador        |
| Generación del reporte de asistencia       | Semanal        | Viernes                        | Administrador        |
| Copia de seguridad                         | Semanal        | Viernes, al cierre             | Administrador        |
| Depuración de usuarios inactivos           | Semanal        | Viernes                        | Administrador        |
| Revisión del historial de acciones         | Mensual        | Último día hábil del mes       | Administrador        |
| Validación de la configuración             | Mensual        | Último día hábil del mes       | Super Admin          |
| Prueba de restauración                     | Por periodo    | Antes del cierre del periodo   | Líder técnico        |
| Actualización del manual                   | Por despliegue | Después de cada versión        | Equipo desarrollador |
| Cierre de periodo académico                | Por periodo    | Lo define el centro de formación | Administrador        |

# **Apéndice E**

**Capturas de Pantalla del Sistema**

Este apéndice debe contener las capturas reales del sistema, con buena resolución y ordenadas según el flujo de administración: inicio de sesión, panel de administración, listado de usuarios, formulario de nuevo usuario, gestión de roles, fichas, cursos, horarios, pantalla de reportes y pantalla de copias de seguridad.

Cada captura debe insertarse con el formato de figura establecido en las normas APA: el rótulo Figura seguido del número correspondiente en negrita, el título en cursiva en la línea siguiente, la imagen, y una nota explicativa cuando sea necesaria. Las capturas no deben mostrar datos personales reales ni credenciales.

# **Apéndice F**

**Elementos Pendientes de Completar**

Antes de la entrega final, el equipo debe reemplazar todos los campos marcados como [COMPLETAR] y verificar los siguientes puntos:

- Datos de la portada: integrantes, centro de formación, ficha e instructor o instructora.

- Dirección URL del panel de administración.

- Nombres reales de los menús y botones, si difieren de los empleados en este manual.

- Valores de los parámetros de configuración y umbrales de las métricas.

- Canales oficiales de comunicación y tiempos de respuesta.

- Directorio de contactos del Apéndice B.

- Capturas de pantalla reales en el Apéndice E.

- Datos de autoría de la guía institucional en la lista de referencias.

- Documentación de los módulos adicionales implementados, según la sección 12.
