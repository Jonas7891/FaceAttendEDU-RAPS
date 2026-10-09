# Manual de Instalación

**FaceAttend EDU**

Integrantes:

Juan David Arboleda Perdomo

Diego Andrés Gutiérrez Nuñez

Jonathan Steven Rizo Solano

Centro de la Industria, la Empresa y los Servicios - SENA

Tecnólogo en Análisis y Desarrollo de Software — Ficha 3145556

Karol Daniela Correa

6 de octubre de 2026

Versión del documento 1.1 — Versión del software documentada: 1.0.0

# Control de Versiones

Toda modificación a este manual debe quedar registrada antes de volver a entregar el documento. El registro permite determinar si el manual corresponde a la versión del software que se está instalando.

**Tabla 1**

*Historial de versiones del documento*

| **Versión** | **Fecha**  | **Descripción del cambio**                                                                                                                                                                                                                    | **Responsable**      |
|-------------|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|
| 1.0         | 06/10/2026 | Creación inicial del manual de instalación. Documenta el despliegue del conjunto completo de 18 contenedores, la configuración mediante el archivo de entorno único, la cadena de migraciones versionadas y el procedimiento de verificación. | Equipo desarrollador |
| 1.1         | 06/10/2026 | Aplicación de las normas APA en su séptima edición y alineación con la estructura común de los manuales del proyecto.                                                                                                                         | Equipo desarrollador |

*Nota.* Los cambios menores, como ajustes de redacción o incorporación de capturas, incrementan el segundo dígito; los cambios que alteran el procedimiento de instalación incrementan el primero.

## Regla de Actualización

El manual se revisa obligatoriamente después de cada despliegue que modifique los servicios, los puertos, las variables de entorno o las versiones de las herramientas requeridas.

> • Toda incorporación de un servicio nuevo al conjunto de contenedores obliga a actualizar la descripción del sistema, el mapa de puertos y el procedimiento de verificación.
>
> • Toda variable de entorno nueva debe quedar documentada en el apartado de configuración inicial antes de la entrega.
>
> • Cada versión entregada debe quedar registrada en la tabla anterior con su responsable.

# Tabla de Contenido

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

