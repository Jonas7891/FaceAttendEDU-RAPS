# Manual de Usuario

**FaceAttend EDU: Sistema de Registro de Asistencia Mediante Reconocimiento Facial**

Jonattan Steven Rizo Solano  
Diego Andrés Gutiérrez Núñez  
Juan David Arboleda Perdomo

# Módulos

A continuación se documenta cada módulo funcional de FaceAttend EDU, siguiendo un formato consistente que incluye nombre, descripción, rol de acceso, ruta de navegación, pasos de uso, captura de pantalla, resultado esperado y observaciones o restricciones.

## Módulo 1: Gestión de usuarios (RF1)

Permite buscar usuarios existentes, vincularlos y editar sus datos básicos desde la aplicación móvil.

### Descripción

Módulo de consulta y vinculación de usuarios. La aplicación móvil no registra usuarios nuevos ni carga archivos CSV: únicamente busca usuarios que ya existen en el sistema y permite vincularlos. El formulario de edición permite modificar los datos personales del usuario, pero no incluye campos para asignar el rol ni para activar o desactivar la cuenta.

### Rol que Puede Usarlo

Administrador.

### Ruta o Menú Donde se Encuentra

Inicio de sesión > Menú principal > Usuarios

### Pasos para Utilizarlo

1.  Ingresar al módulo "Usuarios" desde el menú principal.

2.  Usar el buscador para localizar al usuario existente.

3.  Seleccionar al usuario en el listado para vincularlo.

4.  Si se requiere corregir sus datos, abrir el formulario de edición y modificar los campos disponibles.

5.  Guardar los cambios.

### Captura de Pantalla

<img src="media/image8.png" style="width:1.45833in;height:3in" /> <img src="media/image1.png" style="width:1.44792in;height:3in" />

### Resultado Esperado

El usuario existente queda localizado y vinculado, y sus datos personales quedan actualizados cuando se edita el formulario.

### Observaciones o Restricciones

El registro de usuarios nuevos, la carga masiva por CSV, la asignación de roles y la activación o desactivación de cuentas no se realizan desde la aplicación móvil.

## Módulo 2: Gestión de ambientes (RF2)

Permite consultar y registrar los ambientes o salones donde se realiza el control de asistencia.

### Descripción

Módulo para gestionar los ambientes/salones. La pantalla solo maneja la información del ambiente; la gestión de fichas y cursos no se realiza en la aplicación móvil, sino en la plataforma web. Además de los datos básicos del ambiente, el formulario incluye el campo "Tipo", que indica la clase de ambiente que se está registrando.

### Rol que Puede Usarlo

Administrador.

### Ruta o Menú Donde se Encuentra

Inicio de sesión > Menú principal > Ambientes

### Pasos para Utilizarlo

1.  Ingresar al módulo "Ambientes" desde el menú principal.

2.  Seleccionar la opción para registrar un ambiente.

3.  Ingresar los datos del ambiente: nombre, ubicación y capacidad.

4.  Guardar los cambios.

> **Nota.** La pantalla muestra un campo "Tipo", pero la tabla `scheduling.environment` solo almacena `code`, `name`, `capacity` y `status`: no tiene una columna para el tipo de ambiente. Por lo tanto, el valor elegido en esta pantalla no se conserva en la versión 1.0.0. O bien se agrega la columna al modelo o bien se retira el campo de la pantalla; mientras tanto, el campo debe ignorarse.

### Captura de Pantalla

<img src="media/image13.png" style="width:1.44792in;height:3in" /> <img src="media/image9.png" style="width:1.45833in;height:3in" />

### Resultado Esperado

El ambiente queda registrado, con su tipo definido, y disponible para el control de asistencia.

### Observaciones o Restricciones

La gestión de fichas o cursos y la asignación de responsables y jornadas no están disponibles en esta pantalla; se administran desde la plataforma web.

## Módulo 3: Registro de entradas y salidas (RF3)

Permite capturar y reconocer el rostro de los usuarios mediante la cámara, y actualizar la foto y los parámetros faciales.

### Descripción

Módulo principal de la aplicación móvil. Cuenta con dos pantallas de cámara con propósitos distintos: la pantalla de registro facial (RegisterFace), que activa la cámara para escanear el rostro y compararlo con los datos faciales registrados, y la pantalla de actualización de foto (UpdatePhotoScreen), que permite cambiar la fotografía del usuario. La actualización de los parámetros faciales es una función exclusiva del rol Estudiante.

### Rol que Puede Usarlo

Estudiante y Docente/Instructor (registro facial). Estudiante (actualización de parámetros faciales).

### Ruta o Menú Donde se Encuentra

Aplicación móvil > Escaneo facial

### Pasos para Utilizarlo

**Flujo A: Registro facial (RegisterFace)**

1.  El usuario abre el módulo de escaneo facial en la app móvil.

2.  El sistema activa la cámara para detectar el rostro.

3.  El usuario se posiciona frente a la cámara con buena iluminación.

4.  El sistema compara la imagen con los datos faciales registrados previamente.

5.  Si el rostro coincide, se registra la asistencia como "presente".

6.  Si el rostro no coincide o falla el escaneo, se habilita el registro alterno.

**Flujo B: Actualización de foto (UpdatePhotoScreen)**

1.  Ingresar a la opción de actualización de foto.

2.  Completar los campos obligatorios: nombre, documento y teléfono. Sin estos datos no se habilita la cámara.

3.  Abrir la cámara y capturar la nueva fotografía.

4.  Confirmar y guardar la foto actualizada.

**Flujo C: Actualización de parámetros faciales (solo Estudiante)**

El estudiante puede actualizar sus parámetros faciales desde su perfil de usuario. Esta opción no está disponible para los demás roles.

### Captura de Pantalla

<img src="media/image13.png" style="width:1.44792in;height:3in" /><img src="media/image12.png" style="width:1.38445in;height:3.01063in" />

### Resultado Esperado

Se almacena la información de asistencia o inasistencia del usuario, o bien la foto y los parámetros faciales quedan actualizados, según el flujo utilizado.

### Observaciones o Restricciones

El escaneo puede fallar por mala iluminación o movimiento del usuario. Requiere que el usuario ya esté registrado en el sistema con su rostro y credenciales, y que la base de datos esté activa. La cámara de actualización de foto solo se abre cuando nombre, documento y teléfono están diligenciados.

## Módulo 4: Configuración del sistema (RF4)

Permite ajustar las alertas, el idioma y la apariencia de la aplicación, así como los parámetros de usuario.

### Descripción

Módulo donde el usuario ajusta sus preferencias. La apariencia y el idioma son pantallas independientes. La pantalla de apariencia ofrece controles deslizantes HSL (tono, saturación y luminosidad) para personalizar los colores, modos de visión para personas con daltonismo y un evaluador de accesibilidad WCAG que verifica el contraste de la paleta elegida. La parametrización de retardos no se realiza en este módulo; se describe en el Módulo 8.

### Rol que Puede Usarlo

Usuario (preferencias personales, app móvil); Administrador (parámetros generales del sistema).

### Ruta o Menú Donde se Encuentra

Aplicación móvil > Configuración / Menú principal > Configuración

### Pasos para Utilizarlo

1.  Ingresar al módulo "Configuración".

2.  Para alertas: activar o desactivar notificaciones y elegir el tono.

3.  Para idioma: abrir la pantalla "Idioma" y seleccionar el idioma preferido del aplicativo.

4.  Para la apariencia: abrir la pantalla "Apariencia" y ajustar sus preferencias de colorimetría a su gusto

5.  Seleccionar, si se requiere, un modo de visión para daltonismo.

6.  Para parámetros de usuario: actualizar los datos personales.

7.  El administrador puede además gestionar las alertas de dispositivos IoT y cambiar el rol de un usuario.

### Captura de Pantalla

<img src="media/image13.png" style="width:1.44792in;height:3in" /> <img src="media/image14.png" style="width:1.45833in;height:3in" />

<img src="media/image4.png" style="width:1.9375in;height:3in" /> <img src="media/image7.png" style="width:2.07292in;height:3in" />

<img src="media/image18.png" style="width:1.4375in;height:3in" />

### Resultado Esperado

Los cambios de configuración quedan guardados y se aplican de inmediato en la experiencia del usuario.

### Observaciones o Restricciones

Los cambios de rol solo están disponibles para el rol Administrador. La parametrización de retardos se encuentra en el módulo de Gestión del colegio (RF8).

## Módulo 5: Generar históricos (RF5)

Permite consultar el historial de asistencias e inasistencias filtrando por nombre y fecha.

### Descripción

Módulo de consulta que permite revisar la asistencia e inasistencia de los estudiantes. Los filtros disponibles en la aplicación móvil son únicamente el nombre de la persona y la fecha; no se puede filtrar por ficha, ambiente ni instructor.

### Rol que Puede Usarlo

Docente/Instructor y Administrador.

### Ruta o Menú Donde se Encuentra

Menú principal > Históricos

### Pasos para Utilizarlo

1.  Ingresar al módulo "Históricos".

2.  Escribir el nombre de la persona que se desea consultar.

3.  Seleccionar la fecha o el rango de fechas de interés.

4.  Revisar en pantalla los resultados que coinciden con los filtros.

### Captura de Pantalla

<img src="media/image16.png" style="width:1.45833in;height:3in" /><img src="media/image10.png" style="width:1.38125in;height:2.99479in" />