[**Perfil del Instalador**](#_heading=) **10**

> [Conocimientos Requeridos](#_heading=) 10
>
> [Responsabilidades del Instalador](#_heading=) 10
>
> [Límites del Rol](#_heading=) 11

[**Descripción General del Sistema**](#_heading=) **12**

> [Naturaleza de la Instalación](#_heading=) 12
>
> [Componentes del Sistema](#_heading=) 13
>
> [Organización en Redes](#_heading=) 14

[**Requisitos Previos**](#_heading=) **15**

> [Requisitos de Hardware](#_heading=) 15
>
> [Sistema Operativo](#_heading=) 16
>
> [Software Requerido](#_heading=) 16
>
> [Herramientas Opcionales](#_heading=) 17
>
> [Disponibilidad de Puertos](#_heading=) 17
>
> [Comprobación de la Disponibilidad de un Puerto](#_heading=) 18
>
> [Conectividad de Red](#_heading=) 18

[**Descarga e Instalación**](#_heading=) **19**

> [Instalación de Docker](#_heading=) 19
>
> [Procedimiento en Windows](#_heading=) 19
>
> [Procedimiento en Ubuntu y Debian](#_heading=) 19
>
> [Procedimiento en macOS](#_heading=) 19
>
> [Verificación de la Instalación de Docker](#_heading=) 20
>
> [Instalación de Git](#_heading=) 20
>
> [Obtención del Proyecto](#_heading=) 20
>
> [Primera Vía: Clonación del Repositorio](#_heading=) 20
>
> [Segunda Vía: Archivo Comprimido](#_heading=) 21
>
> [Verificación de la Estructura del Proyecto](#_heading=) 21

[**Configuración Inicial**](#_heading=) **23**

> [Creación del Archivo de Configuración](#_heading=) 23
>
> [Variables de Entorno](#_heading=) 23
>
> [Casos de Personalización](#_heading=) 25
>
> [Primer Caso: El Puerto de la Base de Datos Está Ocupado](#_heading=) 25
>
> [Segundo Caso: Prueba desde un Dispositivo Móvil](#_heading=) 26
>
> [Tercer Caso: El Puerto de la Puerta de Enlace Está Ocupado](#_heading=) 26
>
> [Canal de Correo Electrónico](#_heading=) 26

[**Puesta en Marcha**](#_heading=) **28**

> [Despliegue del Sistema](#_heading=) 28
>
> [Orden de Arranque](#_heading=) 29
>
> [Base de Datos y Migraciones](#_heading=) 29
>
> [Creación de los Esquemas](#_heading=) 29
>
> [Creación de Tablas y Datos de Catálogo](#_heading=) 30
>
> [Datos Iniciales del Sistema](#_heading=) 30
>
> [Carga de Datos de Prueba](#_heading=) 31

[**Verificación de la Instalación**](#_heading=) **33**

> [Lista de Comprobación](#_heading=) 33
>
> [Primera Prueba: Estado de los Contenedores](#_heading=) 33
>
> [Segunda Prueba: Salud de los Servicios](#_heading=) 34
>
> [Tercera Prueba: Autenticación](#_heading=) 34
>
> [Cuarta Prueba: Interfaz Web](#_heading=) 35
>
> [Quinta Prueba: Aislamiento de Red](#_heading=) 36

[**Instalación de la Aplicación Móvil**](#_heading=) **37**

> [Requisitos Adicionales](#_heading=) 37
>
> [Procedimiento de Instalación](#_heading=) 37

[**Operación Cotidiana**](#_heading=) **39**

> [Comandos de Uso Frecuente](#_heading=) 39
>
> [Detención y Reinicio del Sistema](#_heading=) 39
>
> [Actualización a una Versión Nueva](#_heading=) 39
>
> [Desinstalación](#_heading=) 40

[**Solución de Problemas**](#_heading=) **41**

> [Diagnóstico Inicial](#_heading=) 41
>
> [Problemas Durante la Instalación](#_heading=) 41
>
> [Problemas al Desplegar el Sistema](#_heading=) 41
>
> [La Dirección del Puerto ya Está Asignada](#_heading=) 41
>
> [Un Servicio Declara una Dependencia No Definida](#_heading=) 42
>
> [Un Contenedor Permanece en Reinicio Indefinido](#_heading=) 42
>
> [La Interfaz Web Figura en Estado de Salud Deficiente](#_heading=) 42
>
> [Problemas con las Migraciones](#_heading=) 43
>
> [Una Migración Concluye con Error](#_heading=) 43
>
> [El Sistema Informa que una Tabla o un Esquema No Existe](#_heading=) 43
>
> [Problemas de Conexión y Uso](#_heading=) 43
>
> [Problemas de Rendimiento](#_heading=) 44
>
> [Procedimiento de Reinicio Limpio](#_heading=) 44
>
> [Escalamiento de Incidencias](#_heading=) 45

[**Instalación sin Contenedores**](#_heading=) **46**

> [Herramientas Requeridas por Lenguaje de Programación](#_heading=) 46
>
> [Enfoque Recomendado: Modo Híbrido](#_heading=) 46
>
> [Interfaz Web en Modo de Desarrollo](#_heading=) 47

[**Recomendaciones de Seguridad**](#_heading=) **48**

> [Credenciales](#_heading=) 48
>
> [Exposición en Red](#_heading=) 48
>
> [Conservación de la Información](#_heading=) 49
>
> [Entrega al Administrador del Sistema](#_heading=) 49

[**Glosario**](#_heading=) **50**

[**Referencias**](#_heading=) **52**

[**Apéndice A**](#_heading=) **54**

[**Apéndice B**](#_heading=) **55**

[**Apéndice C**](#_heading=) **56**

[**Apéndice D**](#_heading=) **57**

[**Apéndice E**](#_heading=) **58**

# Introducción

FaceAttend EDU es una plataforma web y móvil destinada a la gestión de la asistencia en instituciones educativas. El sistema automatiza el registro de entrada y salida mediante reconocimiento facial, conserva una alternativa de registro manual, centraliza los históricos de asistencia y administra los flujos de justificación, los reportes diferenciados por rol y las alertas dirigidas a los actores académicos.

El presente documento explica, paso a paso, cómo preparar el entorno, configurar las dependencias y poner en funcionamiento el sistema en un equipo local o en un servidor, partiendo de una máquina sin instalación previa.

El sistema no constituye un programa único, sino un conjunto de nueve servicios independientes, implementados en cuatro lenguajes de programación distintos, que se despliegan de manera coordinada junto con una puerta de enlace, tres motores de almacenamiento y un intermediario de mensajería. Por esta razón, el procedimiento de instalación se apoya en contenedores, mecanismo que permite desplegar la totalidad del conjunto mediante un único comando.

La totalidad de la información técnica consignada —puertos, nombres de servicios, variables de configuración, rutas y credenciales iniciales— corresponde a la configuración verificada en el repositorio del proyecto.

La Tabla 2 delimita la función de este documento frente a los otros tres manuales que integran la documentación del proyecto.

**Tabla 2**

*Manuales del proyecto y pregunta que responde cada uno*

| **Documento**                                 | **Pregunta que responde**           | **Destinatario**                           |
|-----------------------------------------------|-------------------------------------|--------------------------------------------|
| Manual de usuario                             | ¿Cómo uso el sistema?               | Usuario final                              |
| Manual técnico                                | ¿Cómo está construido el sistema?   | Desarrollador o personal de soporte        |
| Manual de instalación (el presente documento) | ¿Cómo instalo y ejecuto el sistema? | Técnico responsable de la puesta en marcha |
| Manual de administrador                       | ¿Cómo administro el sistema?        | Usuario con rol administrador              |

# Objetivo

Permitir que una persona ajena al equipo desarrollador instale y ponga en funcionamiento el sistema completo siguiendo instrucciones claras, completas y verificables, sin requerir asistencia adicional.

## Objetivos Específicos

> • Enunciar los requisitos de hardware, de sistema operativo y de software que debe cumplir el equipo antes de iniciar el procedimiento.
>
> • Describir la instalación de las herramientas base y la obtención del código fuente del proyecto.
>
> • Explicar la configuración del sistema mediante el archivo único de variables de entorno y documentar cada variable disponible.
>
> • Detallar el despliegue del conjunto de contenedores, su orden de arranque y la creación automatizada de la base de datos.
>
> • Proporcionar un protocolo de verificación que permita confirmar, mediante pruebas concretas, que la instalación quedó operativa.
>
> • Reunir en un catálogo los problemas más frecuentes del procedimiento con sus respectivas soluciones.

# Alcance

La delimitación del alcance evita que el lector busque en este documento información que corresponde a otro de los manuales del proyecto.

## Contenido Cubierto por el Manual

> • Requisitos de hardware, de sistema operativo y de software.
>
> • Instalación de las herramientas base: plataforma de contenedores y control de versiones.
>
> • Obtención del código fuente del proyecto.
>
> • Configuración mediante variables de entorno.
>
> • Despliegue del conjunto de contenedores.
>
> • Creación de la base de datos y ejecución de las migraciones.
>
> • Verificación funcional de la instalación.
>
> • Solución de los problemas más frecuentes.
>
> • Instalación de la aplicación móvil en modo de desarrollo.

## Contenido No Cubierto por el Manual

> • El uso funcional del sistema, descrito en el manual de usuario.
>
> • La arquitectura interna y el código fuente, descritos en el manual técnico.
>
> • La gestión de usuarios, roles y reportes, descrita en el manual de administrador.
>
> • El despliegue en un entorno de producción con dominio público, certificados de seguridad de la capa de transporte y alta disponibilidad.
>
> • La configuración de la integración y la entrega continuas.

# Perfil del Instalador

El documento está dirigido al técnico o a la persona responsable de poner en marcha el sistema: el instructor que lo despliega con fines de evaluación, el integrante del equipo que configura su estación de trabajo o el administrador de sistemas que lo instala en un servidor institucional.

## Conocimientos Requeridos

Se presume que el lector sabe abrir una terminal y ejecutar comandos. No se presume, en cambio, conocimiento previo de contenedores, de arquitecturas de microservicios ni de alguno de los lenguajes de programación empleados en el proyecto: la información necesaria se expone en el propio documento y los términos técnicos se definen en el apartado Glosario.

## Responsabilidades del Instalador

> • Verificar el cumplimiento de la totalidad de los requisitos previos antes de descargar el proyecto.
>
> • Conservar el archivo de configuración fuera del repositorio y sustituir las credenciales de desarrollo cuando la instalación resulte accesible por terceros.
>
> • Ejecutar el protocolo de verificación completo antes de declarar concluida la instalación.
>
> • Documentar en el formato del Apéndice D cualquier incidencia que no se resuelva con el apartado Solución de Problemas.
>
> • Entregar al administrador del sistema las credenciales iniciales y advertir sobre la obligación de modificarlas.

## Límites del Rol

El instalador no administra el sistema una vez desplegado. La creación de usuarios reales, la asignación de roles, la configuración de los parámetros académicos y la generación de reportes corresponden al administrador y se describen en su propio manual. El instalador tampoco modifica el código fuente: cuando el procedimiento falla por un defecto del software, su responsabilidad consiste en reportarlo, no en corregirlo.

# Descripción General del Sistema

Conviene comprender la naturaleza de aquello que se va a instalar antes de iniciar el procedimiento.

## Naturaleza de la Instalación

El sistema está construido sobre una arquitectura de microservicios. En lugar de un programa único de gran tamaño, se compone de nueve servicios independientes, cada uno responsable de una porción delimitada del negocio: identidad, autorización, gestión académica, programación de horarios, asistencia, biometría, configuración, notificaciones y calidad. Delante de ellos opera una puerta de enlace que recibe la totalidad de las peticiones y las dirige al servicio correspondiente; detrás se sitúan las bases de datos y el intermediario de mensajería.

La instalación manual de este conjunto resultaría inviable, por cuanto cada servicio está implementado en un lenguaje de programación distinto —Java, TypeScript, Python y Go— y posee dependencias propias. Por este motivo, el proyecto emplea contenedores, mecanismo que empaqueta cada servicio junto con todos sus requisitos de ejecución (Docker Inc., s.f.-a), y una herramienta de orquestación que despliega los dieciocho contenedores en el orden correcto mediante un único comando (Docker Inc., s.f.-b).

La consecuencia práctica de esta decisión resulta relevante para el instalador: no se requiere instalar Java, Node.js, Python ni Go en el equipo, puesto que la plataforma de contenedores los descarga y los ejecuta en el interior de cada contenedor. Únicamente se precisan Docker y Git. La instalación manual de los lenguajes se describe en el apartado Instalación sin Contenedores y solo resulta necesaria cuando se pretende modificar el código fuente.

## Componentes del Sistema

El despliegue completo levanta dieciocho contenedores, agrupados en tres capas. Las Tablas 3, 4 y 5 detallan los componentes de la capa de presentación, de la capa de aplicación y de la capa de datos, respectivamente.

**Tabla 3**

*Componentes de la capa de presentación*

| **Contenedor** | **Tecnología**   | **Puerto** | **Función**                                                                         |
|----------------|------------------|------------|-------------------------------------------------------------------------------------|
| frontend-web   | Expo Web y nginx | 8090       | Interfaz gráfica que carga el navegador, construida como aplicación de página única |

**Tabla 4**

*Componentes de la capa de aplicación*

| **Contenedor**   | **Tecnología**        | **Puerto**  | **Función**                                                                                       |
|------------------|-----------------------|-------------|---------------------------------------------------------------------------------------------------|
| kong-gateway     | Kong OSS 3.6          | 8080 y 8001 | Puerta de enlace: enruta las peticiones, aplica política de origen cruzado y límite de peticiones |
| ms-identity      | Java 21 y Spring Boot | 8081        | Personas, usuarios, credenciales y sesiones                                                       |
| ms-authorization | Java 21 y Spring Boot | 8082        | Roles y permisos                                                                                  |
| ms-academic      | TypeScript y Fastify  | 8083        | Sedes, programas, periodos, fichas, cursos y matrículas                                           |
| ms-scheduling    | Java 21 y Spring Boot | 8084        | Ambientes, bloques de horario y sesiones de clase                                                 |
| ms-attendance    | Java 21 y Spring Boot | 8085        | Registros de asistencia y justificaciones                                                         |
| ms-biometric     | Python 3.12 y FastAPI | 8086        | Vectores biométricos faciales y dactilares                                                        |
| ms-configuration | TypeScript y Fastify  | 8087        | Parámetros académicos y de seguridad                                                              |
| ms-notification  | Go 1.22 y Gin         | 8088        | Alertas y envío de correo electrónico                                                             |
| ms-quality       | TypeScript y Fastify  | 8089        | Instrumento de evaluación de calidad                                                              |
| redis            | Redis 7               | 6379        | Almacén del límite de peticiones de la puerta de enlace                                           |

*Nota.* Los nueve microservicios se identifican con el prefijo ms y se numeran del 01 al 09 en el repositorio del proyecto.

**Tabla 5**

*Componentes de la capa de datos*

| **Contenedor**     | **Tecnología** | **Puerto** | **Función**                                                                                               |
|--------------------|----------------|------------|-----------------------------------------------------------------------------------------------------------|
| postgres           | PostgreSQL 17  | 5432       | Base de datos principal, organizada en ocho esquemas, uno por contexto delimitado                         |
| mongodb            | MongoDB 7      | 27017      | Almacenamiento de los vectores biométricos                                                                |
| kafka              | Kafka 3.8      | 9092       | Mensajería de eventos de dominio entre servicios                                                          |
| Migraciones (ocho) | Liquibase 4.29 | No aplica  | Tareas temporales que crean las tablas y cargan los datos iniciales; se ejecutan, concluyen y se detienen |

## Organización en Redes

Los contenedores se distribuyen en tres redes aisladas entre sí. El propósito de esta separación consiste en que la capa de datos resulte inalcanzable desde la red que atiende al navegador. La Figura 1 representa esta organización.

**Figura 1**

*Diagrama de despliegue y separación en redes*

```
+------------------------- EQUIPO ANFITRIÓN --------------------------+
|                                                                     |
|   navegador --:8090--> frontend-web (nginx)                         |
|                           |                                         |
|   +---------------------+ red: faceattend-edge                      |
|                           |                                         |
|   --:8080--> kong-gateway -------------> redis                      |
|                           |  red: faceattend-app                    |
|                           v                                         |
|   ms-identity · ms-authorization · ms-academic · ms-scheduling      |
|   ms-attendance · ms-biometric · ms-configuration                   |
|   ms-notification · ms-quality (puertos 8081 a 8089)                |
|                           |                                         |
|                           v  red: faceattend-data                   |
|   postgres · mongodb · kafka · migraciones (ocho)                   |
|                                                                     |
+---------------------------------------------------------------------+
```

*Nota.* Únicamente los puertos 8080, destinado a la interfaz de programación de aplicaciones, y 8090, destinado a la interfaz web, permanecen abiertos al exterior. Los restantes escuchan exclusivamente en la dirección de retorno 127.0.0.1, conforme al valor de la variable BIND_IP.

Para el instalador, esta organización implica que, una vez desplegado el sistema, solo se requieren dos direcciones: http://localhost:8090 para la interfaz gráfica y http://localhost:8080 para la interfaz de programación de aplicaciones. Las bases de datos no se exponen y no deben abrirse al exterior.

# Requisitos Previos

Corresponde verificar la totalidad de los requisitos de este apartado antes de descargar el proyecto. Un requisito incumplido ocasiona fallos de difícil diagnóstico en etapas posteriores del procedimiento.

## Requisitos de Hardware

El despliegue levanta dieciocho contenedores simultáneos, cuatro de los cuales ejecutan una máquina virtual de Java. El consumo de memoria resulta, en consecuencia, considerable, y constituye el requisito que con mayor frecuencia se incumple.

**Tabla 6**

*Requisitos mínimos y recomendados de hardware*

| **Recurso**                 | **Mínimo**        | **Recomendado** | **Observación**                                                                                                                           |
|-----------------------------|-------------------|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| Memoria de acceso aleatorio | 8 GB              | 16 GB           | Con 8 GB el sistema arranca, aunque conviene cerrar las demás aplicaciones. Deben asignarse al menos 6 GB a la plataforma de contenedores |
| Procesador                  | Cuatro núcleos    | Ocho núcleos    | Debe admitir virtualización por hardware y tenerla habilitada en el sistema básico de entrada y salida                                    |
| Espacio en disco            | 20 GB libres      | 30 GB libres    | Imágenes de contenedor, código fuente, dependencias y volúmenes de datos                                                                  |
| Conexión de red             | Acceso a internet | Banda ancha     | Necesaria únicamente durante la primera construcción, para descargar aproximadamente 5 GB                                                 |

*Nota.* Los valores de memoria se estimaron a partir del número de contenedores desplegados y del consumo característico de las cuatro máquinas virtuales de Java que integran el sistema.

**Advertencia.** La plataforma de contenedores no funciona cuando la virtualización por hardware se encuentra deshabilitada. En Windows, esta condición se verifica en el Administrador de tareas, pestaña Rendimiento, sección CPU, donde debe leerse «Virtualización: Habilitada». En caso contrario, corresponde activarla en el sistema básico de entrada y salida del equipo, bajo las denominaciones Intel VT-x, SVM Mode o Virtualization Technology, según el fabricante.

## Sistema Operativo

**Tabla 7**

*Sistemas operativos compatibles*

| **Sistema operativo**   | **Versión mínima**         | **Observación**                                                                                                            |
|-------------------------|----------------------------|----------------------------------------------------------------------------------------------------------------------------|
| Windows 10 y Windows 11 | Compilación 19044, 64 bits | Requiere el subsistema de Windows para Linux en su segunda versión, que se instala junto con la plataforma de contenedores |
| Ubuntu                  | 22.04 LTS                  | Requiere el motor de contenedores y el complemento de orquestación. Resultan igualmente compatibles Debian 12 y Fedora 39  |
| macOS                   | 13 (Ventura)               | Compatible con procesadores Intel y con procesadores de arquitectura Apple Silicon                                         |

## Software Requerido

Solo se requieren dos herramientas, dado que la plataforma de contenedores aporta el resto del entorno de ejecución.

**Tabla 8**

*Software requerido para la instalación*

| **Herramienta** | **Versión mínima** | **Fuente de obtención**    | **Comando de verificación** | **Finalidad**                                    |
|-----------------|--------------------|----------------------------|-----------------------------|--------------------------------------------------|
| Docker          | 24.0               | docker.com                 | docker --version            | Ejecutar cada servicio en un contenedor propio   |
| Docker Compose  | 2.20               | Incluido en Docker Desktop | docker compose version      | Desplegar y coordinar los dieciocho contenedores |
| Git             | 2.40               | git-scm.com                | git --version               | Descargar el código fuente del repositorio       |

**Nota.** La herramienta de orquestación admite dos formas de invocación. Las versiones actuales emplean docker compose, en dos palabras, por tratarse de un subcomando de Docker; las versiones anteriores empleaban docker-compose, con guion, por constituir un programa independiente. El presente manual utiliza siempre la forma actual. Si el equipo solo reconoce la forma con guion, corresponde actualizar la plataforma: el proyecto emplea la directiva *include*, que requiere la versión 2.20 o superior.

## Herramientas Opcionales

Las herramientas relacionadas a continuación no resultan necesarias para la instalación, pero facilitan tareas posteriores de revisión y diagnóstico.

**Tabla 9**

*Herramientas complementarias de apoyo*

| **Herramienta**    | **Situación de uso**                         | **Función**                                             |
|--------------------|----------------------------------------------|---------------------------------------------------------|
| Visual Studio Code | Revisión o edición del código                | Editor compatible con los cuatro lenguajes del proyecto |
| Postman o Insomnia | Prueba manual de la interfaz de programación | Envío de peticiones sin redactar comandos de terminal   |
| DBeaver o pgAdmin  | Consulta de la base de datos relacional      | Cliente gráfico para PostgreSQL                         |
| MongoDB Compass    | Revisión de los vectores biométricos         | Cliente gráfico para MongoDB                            |
| Mailpit            | Prueba del envío de correo electrónico       | Buzón de correo simulado para entornos de desarrollo    |

## Disponibilidad de Puertos

El sistema requiere quince puertos libres. Cuando alguno se encuentra ocupado por otro programa, el contenedor correspondiente no arranca.

**Tabla 10**

*Puertos requeridos y conflictos frecuentes*

| **Puerto**  | **Servicio**                          | **Exposición**       | **Conflicto frecuente**                                                        |
|-------------|---------------------------------------|----------------------|--------------------------------------------------------------------------------|
| 8080        | Puerta de enlace                      | Toda la red          | Tomcat, Jenkins u otro servidor de aplicaciones                                |
| 8090        | Interfaz web                          | Toda la red          | Poco frecuente                                                                 |
| 8001        | Administración de la puerta de enlace | Solo el equipo local | Poco frecuente                                                                 |
| 8081 a 8089 | Los nueve microservicios              | Solo el equipo local | Los puertos 8081 y 8085 suelen estar ocupados por otros entornos de desarrollo |
| 5432        | PostgreSQL                            | Solo el equipo local | Muy frecuente: una instalación previa de PostgreSQL en el equipo               |
| 27017       | MongoDB                               | Solo el equipo local | Una instalación previa de MongoDB                                              |
| 6379        | Redis                                 | Solo el equipo local | Una instalación previa de Redis                                                |
| 9092        | Kafka                                 | Solo el equipo local | Poco frecuente                                                                 |

### Comprobación de la Disponibilidad de un Puerto

En Windows, mediante el intérprete de comandos o PowerShell, se emplea la instrucción siguiente, sustituyendo el número por el puerto que se desea comprobar:

> netstat -ano | findstr ":5432"

En Linux y en macOS se emplea la instrucción siguiente:

> lsof -i :5432

**Verificación.** Cuando el comando no devuelve ninguna línea, el puerto se encuentra libre. Cuando devuelve una o más líneas, el puerto está ocupado: corresponde identificar el programa y detenerlo, o bien modificar el puerto en el archivo de configuración, según se explica en el apartado Configuración Inicial.

## Conectividad de Red

Durante la primera instalación, el equipo descarga imágenes de contenedor y dependencias desde internet. Si se trabaja detrás de un servidor intermediario corporativo o de un cortafuegos restrictivo, debe garantizarse el acceso a los dominios siguientes:

> • hub.docker.com y los subdominios de docker.io, para las imágenes base de los contenedores.
>
> • registry.npmjs.org, para las dependencias de los servicios en TypeScript y de las interfaces de usuario.
>
> • repo.maven.apache.org, para las dependencias de los servicios en Java.
>
> • pypi.org, para las dependencias del servicio en Python.
>
> • proxy.golang.org, para las dependencias del servicio en Go.
>
> • github.com, para la descarga del código fuente del proyecto.

# Descarga e Instalación

El presente apartado comprende la instalación de las herramientas base y la descarga del proyecto. Al concluirlo, el código fuente residirá en el equipo y la plataforma de contenedores estará en condiciones de construirlo.

## Instalación de Docker

### Procedimiento en Windows

> 1. Acceder a la dirección docker.com/products/docker-desktop y descargar la versión para Windows.
>
> 2. Ejecutar el instalador descargado y conservar marcada la opción de utilizar el subsistema de Windows para Linux en lugar de Hyper-V.
>
> 3. Reiniciar el equipo al concluir la instalación. El reinicio resulta obligatorio para que el subsistema quede activo.
>
> 4. Abrir la aplicación desde el menú de inicio y esperar a que el panel indique que el motor se encuentra en ejecución.

### Procedimiento en Ubuntu y Debian

> # 1. Instalar el motor de contenedores y el complemento de orquestacion
>
> curl -fsSL https://get.docker.com | sudo sh
>
> # 2. Permitir el uso sin privilegios de superusuario
>
> sudo usermod -aG docker $USER
>
> # 3. Cerrar la sesion y volver a iniciarla para aplicar el cambio de grupo

### Procedimiento en macOS

> 1. Descargar la versión para macOS, seleccionando la variante correspondiente al procesador del equipo: Apple Silicon o Intel.
>
> 2. Arrastrar la aplicación a la carpeta Aplicaciones y abrirla.
>
> 3. Conceder los permisos que solicite el sistema operativo.

### Verificación de la Instalación de Docker

> docker --version
>
> docker compose version
>
> docker run hello-world

**Verificación.** Los dos primeros comandos deben mostrar un número de versión. El tercero debe concluir con un mensaje que confirme que la instalación funciona correctamente. La aparición de dicho mensaje acredita que la plataforma quedó correctamente instalada.

## Instalación de Git

**Tabla 11**

*Procedimiento de instalación de Git por sistema operativo*

| **Sistema operativo** | **Procedimiento**                                                                                          |
|-----------------------|------------------------------------------------------------------------------------------------------------|
| Windows               | Descargar el instalador desde git-scm.com/download/win y ejecutarlo aceptando las opciones predeterminadas |
| Ubuntu y Debian       | Ejecutar: sudo apt update && sudo apt install -y git                                                       |
| macOS                 | Ejecutar: brew install git, o bien instalar las herramientas de línea de comandos de Xcode                 |

> git --version

**Verificación.** El comando debe mostrar la versión 2.40 o una superior.

## Obtención del Proyecto

Existen dos vías para obtener el código fuente. Corresponde emplear la primera cuando se dispone de acceso al repositorio.

### Primera Vía: Clonación del Repositorio

> # 1. Situarse en la carpeta donde se instalara el proyecto
>
> cd C:\Users\usuario>\Documents # Windows
>
> cd ~/Documents # Linux y macOS
>
> # 2. Clonar el repositorio
>
> git clone https://github.com/Jonas7891/ProyectoFaceAttendEDU.git
>
> # 3. Entrar a la carpeta que contiene el codigo
>
> cd ProyectoFaceAttendEDU/FULL

**Advertencia.** La totalidad de los comandos del presente manual se ejecutan desde la carpeta FULL, raíz del código fuente. Se trata de la única carpeta desde la cual la herramienta de orquestación localiza el archivo de configuración y los archivos de los tres niveles del sistema. La ejecución desde otra ubicación produce errores de configuración no encontrada.

### Segunda Vía: Archivo Comprimido

Cuando el proyecto se recibe como archivo comprimido, corresponde proceder del modo siguiente:

> 1. Descomprimir el archivo en una ruta que no contenga espacios ni tildes en su nombre, por cuanto dichas rutas ocasionan problemas en algunos comandos de la plataforma de contenedores.
>
> 2. Abrir una terminal en el interior de la carpeta FULL resultante.

## Verificación de la Estructura del Proyecto

Corresponde confirmar que la carpeta raíz contiene los elementos representados en la Figura 2.

**Figura 2**

*Estructura de carpetas del proyecto*

```
FULL/
|
+-- .env.example            plantilla de configuración
+-- docker-compose.yml      orquestador principal, punto de entrada
+-- COMPOSE.md              referencia de la infraestructura
|
+-- back-end/               los nueve microservicios y la puerta de enlace
|   +-- 01-ms-identity/        Java 21 y Spring Boot
|   +-- 02-ms-authorization/   Java 21 y Spring Boot
|   +-- 03-ms-academic/        TypeScript y Fastify
|   +-- 04-ms-scheduling/      Java 21 y Spring Boot
|   +-- 05-ms-attendance/      Java 21 y Spring Boot
|   +-- 06-ms-biometric/       Python 3.12 y FastAPI
|   +-- 07-ms-configuration/   TypeScript y Fastify
|   +-- 08-ms-notification/    Go 1.22 y Gin
|   +-- 09-ms-quality/         TypeScript y Fastify
|   +-- 99-api-gateway/        Kong OSS 3.6
|
+-- database/               base de datos y las ocho migraciones
|   +-- database-init/         creación de los ocho esquemas
|
+-- front-end/
|   +-- Web/                   aplicación web
|   +-- Mobile/                aplicación móvil
|
+-- tools/
    +-- seed-api-test-data/    carga opcional de datos de prueba
```

La comprobación se efectúa mediante el comando correspondiente al sistema operativo:

> dir # Windows
>
> ls -la # Linux y macOS

**Verificación.** Deben aparecer los archivos docker-compose.yml y .env.example, así como las carpetas back-end, database, front-end y tools. La ausencia de alguno de ellos indica que la descarga quedó incompleta, en cuyo caso corresponde repetir el procedimiento de obtención.

# Configuración Inicial

La totalidad de la configuración del sistema reside en un único archivo, denominado .env, ubicado en la raíz de la carpeta FULL. La herramienta de orquestación lo lee de forma automática y distribuye sus valores entre los dieciocho contenedores.

## Creación del Archivo de Configuración

El proyecto incluye una plantilla documentada denominada .env.example. El primer paso de la configuración consiste en copiarla con el nombre .env:

> # Desde la carpeta FULL
>
> copy .env.example .env # Windows, interprete de comandos
>
> Copy-Item .env.example .env # Windows, PowerShell
>
> cp .env.example .env # Linux y macOS

**Verificación.** Mediante el comando dir .env en Windows, o ls -la .env en Linux y macOS, debe constatarse que el archivo existe y ocupa aproximadamente 3 kilobytes.

**Nota.** Para una instalación local no resulta necesario modificar ningún valor, por cuanto la totalidad de las variables posee valores predeterminados funcionales. El lector puede continuar directamente con el apartado Puesta en Marcha y regresar al presente apartado únicamente si requiere modificar un puerto o si se presenta algún fallo.

**Advertencia.** El archivo de configuración nunca se incorpora al repositorio. Se encuentra excluido de forma deliberada, por cuanto contiene credenciales. Los valores de la plantilla constituyen credenciales de desarrollo y no secretos de producción. Para un entorno productivo, la totalidad de las contraseñas debe modificarse y gestionarse mediante variables del sistema operativo o un gestor de secretos, y nunca mediante un archivo perteneciente al proyecto.

## Variables de Entorno

El archivo se organiza en bloques temáticos. Las tablas siguientes constituyen la referencia completa de las variables disponibles.

**Tabla 12**

*Variables de configuración de PostgreSQL*

| **Variable**               | **Valor predeterminado**   | **Elemento que controla**                                                                      |
|----------------------------|----------------------------|------------------------------------------------------------------------------------------------|
| POSTGRES_USER              | postgres                   | Usuario administrador de la base de datos                                                      |
| POSTGRES_PASSWORD          | postgres                   | Contraseña de dicho usuario; debe modificarse en producción                                    |
| POSTGRES_DB                | faceattend_db              | Nombre de la base de datos que contiene los ocho esquemas                                      |
| POSTGRES_PORT              | 5432                       | Puerto en el equipo anfitrión; debe modificarse si existe una instalación previa de PostgreSQL |
| FACEATTEND_LIQUIBASE_IMAGE | liquibase/liquibase:4.29.0 | Imagen que ejecuta las ocho migraciones                                                        |

**Tabla 13**

*Variables de configuración de MongoDB*

| **Variable**   | **Valor predeterminado** | **Elemento que controla**                                           |
|----------------|--------------------------|---------------------------------------------------------------------|
| MONGO_USER     | mongoadmin               | Usuario de la base de datos documental                              |
| MONGO_PASSWORD | mongopass                | Contraseña de dicho usuario; debe modificarse en producción         |
| MONGO_DB       | faceattend_biometric     | Base de datos donde se almacenan los vectores faciales y dactilares |
| MONGO_PORT     | 27017                    | Puerto en el equipo anfitrión                                       |

**Tabla 14**

*Variables de la infraestructura de apoyo*

| **Variable**    | **Valor predeterminado** | **Elemento que controla**                                                               |
|-----------------|--------------------------|-----------------------------------------------------------------------------------------|
| REDIS_PORT      | 6379                     | Puerto del almacén que soporta el límite de peticiones                                  |
| KAFKA_PORT      | 9092                     | Puerto del intermediario de eventos de dominio                                          |
| KONG_PROXY_PORT | 8080                     | Puerto por el que ingresan la totalidad de las peticiones a la interfaz de programación |
| KONG_ADMIN_PORT | 8001                     | Puerto de administración de la puerta de enlace, de alcance local                       |

**Tabla 15**

*Variables de exposición en red e interfaz web*

| **Variable**        | **Valor predeterminado** | **Elemento que controla**                                                                                                                                                                  |
|---------------------|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| BIND_IP             | 127.0.0.1                | Dirección a la que se asocian los puertos internos. Con el valor predeterminado solo resultan accesibles desde el propio equipo; con el valor 0.0.0.0 quedan visibles en toda la red local |
| FRONTEND_PORT       | 8090                     | Puerto de la interfaz web                                                                                                                                                                  |
| EXPO_PUBLIC_API_URL | http://localhost:8080    | Dirección de la interfaz de programación que emplea el navegador                                                                                                                           |

**Advertencia.** La variable EXPO_PUBLIC_API_URL no se lee durante el arranque, sino que queda incorporada al paquete de la aplicación web en el momento de construir la imagen. Su modificación exige, por tanto, reconstruir la interfaz mediante el comando docker compose up -d --build frontend-web, y no basta con reiniciar el sistema. Esta circunstancia constituye la causa más frecuente de que la interfaz cargue sin lograr conectarse con la interfaz de programación.

**Tabla 16**

*Variables de usuarios iniciales y datos de prueba*

| **Variable**       | **Valor predeterminado** | **Elemento que controla**                                                     |
|--------------------|--------------------------|-------------------------------------------------------------------------------|
| BOOTSTRAP_USERNAME | admin.faceattend         | Usuario administrador creado automáticamente por las migraciones              |
| BOOTSTRAP_PASSWORD | Admin123!ChangeMe        | Contraseña de dicho usuario; debe modificarse tras el primer inicio de sesión |
| SEED_USERNAME      | seed.admin               | Usuario que emplea la herramienta de carga de datos de prueba                 |
| SEED_PASSWORD      | SeedAdmin123!            | Contraseña de dicho usuario                                                   |

## Casos de Personalización

### Primer Caso: El Puerto de la Base de Datos Está Ocupado

Cuando el equipo cuenta con una instalación previa de PostgreSQL, corresponde modificar el puerto en el equipo anfitrión. El puerto interno del contenedor permanece inalterado, de modo que el resto del sistema continúa funcionando sin cambios:

> POSTGRES_PORT=5433

### Segundo Caso: Prueba desde un Dispositivo Móvil

El procedimiento comprende tres modificaciones y una reconstrucción. En primer término, corresponde averiguar la dirección del equipo en la red local mediante ipconfig en Windows, o ip addr en Linux y macOS. A continuación se editan las variables:

> BIND_IP=0.0.0.0
>
> EXPO_PUBLIC_API_URL=http://192.168.1.10:8080

Finalmente se reconstruye la interfaz web:

> docker compose up -d --build frontend-web

**Advertencia.** La puerta de enlace solo admite peticiones procedentes de tres orígenes preconfigurados. Para servir la aplicación desde otra dirección, dicha dirección debe incorporarse a la lista de orígenes permitidos en la configuración de la puerta de enlace. Asimismo, corresponde restituir el valor 127.0.0.1 a la variable BIND_IP al concluir la prueba, por cuanto el valor 0.0.0.0 expone las bases de datos a la totalidad de la red local.

### Tercer Caso: El Puerto de la Puerta de Enlace Está Ocupado

> KONG_PROXY_PORT=8070
>
> EXPO_PUBLIC_API_URL=http://localhost:8070

Corresponde reconstruir posteriormente la interfaz web, por el motivo expuesto en el apartado anterior.

## Canal de Correo Electrónico

El servicio de notificaciones puede remitir mensajes de correo electrónico destinados a la recuperación de contraseña y a la verificación de cuentas. Cuando la variable del servidor de correo permanece vacía, el canal se desactiva: el sistema continúa funcionando con normalidad y se limita a registrar una advertencia en sus archivos de registro.

Para probar esta funcionalidad en desarrollo sin disponer de un servidor de correo real, corresponde desplegar un buzón simulado:

> docker run -d -p 1025:1025 -p 8025:8025 axllent/mailpit

Y configurar las variables siguientes:

> SMTP_HOST=host.docker.internal
>
> SMTP_PORT=1025
>
> SMTP_FROM=FaceAttend EDU <no-reply@faceattend.local>
>
> SMTP_STARTTLS=false

**Verificación.** Al acceder a la dirección http://localhost:8025 debe presentarse la bandeja en la que se reciben los mensajes remitidos por el sistema.

# Puesta en Marcha

Con el proyecto descargado y el archivo de configuración creado, el sistema se despliega mediante un único comando. La base de datos, las tablas y los datos iniciales se crean de forma automática.

## Despliegue del Sistema

Desde la carpeta FULL, con la plataforma de contenedores en ejecución, se emplea el comando siguiente:

> docker compose up -d --build

El comando se descompone del modo siguiente:

> • El verbo up crea y arranca los contenedores definidos.
>
> • La opción -d los mantiene en ejecución en segundo plano y devuelve el control de la terminal.
>
> • La opción --build construye las imágenes de los servicios antes de arrancarlos; resulta obligatoria en la primera ejecución.

**Advertencia.** La primera ejecución resulta prolongada, por cuanto la plataforma debe descargar las imágenes base y compilar los nueve servicios en cuatro lenguajes distintos. El tiempo estimado oscila entre quince y cuarenta minutos, según la velocidad de la conexión y la capacidad del equipo. Las ejecuciones posteriores demoran entre uno y tres minutos, dado que reutilizan los componentes previamente construidos. No corresponde interrumpir el proceso; en caso de cancelación, basta con ejecutar nuevamente el mismo comando.

## Orden de Arranque

La herramienta de orquestación no arranca la totalidad de los contenedores de forma simultánea, sino que respeta una cadena de dependencias destinada a impedir que un servicio intente emplear un recurso aún inexistente.

**Tabla 17**

*Fases del orden de arranque del sistema*

| **Fase** | **Componentes que arrancan**                                                                                                                                        | **Condición para avanzar**                                                                          |
|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| 1        | Base de datos relacional                                                                                                                                            | La base de datos responde correctamente a su comprobación de salud                                  |
| 2        | Las ocho migraciones, ejecutadas de forma consecutiva en el orden identidad, autorización, académico, horarios, asistencia, biometría, configuración y notificación | Cada migración debe concluir con éxito antes de que se inicie la siguiente                          |
| 3        | Los nueve microservicios                                                                                                                                            | Cada uno aguarda a su propia migración y a que la base de datos se encuentre en estado saludable    |
| 4        | Puerta de enlace                                                                                                                                                    | Aguarda al almacén de caché, al intermediario de mensajería y a los ocho microservicios principales |
| 5        | Interfaz web                                                                                                                                                        | Aguarda a que la puerta de enlace alcance el estado saludable                                       |

**Nota.** La consecuencia práctica de esta cadena consiste en que, cuando una migración falla, el proceso se detiene y los servicios que dependen de ella no arrancan. Ante cualquier dificultad de arranque corresponde, por tanto, revisar en primer término los registros de las migraciones.

## Base de Datos y Migraciones

No corresponde crear la base de datos ni ejecutar manualmente ningún archivo de instrucciones de consulta estructurada: el proceso es automático y comprende dos etapas.

### Creación de los Esquemas

La primera vez que la base de datos relacional arranca con su volumen de datos vacío, ejecuta un archivo de inicialización que crea los ocho esquemas correspondientes a los contextos delimitados del sistema: identidad, autorización, académico, horarios, asistencia, biometría, configuración y notificación.

**Nota.** La organización en ocho esquemas, y no en ocho bases de datos independientes, obedece a que cada microservicio es propietario exclusivo de su esquema y ningún servicio consulta las tablas de otro: las referencias entre contextos se establecen mediante identificadores, sin claves foráneas físicas. Se trata del patrón de base de datos por contexto, implementado sobre una sola instancia con el propósito de simplificar el despliegue.

### Creación de Tablas y Datos de Catálogo

Las ocho tareas de migración aplican, en orden, los cambios correspondientes a cada contexto: en primer término la estructura, compuesta por tablas, índices y restricciones, y a continuación los datos de catálogo indispensables (Liquibase, s.f.). Se trata de contenedores temporales que se ejecutan, concluyen y se detienen; su aparición con estado de salida correcto en el listado constituye el resultado esperado.

En los arranques posteriores, la herramienta de migración detecta los cambios previamente aplicados y ejecuta únicamente los nuevos, razón por la cual resulta seguro desplegar el sistema de forma cotidiana.

## Datos Iniciales del Sistema

Al concluir las migraciones, el sistema contiene los elementos mínimos requeridos para su utilización.

**Tabla 18**

*Roles predefinidos del sistema*

| **Rol**       | **Alcance de las atribuciones**                                                          |
|---------------|------------------------------------------------------------------------------------------|
| Administrador | Acceso total: gestiona usuarios, roles, estructura académica y configuración del sistema |
| Instructor    | Docencia: abre y cierra sesiones de clase, registra asistencia y revisa justificaciones  |
| Aprendiz      | Consulta de su propia información y remisión de justificaciones                          |

**Tabla 19**

*Usuarios creados automáticamente*

| **Usuario**           | **Contraseña inicial** | **Rol asignado** |
|-----------------------|------------------------|------------------|
| admin.faceattend      | Admin123!ChangeMe      | Administrador    |
| instructor.faceattend | Instructor123!ChangeMe | Instructor       |
| aprendiz.faceattend   | Aprendiz123!ChangeMe   | Aprendiz         |

*Nota.* El sufijo de las contraseñas advierte de forma expresa sobre la necesidad de modificarlas.

**Advertencia.** Las credenciales anteriores son de conocimiento público, por cuanto figuran en el código fuente del proyecto. Resultan admisibles en un entorno de desarrollo o de evaluación. En cualquier instalación accesible por terceros corresponde modificar las tres contraseñas de forma inmediata tras el primer inicio de sesión y desactivar las cuentas de demostración que no vayan a emplearse. El procedimiento se describe en el manual de administrador.

Se cargan, asimismo, los catálogos base del sistema: la matriz de permisos por rol, los tipos de actor académico, los tipos de justificación y los tipos de alerta.

## Carga de Datos de Prueba

Cuando se requiere el sistema poblado con sedes, programas, fichas, cursos y matrículas de ejemplo —situación habitual en demostraciones y en pruebas de las interfaces de usuario—, el proyecto incluye una herramienta que los crea mediante llamadas a la interfaz de programación. Su ejecución exige Node.js en su versión 20 o superior instalada en el equipo, dado que no emplea contenedores:

> # Con el sistema desplegado y la totalidad de los servicios saludables
>
> cd tools/seed-api-test-data
>
> npm run seed

**Verificación.** El proceso concluye con código de salida correcto. Su ejecución reiterada resulta segura, por cuanto la herramienta detecta los registros preexistentes y continúa sin duplicarlos.

# Verificación de la Instalación

La instalación no debe darse por concluida hasta completar el presente apartado. Cada prueba comprueba una capa distinta del sistema, por lo que corresponde ejecutarlas en el orden propuesto: un fallo temprano explica los fallos subsiguientes.

## Lista de Comprobación

La verificación se considera completa cuando se satisfacen las siete condiciones siguientes:

> 1. Los dieciocho contenedores figuran en el listado, y los de servicio se encuentran en ejecución y en estado saludable.
>
> 2. Las ocho tareas de migración concluyeron con código de salida correcto.
>
> 3. Cada microservicio responde a su prueba de salud.
>
> 4. La puerta de enlace responde en su puerto.
>
> 5. El inicio de sesión devuelve un identificador de sesión válido.
>
> 6. La interfaz web carga en el navegador y admite el inicio de sesión.
>
> 7. La capa de datos no resulta alcanzable desde la red que atiende al navegador.

## Primera Prueba: Estado de los Contenedores

> docker compose ps

**Verificación.** Debe presentarse una tabla de dieciocho filas. Los microservicios, la puerta de enlace, la interfaz web y las bases de datos deben figurar en ejecución, y aquellos que incorporan comprobación de salud deben figurar en estado saludable. Las ocho filas correspondientes a las migraciones deben figurar con estado de salida correcto, lo cual acredita que concluyeron su cometido.

**Advertencia.** Cuando algún contenedor figura en estado de reinicio permanente, de salud deficiente o de salida con error, la instalación no se encuentra completa. Corresponde revisar los registros de dicho contenedor y consultar el apartado Solución de Problemas antes de continuar.

Para visualizar adicionalmente los puertos publicados se emplea el comando siguiente:

> docker ps --format "{{.Names}} | {{.Status}} | {{.Ports}}"

## Segunda Prueba: Salud de los Servicios

Cada servicio expone una ruta que confirma su disponibilidad. Dichas rutas difieren según la tecnología de implementación:

> # Servicios implementados en Java
>
> curl http://localhost:8081/api/v1/health # identidad
>
> curl http://localhost:8084/actuator/health # horarios
>
> # Servicios implementados en TypeScript
>
> curl http://localhost:8083/health # academico
>
> curl http://localhost:8087/health # configuracion
>
> # A traves de la puerta de enlace
>
> curl http://localhost:8080/health/quality

**Verificación.** Cada comando devuelve una respuesta en notación de objetos de JavaScript con un estado positivo. Ninguno debe devolver un error de conexión.

**Nota.** Cuando la herramienta de línea de comandos no se encuentra disponible en Windows, puede emplearse en su lugar la instrucción Invoke-RestMethod de PowerShell, o bien acceder a la dirección directamente desde el navegador.

## Tercera Prueba: Autenticación

La presente constituye la prueba de mayor relevancia, por cuanto recorre la cadena completa —navegador, puerta de enlace, servicio de identidad y base de datos— y confirma que los datos iniciales se cargaron correctamente:

> curl -X POST http://localhost:8080/api/v1/auth/login \
>
> -H "Content-Type: application/json" \
>
> -d "{\identifier\:\admin.faceattend\,\password\:\Admin123!ChangeMe\}"

**Verificación.** Se obtiene una respuesta que contiene un identificador de sesión, el identificador del usuario y el estado activo de la sesión.

Dicho identificador se emplea a continuación para comprobar un extremo protegido:

> curl http://localhost:8080/api/v1/persons \
>
> -H "Authorization: Bearer <identificador-de-sesion>"

**Nota.** El sistema no emplea credenciales en formato de testigo web de JavaScript. Al iniciar sesión, el servicio de identidad emite un identificador de sesión opaco que se remite en cada petición mediante la cabecera de autorización. La puerta de enlace no valida credenciales: cada servicio comprueba la sesión por su cuenta. Por consiguiente, un error de no autorizado nunca procede de la puerta de enlace, sino del servicio de destino.

## Cuarta Prueba: Interfaz Web

> 1. Abrir un navegador actualizado.
>
> 2. Acceder a la dirección http://localhost:8090.
>
> 3. Constatar que carga la pantalla de inicio de sesión del sistema.
>
> 4. Iniciar sesión con las credenciales del usuario administrador.
>
> 5. Constatar el acceso al panel principal correspondiente al rol administrador.

**Verificación.** La interfaz carga, admite las credenciales y presenta el panel. Si la página carga pero el inicio de sesión no responde, corresponde abrir las herramientas de desarrollo del navegador y revisar la consola: los errores de conexión indican casi siempre que la dirección de la interfaz de programación no corresponde, situación tratada en el apartado Solución de Problemas.

Los servicios implementados en Java publican, además, su documentación interactiva en las direcciones http://localhost:8081/swagger-ui.html y equivalentes para los puertos 8082, 8084 y 8085.

## Quinta Prueba: Aislamiento de Red

La presente prueba confirma el funcionamiento de la separación en tres redes: ni la interfaz web ni la puerta de enlace deben alcanzar la base de datos.

> # Los dos comandos siguientes deben FALLAR
>
> docker exec faceattend-frontend-web nslookup postgres
>
> docker exec faceattend-kong getent hosts postgres
>
> # El comando siguiente debe FUNCIONAR
>
> docker exec faceattend-kong getent hosts ms-identity

**Verificación.** Los dos primeros comandos devuelven un error de resolución de nombre y el tercero devuelve una dirección de red. Si la interfaz web logra resolver el nombre de la base de datos, las redes no se encuentran correctamente separadas y la capa de datos resultaría alcanzable desde la capa expuesta al navegador.

Superadas las cinco pruebas, la instalación se considera completa y verificada. El sistema queda en condiciones de ser utilizado conforme al manual de usuario, o de ser configurado con usuarios reales conforme al manual de administrador.

# Instalación de la Aplicación Móvil

La aplicación móvil no se despliega mediante contenedores, sino que se ejecuta en modo de desarrollo y se abre en un teléfono físico o en un emulador. Su instalación resulta opcional respecto del sistema principal.

## Requisitos Adicionales

**Tabla 20**

*Requisitos adicionales de la aplicación móvil*

| **Requisito**              | **Detalle**                                                                                                      |
|----------------------------|------------------------------------------------------------------------------------------------------------------|
| Node.js                    | Versión 20 o superior, instalada en el equipo                                                                    |
| Aplicación cliente de Expo | Instalada en el teléfono, desde la tienda de aplicaciones correspondiente                                        |
| Red                        | El teléfono y el equipo deben hallarse en la misma red inalámbrica                                               |
| Sistema principal          | Debe encontrarse desplegado y accesible desde la red local, con la variable de exposición configurada en 0.0.0.0 |

## Procedimiento de Instalación

> # 1. Averiguar la direccion del equipo en la red local
>
> ipconfig # Windows
>
> ip addr # Linux
>
> ifconfig # macOS
>
> # 2. Entrar a la carpeta de la aplicacion movil
>
> cd front-end/Mobile
>
> # 3. Instalar las dependencias (solo la primera vez)
>
> npm install
>
> # 4. Arrancar el servidor de desarrollo
>
> npx expo start

**Advertencia.** Desde el teléfono, la denominación de retorno localhost designa al propio teléfono y no al computador. Resulta obligatorio emplear la dirección del equipo en la red local en la configuración de la aplicación móvil, y mantener la variable de exposición configurada en 0.0.0.0 en el archivo de configuración del sistema principal.

Al arrancar, la herramienta de desarrollo presenta un código de respuesta rápida en la terminal. Corresponde abrir la aplicación cliente en el teléfono y escanear dicho código, tras lo cual la aplicación se descarga al dispositivo y se ejecuta.

**Verificación.** La aplicación presenta la pantalla de inicio de sesión y admite el ingreso con las credenciales del usuario aprendiz. Si la pantalla carga pero el inicio de sesión falla, el problema reside en la dirección de la interfaz de programación o en el cortafuegos del equipo, que puede estar bloqueando el puerto de la puerta de enlace.

# Operación Cotidiana

## Comandos de Uso Frecuente

**Tabla 21**

*Comandos de operación frecuente*

| **Comando**                                 | **Efecto**                                                                     |
|---------------------------------------------|--------------------------------------------------------------------------------|
| docker compose up -d                        | Arranca el sistema reutilizando las imágenes previamente construidas           |
| docker compose up -d --build                | Reconstruye las imágenes antes de arrancar; se emplea tras modificar el código |
| docker compose ps                           | Presenta el estado y la salud de cada contenedor                               |
| docker compose logs -f ms-identity          | Presenta los registros de un servicio en tiempo real                           |
| docker compose logs --tail=100 kong-gateway | Presenta las últimas cien líneas de registro de un servicio                    |
| docker compose restart ms-academic          | Reinicia un servicio individual                                                |
| docker compose config                       | Valida la configuración sin desplegar el sistema                               |
| docker compose down                         | Detiene y elimina los contenedores conservando los datos                       |
| docker stats                                | Presenta el consumo de procesador y memoria de cada contenedor                 |

## Detención y Reinicio del Sistema

> # Detener el sistema conservando la totalidad de los datos
>
> docker compose down
>
> # Arrancarlo nuevamente
>
> docker compose up -d

**Nota.** La detención del sistema elimina los contenedores pero no los volúmenes de datos: la base de datos, los vectores biométricos y los eventos permanecen intactos. Al desplegarlo nuevamente, el sistema continúa en el estado en que quedó.

## Actualización a una Versión Nueva

> # 1. Incorporar los cambios del repositorio
>
> git pull origin develop
>
> # 2. Revisar si la plantilla de configuracion incorporo variables nuevas
>
> # y, en caso afirmativo, agregarlas al archivo de configuracion local
>
> # 3. Reconstruir y arrancar; las migraciones pendientes se aplican solas
>
> docker compose up -d --build

## Desinstalación

**Advertencia.** El primer comando del bloque siguiente elimina de forma irreversible la totalidad de los datos del sistema: usuarios, registros de asistencia, justificaciones y vectores biométricos. La operación no admite reversión, por lo que debe ejecutarse únicamente cuando se pretende partir de cero.

> # Elimina contenedores y volumenes de datos
>
> docker compose down -v
>
> # Elimina ademas las imagenes construidas
>
> docker compose down -v --rmi all
>
> # Finalmente, eliminar la carpeta del proyecto si ya no se requiere

# Solución de Problemas

El presente apartado reúne las dificultades que se presentan con mayor frecuencia, ordenadas según el momento del procedimiento en que suelen manifestarse. Cada entrada consigna el síntoma, la causa probable y las instrucciones conducentes a su resolución.

## Diagnóstico Inicial

Ante cualquier fallo corresponde ejecutar los tres comandos siguientes, que en la mayoría de los casos identifican la causa:

> docker compose ps # que contenedor presenta la falla
>
> docker compose logs --tail=50 <nombre> # que informa dicho contenedor
>
> docker compose config # si la configuracion es valida

## Problemas Durante la Instalación

**Tabla 22**

*Problemas frecuentes durante la instalación*

| **Síntoma**                                            | **Causa probable**                                                                     | **Solución**                                                                                                                                      |
|--------------------------------------------------------|----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| El sistema no reconoce el comando docker               | La plataforma no está instalada o no figura en la variable de rutas del sistema        | Reinstalar la plataforma y reiniciar el equipo. En Linux, cerrar la sesión y volver a iniciarla tras agregar el usuario al grupo correspondiente  |
| No es posible conectar con el servicio de contenedores | El servicio no se encuentra en ejecución                                               | Abrir la aplicación de escritorio y aguardar a que informe que el motor está activo. En Linux, iniciar el servicio mediante el gestor del sistema |
| No se proporciona archivo de configuración             | El comando se ejecuta desde una carpeta distinta de la raíz del código                 | Situarse en la carpeta que contiene el archivo de orquestación                                                                                    |
| La construcción falla al descargar dependencias        | Ausencia de conexión, o un servidor intermediario corporativo bloquea los repositorios | Verificar la conexión y el acceso a los dominios relacionados en el apartado Requisitos Previos, y reintentar la construcción sin caché           |
| No queda espacio en el dispositivo                     | Disco saturado por imágenes y contenedores en desuso                                   | Depurar los recursos no utilizados y confirmar que restan al menos 20 GB libres                                                                   |

## Problemas al Desplegar el Sistema

### La Dirección del Puerto ya Está Asignada

La causa reside en que otro programa del equipo emplea dicho puerto. El caso más frecuente corresponde a una instalación previa de PostgreSQL que ocupa el puerto 5432.

> # 1. Identificar el proceso que ocupa el puerto
>
> netstat -ano | findstr ":5432" # Windows
>
> lsof -i :5432 # Linux y macOS
>
> # 2a. Detener dicho proceso
>
> taskkill /PID <identificador> /F # Windows
>
> kill -9 <identificador> # Linux y macOS
>
> # 2b. O bien modificar el puerto en el archivo de configuracion
>
> docker compose up -d

### Un Servicio Declara una Dependencia No Definida

El error se produce al ejecutar el despliegue desde la carpeta de los servicios en lugar de la raíz del proyecto. Dicho archivo, por sí solo, no define la base de datos, que reside en otra carpeta. La solución consiste en ejecutar siempre el despliegue desde la raíz. Se trata de un error deliberado del diseño, destinado a evidenciar cuál constituye el punto de entrada correcto.

### Un Contenedor Permanece en Reinicio Indefinido

> docker compose logs --tail=100 <nombre-del-servicio>

Las causas habituales comprenden que la base de datos aún no se encontraba disponible, situación que se resuelve por sí sola tras algunos reintentos; que falte una variable de entorno; o que el servicio no logre resolver el nombre de otro contenedor. Si transcurridos cinco minutos el contenedor continúa reiniciándose, corresponde reiniciar el sistema completo.

### La Interfaz Web Figura en Estado de Salud Deficiente

La causa conocida consiste en que la comprobación de salud debe emplear la dirección numérica de retorno y no su denominación, por cuanto en el interior del contenedor dicha denominación resuelve también a la dirección de la sexta versión del protocolo de internet, en la que el servidor web no escucha, de modo que la conexión se rechaza. El archivo del proyecto ya incorpora la corrección; ante la aparición del síntoma, corresponde reconstruir la interfaz web.

## Problemas con las Migraciones

### Una Migración Concluye con Error

La cadena de migraciones se detiene y ningún servicio posterior arranca. Corresponde identificar cuál falló y por qué motivo:

> # 1. Identificar la migracion fallida
>
> docker compose ps | findstr liquibase # Windows
>
> docker compose ps | grep liquibase # Linux y macOS
>
> # 2. Consultar el motivo de la falla
>
> docker compose logs identity-liquibase
>
> # 3. Tras corregir, reejecutar unicamente esa migracion
>
> docker compose up identity-liquibase
>
> # 4. La cadena prosigue en el siguiente arranque
>
> docker compose up -d

**Advertencia.** Como último recurso, cuando la base de datos quede en estado inconsistente y se trate de un entorno de pruebas, puede reiniciarse desde cero eliminando los volúmenes. Dicha operación borra la totalidad de los datos y nunca debe ejecutarse en una instalación que contenga información real.

### El Sistema Informa que una Tabla o un Esquema No Existe

La causa consiste en que las migraciones no se ejecutaron o quedaron incompletas. Corresponde verificar que las ocho tareas concluyeron con código de salida correcto y revisar los registros de la primera que no lo haya hecho.

## Problemas de Conexión y Uso

**Tabla 23**

*Problemas frecuentes de conexión y uso*

| **Síntoma**                                                          | **Causa probable**                                                                                     | **Solución**                                                                                                                       |
|----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Pantalla en blanco en la dirección de la interfaz web                | La interfaz aún no concluyó su arranque, o su construcción falló                                       | Aguardar un minuto y recargar. De persistir, consultar los registros de la interfaz web                                            |
| La interfaz carga pero no se conecta con la interfaz de programación | La dirección configurada no corresponde a la real; dicha variable se incorpora durante la construcción | Corregir el valor en el archivo de configuración y reconstruir la interfaz web                                                     |
| La puerta de enlace responde con error de pasarela                   | El microservicio de destino aún no alcanza el estado saludable                                         | Identificar el servicio pendiente y revisar sus registros hasta que alcance dicho estado                                           |
| La puerta de enlace informa que ninguna ruta coincide                | La ruta solicitada no se encuentra declarada en la configuración de la puerta de enlace                | Comprobar la ruta en el Apéndice B. Las rutas válidas se inician con el prefijo de versión de la interfaz de programación          |
| Todas las llamadas devuelven error de no autorizado                  | Falta la cabecera de sesión, la sesión expiró, o el usuario carece de roles asignados                  | Obtener un identificador nuevo mediante el extremo de inicio de sesión y remitirlo en la cabecera de autorización de cada petición |
| Las llamadas devuelven error de acceso prohibido                     | La sesión es válida pero el rol carece del permiso requerido                                           | Verificar los roles asignados al usuario mediante el extremo correspondiente                                                       |
| El navegador informa un error de política de origen cruzado          | El acceso se realiza desde un origen no autorizado en la puerta de enlace                              | Incorporar dicho origen a la lista de orígenes permitidos en la configuración de la puerta de enlace                               |
| No se reciben los mensajes de correo electrónico                     | La variable del servidor de correo permanece vacía, de modo que el canal está desactivado              | Constituye el comportamiento predeterminado. Para activarlo, configurar el canal conforme al apartado Configuración Inicial        |

## Problemas de Rendimiento

**Tabla 24**

*Problemas frecuentes de rendimiento*

| **Síntoma**                                                   | **Causa y solución**                                                                                                                                                                                                                                                                                        |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| El equipo se torna considerablemente lento tras el despliegue | Los dieciocho contenedores, cuatro de ellos con máquina virtual de Java, consumen memoria de forma apreciable. Corresponde cerrar las demás aplicaciones y asignar al menos 6 GB de memoria a la plataforma de contenedores. El consumo por contenedor puede examinarse mediante el comando de estadísticas |
| La construcción se prolonga más de una hora                   | Constituye un comportamiento normal en la primera ejecución con conexión lenta, dado que se descargan aproximadamente 5 GB. Las ejecuciones posteriores resultan considerablemente más rápidas. No corresponde interrumpir el proceso                                                                       |
| Los contenedores se detienen por sí solos                     | La plataforma agotó la memoria asignada. Corresponde incrementarla o desplegar únicamente los servicios requeridos                                                                                                                                                                                          |

## Procedimiento de Reinicio Limpio

Cuando se agotan las alternativas anteriores y el sistema continúa sin funcionar, el procedimiento siguiente restituye el estado propio de una instalación nueva:

> # 1. Detener y eliminar todo, incluidos los datos
>
> docker compose down -v
>
> # 2. Depurar los recursos no utilizados
>
> docker system prune -a
>
> # 3. Verificar que el archivo de configuracion existe y es correcto
>
> # 4. Reconstruir desde cero
>
> docker compose up -d --build

**Advertencia.** El procedimiento elimina la totalidad de los datos del sistema, por lo que debe emplearse exclusivamente en entornos de desarrollo o de evaluación.

## Escalamiento de Incidencias

Cuando una dificultad no se resuelve mediante los procedimientos anteriores, corresponde reportarla empleando el formato del Apéndice D. El reporte debe adjuntar la salida de los tres comandos de diagnóstico inicial; sin dicha evidencia, el diagnóstico remoto resulta prácticamente inviable.

# Instalación sin Contenedores

**Advertencia.** El presente apartado reviste carácter opcional y no se recomienda para una instalación ordinaria. Resulta pertinente únicamente cuando se pretende modificar el código de un servicio y depurarlo desde el entorno de desarrollo. Para instalar y emplear el sistema, el procedimiento basado en contenedores resulta más rápido y considerablemente menos propenso a errores.

## Herramientas Requeridas por Lenguaje de Programación

Cada servicio exige su propio entorno de ejecución instalado en el equipo.

**Tabla 25**

*Herramientas requeridas por lenguaje de programación*

| **Lenguaje** | **Versión** | **Servicios afectados**                              | **Fuente**       | **Verificación** |
|--------------|-------------|------------------------------------------------------|------------------|------------------|
| Java         | 21 LTS      | Identidad, autorización, horarios y asistencia       | adoptium.net     | java --version   |
| Maven        | 3.9         | Los mismos cuatro servicios                          | maven.apache.org | mvn --version    |
| Node.js      | 20          | Académico, configuración, calidad y ambas interfaces | nodejs.org       | node --version   |
| Python       | 3.12        | Biometría                                            | python.org       | python --version |
| Go           | 1.22        | Notificaciones                                       | go.dev           | go version       |

## Enfoque Recomendado: Modo Híbrido

La práctica habitual no consiste en prescindir por completo de los contenedores, sino en desplegar mediante ellos la totalidad de la infraestructura —bases de datos, mensajería, puerta de enlace y los servicios que no se están modificando— y ejecutar fuera de ellos únicamente el servicio objeto del trabajo:

> # 1. Desplegar unicamente la capa de datos y las migraciones
>
> docker compose up -d database
>
> # 2. Ejecutar un servicio desde su carpeta, segun el lenguaje
>
> cd back-end/01-ms-identity && mvn spring-boot:run
>
> cd back-end/03-ms-academic && npm install && npm run dev
>
> cd back-end/06-ms-biometric && uvicorn main:app --reload
>
> cd back-end/08-ms-notification && go run ./cmd/server

**Nota.** Los servicios leen el mismo archivo de configuración empleado por los contenedores. Al arrancar fuera de ellos, cada servicio carga la configuración desde la raíz del proyecto mediante el mecanismo propio de su lenguaje, de modo que no corresponde duplicar la configuración.

## Interfaz Web en Modo de Desarrollo

> cd front-end/Web
>
> npm install
>
> npx expo start --web

La interfaz arranca con recarga automática al guardar los cambios.

# Recomendaciones de Seguridad

Las indicaciones siguientes aplican desde el momento en que la instalación deja de ser un entorno de pruebas aislado y pasa a resultar accesible por terceros.

## Credenciales

> • Modificar las tres contraseñas de arranque inmediatamente después del primer inicio de sesión y desactivar las cuentas de demostración que no vayan a emplearse.
>
> • Sustituir las credenciales predeterminadas de las bases de datos relacional y documental por valores propios de la instalación.
>
> • No compartir el archivo de configuración por canales de mensajería ni adjuntarlo a reportes de incidencia sin suprimir previamente las contraseñas.
>
> • Conservar el archivo de configuración fuera del repositorio; su exclusión ya está declarada en el proyecto y no debe revertirse.

## Exposición en Red

> • Mantener la variable de exposición en la dirección de retorno, de modo que las bases de datos y los microservicios resulten inalcanzables desde la red local.
>
> • Restituir dicho valor al concluir cualquier prueba que haya exigido abrir los puertos a la red.
>
> • Publicar únicamente los dos puertos previstos para el exterior: el de la interfaz web y el de la puerta de enlace.
>
> • Verificar el aislamiento entre redes mediante la quinta prueba del apartado Verificación de la Instalación después de cada cambio en la infraestructura.

## Conservación de la Información

> • Ejecutar el comando de detención sin la opción de eliminación de volúmenes en la operación cotidiana, por cuanto dicha opción destruye la totalidad de los datos sin posibilidad de reversión.
>
> • Generar una copia de seguridad antes de cualquier actualización que incorpore migraciones nuevas, conforme al procedimiento descrito en el manual de administrador.
>
> • Reservar el procedimiento de reinicio limpio para entornos de desarrollo o de evaluación.

## Entrega al Administrador del Sistema

Al concluir la instalación, el instalador entrega al administrador las credenciales iniciales, la advertencia sobre su modificación obligatoria y la constancia de que el protocolo de verificación se ejecutó en su totalidad. A partir de ese momento, la responsabilidad sobre el sistema corresponde al administrador y se rige por su propio manual.

# Glosario

La tabla siguiente define los términos técnicos empleados en el presente manual, en el contexto específico del proyecto.

**Tabla 26**

*Glosario de términos del sistema*

| **Término**                       | **Definición**                                                                                                                                                                                                                                     |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Contenedor                        | Paquete que comprende un programa y la totalidad de los elementos necesarios para su funcionamiento: sistema base, bibliotecas y configuración. Se ejecuta aislado del resto del equipo, de modo que opera de manera idéntica en cualquier máquina |
| Imagen                            | Plantilla de solo lectura a partir de la cual se crean los contenedores. Construir una imagen equivale a compilar el servicio y empaquetarlo; arrancar un contenedor equivale a ejecutar una copia de dicha imagen                                 |
| Docker                            | Plataforma que construye y ejecuta contenedores                                                                                                                                                                                                    |
| Docker Compose                    | Herramienta que despliega varios contenedores relacionados mediante un único comando, respetando el orden de dependencias declarado en su archivo de configuración                                                                                 |
| Volumen                           | Espacio de disco administrado por la plataforma de contenedores, donde estos almacenan los datos que deben subsistir a su eliminación                                                                                                              |
| Microservicio                     | Servicio independiente, responsable de una sola porción del negocio, con base de datos y ciclo de despliegue propios                                                                                                                               |
| Puerta de enlace                  | Componente que recibe la totalidad de las peticiones externas y las dirige al microservicio correspondiente. Aplica, asimismo, la política de origen cruzado y el límite de peticiones                                                             |
| Kong                              | Puerta de enlace de código abierto empleada en el proyecto. Se configura de forma declarativa, sin base de datos propia                                                                                                                            |
| Extremo                           | Dirección concreta de la interfaz de programación que atiende una operación determinada                                                                                                                                                            |
| Esquema                           | Subdivisión lógica en el interior de una base de datos relacional. El sistema emplea una sola base dividida en ocho esquemas, uno por contexto                                                                                                     |
| Migración                         | Cambio versionado en la estructura de la base de datos. Las migraciones se aplican en orden y quedan registradas, de modo que nunca se ejecutan dos veces                                                                                          |
| Liquibase                         | Herramienta que ejecuta las migraciones. En el proyecto comprende ocho tareas, una por esquema, que se ejecutan automáticamente durante el despliegue                                                                                              |
| Datos semilla                     | Registros mínimos que el sistema requiere para su arranque: roles, permisos, tipos de justificación y usuario administrador inicial                                                                                                                |
| Variable de entorno               | Valor de configuración definido fuera del código, que el programa lee durante su arranque. Permite modificar puertos o credenciales sin alterar el código fuente                                                                                   |
| Puerto                            | Número que identifica un canal de comunicación en un equipo. Dos programas no pueden emplear simultáneamente el mismo puerto                                                                                                                       |
| Dirección de retorno              | Dirección que designa al propio equipo. Un servicio asociado a ella resulta accesible únicamente desde dicha máquina                                                                                                                               |
| Comprobación de salud             | Verificación periódica que la plataforma de contenedores ejecuta sobre un contenedor para determinar su correcto funcionamiento                                                                                                                    |
| Política de origen cruzado        | Mecanismo de seguridad del navegador que impide las peticiones dirigidas a un dominio distinto del de la página, salvo autorización expresa de dicho dominio                                                                                       |
| Sesión opaca                      | Identificador carente de contenido legible que el servidor entrega al iniciar sesión y que el cliente remite en cada petición                                                                                                                      |
| Control de acceso basado en roles | Modelo en el que los permisos se asignan a roles y los roles se asignan a usuarios                                                                                                                                                                 |
| Kafka                             | Sistema de mensajería que permite a los microservicios comunicarse de forma asíncrona mediante eventos, sin invocarse directamente                                                                                                                 |
| Redis                             | Almacén de datos en memoria. En el proyecto conserva los contadores del límite de peticiones                                                                                                                                                       |
| MongoDB                           | Base de datos orientada a documentos. En el proyecto almacena los vectores biométricos                                                                                                                                                             |
| Expo                              | Plataforma que permite construir, a partir de un mismo código, la aplicación web y la aplicación móvil                                                                                                                                             |
| nginx                             | Servidor web que entrega al navegador los archivos previamente construidos de la interfaz                                                                                                                                                          |
| Subsistema de Windows para Linux  | Capa de Windows que permite ejecutar Linux. La plataforma de contenedores la emplea para ejecutar los contenedores                                                                                                                                 |

# Referencias

Apache Software Foundation. (s.f.). *Apache Kafka documentation*. Recuperado el 6 de octubre de 2026, de https://kafka.apache.org/documentation/

Docker Inc. (s.f.-a). *Docker docs*. Recuperado el 6 de octubre de 2026, de https://docs.docker.com/

Docker Inc. (s.f.-b). *Docker Compose documentation*. Recuperado el 6 de octubre de 2026, de https://docs.docker.com/compose/

Equipo desarrollador FaceAttend EDU. (2026a). *COMPOSE.md: Docker Compose y redes* [Documento interno no publicado]. Proyecto formativo FaceAttend EDU, Servicio Nacional de Aprendizaje.

Equipo desarrollador FaceAttend EDU. (2026b). *FaceAttend EDU* (Versión 1.0) [Software]. GitHub. https://github.com/Jonas7891/ProyectoFaceAttendEDU

Equipo desarrollador FaceAttend EDU. (2026c). *SEEDS.md: registros predefinidos* [Documento interno no publicado]. Proyecto formativo FaceAttend EDU, Servicio Nacional de Aprendizaje.

Equipo desarrollador FaceAttend EDU. (2026d). *SERVICES.md: guía general de microservicios* [Documento interno no publicado]. Proyecto formativo FaceAttend EDU, Servicio Nacional de Aprendizaje.

Expo. (s.f.). *Expo documentation*. Recuperado el 6 de octubre de 2026, de https://docs.expo.dev/

Git. (s.f.). *Git documentation*. Recuperado el 6 de octubre de 2026, de https://git-scm.com/doc

Kong Inc. (s.f.). *Kong Gateway documentation*. Recuperado el 6 de octubre de 2026, de https://docs.konghq.com/gateway/

Liquibase. (s.f.). *Liquibase documentation*. Recuperado el 6 de octubre de 2026, de https://docs.liquibase.com/

MongoDB Inc. (s.f.). *MongoDB manual*. Recuperado el 6 de octubre de 2026, de https://www.mongodb.com/docs/manual/

PostgreSQL Global Development Group. (s.f.). *PostgreSQL 17 documentation*. Recuperado el 6 de octubre de 2026, de https://www.postgresql.org/docs/17/

Redis Ltd. (s.f.). *Redis documentation*. Recuperado el 6 de octubre de 2026, de https://redis.io/docs/

# Apéndice A

Mapa Completo de Puertos

El presente apéndice reúne, en un solo lugar, la totalidad de los puertos empleados por el sistema, su grado de exposición y la dirección mediante la cual se accede a cada uno.

**Tabla 27**

*Mapa completo de puertos del sistema*

| **Puerto** | **Servicio**                          | **Exposición** | **Dirección de acceso**               |
|------------|---------------------------------------|----------------|---------------------------------------|
| 8090       | Interfaz web                          | Toda la red    | http://localhost:8090                 |
| 8080       | Puerta de enlace                      | Toda la red    | http://localhost:8080                 |
| 8001       | Administración de la puerta de enlace | Local          | http://localhost:8001                 |
| 8081       | Identidad                             | Local          | http://localhost:8081/swagger-ui.html |
| 8082       | Autorización                          | Local          | http://localhost:8082/swagger-ui.html |
| 8083       | Académico                             | Local          | http://localhost:8083/health          |
| 8084       | Horarios                              | Local          | http://localhost:8084/actuator/health |
| 8085       | Asistencia                            | Local          | http://localhost:8085/actuator/health |
| 8086       | Biometría                             | Local          | http://localhost:8086/docs            |
| 8087       | Configuración                         | Local          | http://localhost:8087/health          |
| 8088       | Notificaciones                        | Local          | http://localhost:8088                 |
| 8089       | Calidad                               | Local          | http://localhost:8089                 |
| 5432       | PostgreSQL                            | Local          | localhost:5432                        |
| 27017      | MongoDB                               | Local          | localhost:27017                       |
| 6379       | Redis                                 | Local          | localhost:6379                        |
| 9092       | Kafka                                 | Local          | localhost:9092                        |

*Nota.* La exposición denominada local corresponde al valor de la variable BIND_IP, cuyo valor predeterminado es 127.0.0.1.

# Apéndice B

Rutas Principales de la Interfaz de Programación

La totalidad de las rutas relacionadas a continuación transitan por la puerta de enlace y requieren la cabecera de autorización con el identificador de sesión, con excepción del extremo de inicio de sesión.

**Tabla 28**

*Rutas de la API por servicio de destino*

| **Servicio de destino** | **Rutas**                                                                                                                                   |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Identidad               | /api/v1/auth, /api/v1/persons, /api/v1/users, /api/v1/sessions, /api/v1/cities, /api/v1/password-policies                                   |
| Autorización            | /api/v1/roles, /api/v1/permissions, /api/v1/auth/evaluate, /api/v1/users/{id}/roles                                                         |
| Académico               | /api/v1/schools, /api/v1/programs, /api/v1/academic-periods, /api/v1/cohorts, /api/v1/courses, /api/v1/academic-actors, /api/v1/enrollments |
| Horarios                | /api/v1/environments, /api/v1/schedule-blocks, /api/v1/class-sessions                                                                       |
| Asistencia              | /api/v1/attendance-records, /api/v1/justifications, /api/v1/justification-types, /api/v1/supporting-documents                               |
| Biometría               | /api/v1/biometric                                                                                                                           |
| Configuración           | /api/v1/academic-configurations, /api/v1/security-configurations, /api/v1/biometric-update-cases                                            |
| Notificaciones          | /api/v1/alert-types, /api/v1/alerts                                                                                                         |
| Calidad                 | /api/v1/quality, /health/quality                                                                                                            |

# Apéndice C

Credenciales Iniciales del Sistema

**Advertencia.** Las credenciales relacionadas a continuación son de conocimiento público y de uso exclusivo en entornos de desarrollo. Corresponde modificarlas tan pronto como la instalación resulte accesible por terceros.

**Tabla 29**

*Credenciales iniciales creadas por las migraciones*

| **Usuario**           | **Contraseña**         | **Rol**       |
|-----------------------|------------------------|---------------|
| admin.faceattend      | Admin123!ChangeMe      | Administrador |
| instructor.faceattend | Instructor123!ChangeMe | Instructor    |
| aprendiz.faceattend   | Aprendiz123!ChangeMe   | Aprendiz      |

# Apéndice D

Formato de Reporte de Incidencias

Cuando una dificultad no se resuelve mediante el apartado Solución de Problemas, corresponde reportarla consignando la información que se relaciona a continuación. En ausencia de estos datos, el diagnóstico remoto resulta prácticamente inviable.

> REPORTE DE INCIDENCIA - INSTALACION FACEATTEND EDU
>
> 1. Datos del entorno
>
> Sistema operativo y version : ______________________________
>
> Memoria del equipo : ______________________________
>
> Version de la plataforma : (salida de docker --version)
>
> Version del orquestador : (salida de docker compose version)
>
> 2. Punto del procedimiento donde ocurrio
>
> Apartado del manual : ______________________________
>
> Comando ejecutado : ______________________________
>
> 3. Comportamiento observado
>
> Resultado esperado : ______________________________
>
> Resultado obtenido : ______________________________
>
> Mensaje de error completo : ______________________________
>
> 4. Evidencia adjunta
>
> ( ) Salida de docker compose ps
>
> ( ) Salida de docker compose logs del servicio afectado
>
> ( ) Captura de pantalla del error
>
> ( ) Contenido del archivo de configuracion, sin las contrasenas
>
> 5. Acciones previamente intentadas
>
> ____________________________________________________________

# Apéndice E

Resumen del Procedimiento de Instalación

El presente apéndice reúne la secuencia completa para quien ya conoce el procedimiento y únicamente requiere la sucesión de comandos.

> # 1. Obtener el proyecto
>
> git clone https://github.com/Jonas7891/ProyectoFaceAttendEDU.git
>
> # 2. Entrar a la carpeta que contiene el codigo
>
> cd ProyectoFaceAttendEDU/FULL
>
> # 3. Crear el archivo de configuracion
>
> cp .env.example .env
>
> # 4. Desplegar el sistema (de 15 a 40 minutos la primera vez)
>
> docker compose up -d --build
>
> # 5. Verificar el estado de los contenedores
>
> docker compose ps
>
> # 6. Abrir la interfaz web
>
> # http://localhost:8090
>
> # Usuario: admin.faceattend Contrasena: Admin123!ChangeMe

**Verificación.** El procedimiento se considera concluido únicamente cuando se superan las cinco pruebas del apartado Verificación de la Instalación.