### Resultado Esperado

El sistema muestra el listado de asistencias e inasistencias que coinciden con el nombre y la fecha ingresados.

### Observaciones o Restricciones

La aplicación móvil no incluye historial de cambios de usuario. La consulta por ficha, ambiente o instructor no está disponible en esta pantalla.

## Módulo 6: Gestión de justificaciones (RF6)

Permite a los usuarios registrar justificaciones de inasistencia y a los responsables aprobarlas o rechazarlas.

### Descripción

Módulo que permite registrar una justificación de inasistencia. El proceso no inicia con un aviso del sistema: es el usuario quien ingresa manualmente el tipo de justificación, la fecha y los datos solicitados. Antes del formulario existe un menú intermedio de justificaciones desde el cual se elige la acción a realizar. Posteriormente un responsable la aprueba o rechaza, y el resultado se notifica automáticamente.

### Rol que Puede Usarlo

Estudiante (registro de justificaciones); Docente/Instructor o Administrador (aprobación o rechazo).

### Ruta o Menú Donde se Encuentra

Menú principal > Justificaciones > Menú de justificaciones

### Pasos para Utilizarlo

1.  Ingresar al módulo "Justificaciones" desde el menú principal.

2.  En el menú intermedio de justificaciones, seleccionar la opción para registrar una nueva justificación.

3.  Seleccionar el tipo de justificación.

4.  Ingresar la fecha de la inasistencia.

5.  Completar manualmente los demás datos solicitados y cargar el soporte (documento o imagen), si aplica o si es necesario hacerlo.

6.  Si no se carga ningún soporte, la inasistencia queda marcada como no justificada en caso de que dicha justificación.

7.  El responsable revisa la justificación y la aprueba o rechaza.

8.  El sistema notifica automáticamente el resultado al usuario.

### Captura de Pantalla

<img src="media/image13.png" style="width:1.44792in;height:3in" /> <img src="media/image14.png" style="width:1.45833in;height:3in" /> <img src="media/image19.png" style="width:1.47917in;height:3in" />

### Resultado Esperado

La inasistencia queda registrada como justificada o no justificada, según la decisión tomada, y el usuario recibe la notificación correspondiente.

### Observaciones o Restricciones

El sistema toma como válidas únicamente las justificaciones aprobadas por el responsable asignado.

## Módulo 7: Reportes y alertas de asistencia (RF7)

Permite consultar las alertas por exceso de inasistencias o retardos y notificar a los tutores.

### Descripción

En la aplicación móvil esta pantalla funciona como un sistema de alertas: identifica a los estudiantes que superan el límite de inasistencias o retardos y permite notificar a sus tutores. No incluye exportación de reportes ni un dashboard analítico independiente.

### Rol que Puede Usarlo

Docente/Instructor, Administrador.

### Ruta o Menú Donde se Encuentra

Menú principal > Reportes

### Pasos para Utilizarlo

1.  Ingresar al módulo "Reportes".

2.  Revisar las alertas generadas por exceso de inasistencias o retardos.

3.  Seleccionar el estudiante o la alerta que se desea atender.

4.  Enviar la notificación al tutor correspondiente.

### Captura de Pantalla

<img src="media/image13.png" style="width:1.44792in;height:3in" /> <img src="media/image17.png" style="width:1.44792in;height:3in" />

### Resultado Esperado

La alerta queda atendida y el tutor del estudiante recibe la notificación sobre las inasistencias o retardos acumulados.

### Observaciones o Restricciones

La exportación de reportes y el dashboard analítico no están disponibles en la aplicación móvil. Las alertas dependen de los límites de inasistencias y retardos configurados en el sistema.

## Módulo 8: Gestión del colegio (RF8)

Permite consultar y editar la información general, de contacto, académica y de asistencia del colegio.

### Descripción

Módulo administrativo organizado en cuatro pestañas: General, Contacto, Académica y Asistencia. En la pestaña Asistencia se encuentra la parametrización de retardos. La aplicación móvil no gestiona cursos ni el plan de estudios.

### Rol que Puede Usarlo

Administrador.

### Ruta o Menú Donde se Encuentra

Menú principal > Configuración > Configuración de Colegio

### Pasos para Utilizarlo

1.  Ingresar al módulo "Configuración de Colegio".

2.  En la pestaña "General", revisar o actualizar los datos generales del colegio.

3.  En la pestaña "Contacto", revisar o actualizar los datos de contacto.

4.  En la pestaña "Académica", revisar o actualizar la información académica.

5.  En la pestaña "Asistencia", revisar o actualizar los parámetros de asistencia, incluida la parametrización de retardos.

6.  Guardar los cambios realizados.

### Captura de Pantalla

<img src="media/image13.png" style="width:1.44792in;height:3in" /> <img src="media/image14.png" style="width:1.45833in;height:3in" /> <img src="media/image20.png" style="width:1.44792in;height:3in" />

### Resultado Esperado

La información del colegio y los parámetros de asistencia quedan actualizados.

### Observaciones o Restricciones

Solo el administrador puede modificar esta información. La gestión de cursos y del plan de estudios no está disponible en la aplicación móvil.

## Módulo 9: Recuperación de contraseña (Transversal)

Permite restablecer el acceso a la cuenta cuando el usuario olvida su contraseña.

### Descripción

Pantalla de acceso público disponible antes de iniciar sesión, que guía al usuario en el proceso de recuperación de su contraseña.

### Rol que Puede Usarlo

Todos los roles.

### Ruta o Menú Donde se Encuentra

Inicio de sesión > Recuperar contraseña

### Pasos para Utilizarlo

1.  En la pantalla de inicio de sesión, seleccionar la opción de recuperación de contraseña.

2.  Ingresar los datos solicitados para identificar la cuenta.

3.  Seguir las instrucciones del sistema para definir una nueva contraseña.

4.  Iniciar sesión con la nueva contraseña.

### Captura de Pantalla

<img src="media/image15.png" style="width:1.82292in;height:3.88069in" /><img src="media/image11.png" style="width:1.79688in;height:3.87747in" />

### Resultado Esperado

El usuario restablece su contraseña y puede volver a ingresar al sistema.

### Observaciones o Restricciones

Luego de hacer los pasos necesarios para el cambio de contraseña debe volver a ingresar desde 0 en el aplicativo, no inicia directamente a la dashboard por seguridad del usuario.

## Módulo 10: Perfil (Transversal)

Permite al usuario consultar la información de su cuenta.

### Descripción

Pantalla donde el usuario visualiza los datos de su perfil dentro de la aplicación móvil.

### Rol que Puede Usarlo

Todos los roles.

### Ruta o Menú Donde se Encuentra

Barra de navegación inferior > Perfil

### Pasos para Utilizarlo

1.  Seleccionar "Perfil" en la barra de navegación inferior.

2.  Revisar la información personal mostrada.

3.  Usar las opciones disponibles en la pantalla para actualizar los datos permitidos.

### Captura de Pantalla<img src="media/image6.png" style="width:1.44792in;height:0.40491in" />

<img src="media/image5.png" style="width:1.9324in;height:5.00735in" /><img src="media/image2.png" style="width:2.36458in;height:4.72917in" /><img src="media/image3.png" style="width:2.31436in;height:4.68229in" />

### Resultado Esperado

El usuario consulta y, cuando corresponde, actualiza la información de su perfil.

### Observaciones o Restricciones

Editar esta información sólo si es estrictamente requerida o si un instructor/administrador lo pide para no tener problemas luego de cambiar esta información.

## Módulo 11: Noticias (Transversal)

Permite consultar los comunicados y novedades de la institución.

### Descripción

Pantalla informativa donde el usuario revisa las noticias publicadas dentro de la aplicación.

### Rol que Puede Usarlo

Todos los roles.

### Ruta o Menú Donde se Encuentra

Pantalla principal del aplicativo

### Pasos para Utilizarlo

1.  Seleccionar "Noticias" en la barra de navegación inferior.

2.  Desplazarse por el listado de noticias.

3.  Abrir la noticia de interés para leer su contenido.

### Captura de Pantalla

<img src="media/image21.png" style="width:1.84375in;height:4.09805in" />

### Resultado Esperado

El usuario consulta las noticias vigentes de la institución.

### Observaciones o Restricciones

Esta vista es únicamente visible para ver qué cambios o información se agrega/cambia/eliminar del colegio

## Módulo 12: Barra de navegación inferior (Transversal)

Permite desplazarse entre las secciones principales de la aplicación móvil.

### Descripción

Barra fija en la parte inferior de la pantalla que da acceso rápido a las secciones principales de la aplicación, entre ellas Perfil y Noticias.

### Rol que Puede Usarlo

Todos los roles (las opciones visibles pueden variar según el rol).

### Ruta o Menú Donde se Encuentra

Aplicación móvil > Barra inferior

### Pasos para Utilizarlo

1.  Iniciar sesión en la aplicación.

2.  Tocar el ícono de la sección deseada en la barra inferior.

3.  Para volver, tocar otra opción de la barra.

### Captura de Pantalla<img src="media/image6.png" style="width:1.44792in;height:0.40491in" />

### Resultado Esperado

El usuario cambia de sección de forma rápida desde cualquier pantalla principal.

### Observaciones o Restricciones

Esto con la finalidad de mejor experiencia y un uso más rápido y fácil para los usuarios del aplicativo movil
