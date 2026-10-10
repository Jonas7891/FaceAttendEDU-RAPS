---
title: "Documentación de Implementación del Software"
authors: Jonattan Steven Rizo Solano, Diego Andres Gutierrez Nuñez, Juan Davida Arboleda Perdomo
abstract: |
  El presente documento describe el procedimiento técnico para la implementación, despliegue y puesta en marcha del sistema FaceAttendEDU. Se detalla la estrategia de despliegue basada en la contenerización mediante Docker y la orquestación con Docker Compose, asegurando la correcta configuración de la infraestructura de datos (PostgreSQL, MongoDB, Redis y Apache Kafka) y el despliegue de los microservicios políglotas. El documento provee una guía paso a paso que abarca desde los requisitos mínimos de hardware y software hasta el protocolo de verificación final, garantizando que el entorno operativo sea consistente, seguro y trazable.
keywords:
  - implementación de software
  - despliegue de microservicios
  - Docker
  - Docker Compose
  - infraestructura de datos
  - configuración de entorno
---

# Introducción

La implementación del software es la fase donde la arquitectura diseñada y el código desarrollado se transforman en un sistema operativo y accesible para los usuarios. Para FaceAttendEDU, dada la complejidad de contar con nueve microservicios implementados en cuatro lenguajes diferentes (Java, TypeScript, Python y Go) y múltiples motores de persistencia, se ha optado por una estrategia de despliegue basada en contenedores.

Este enfoque elimina la necesidad de instalar manualmente cada entorno de ejecución en la máquina host, encapsulando todas las dependencias dentro de imágenes estandarizadas. Esto asegura que el sistema se comporte de la misma manera en el entorno de desarrollo, pruebas y producción.

## Objetivo del documento
Establecer el procedimiento formal y detallado para la instalación, configuración y despliegue del sistema FaceAttendEDU, permitiendo que cualquier técnico calificado pueda poner en marcha la plataforma sin errores y de manera reproducible.

## Alcance del documento
El documento cubre la preparación del entorno, la instalación de herramientas base, la configuración de variables de entorno, el despliegue de la infraestructura de datos, la orquestación de los microservicios y el protocolo de verificación de salud del sistema. No incluye la configuración de redes externas (DNS, Firewall corporativo) ni la gestión de alta disponibilidad en la nube.

# Estrategia de Implementación

## Modelo de Despliegue: Contenerización
El sistema utiliza **Docker** como plataforma de contenedores y **Docker Compose** como orquestador. Esta elección permite gestionar la complejidad de los 18 contenedores necesarios (servicios, bases de datos, gateway y tareas de migración) mediante un único archivo de configuración.

### Organización de Redes
Para garantizar la seguridad, la implementación divide el tráfico en tres redes aisladas:
1. **Edge Network**: Conecta la interfaz web y el Gateway (Kong).
2. **App Network**: Conecta el Gateway con los microservicios.
3. **Data Network**: Conecta los microservicios con las bases de datos y el broker de mensajería.

Esta arquitectura impide que la capa de presentación tenga acceso directo a la capa de datos, mitigando riesgos de ataques directos a la base de datos.

# Requisitos de Implementación

## Requisitos de Hardware (Mínimos)
| Recurso | Requisito | Justificación |
|---|---|---|
| RAM | 8 GB (16 GB Rec.) | Ejecución simultánea de múltiples JVM y motores de datos. |
| CPU | 4 Cores (x86_64/ARM) | Procesamiento de biometría y orquestación de contenedores. |
| Disco | 20 GB libres | Almacenamiento de imágenes de Docker y volúmenes de datos. |
| Red | Acceso a Internet | Descarga de imágenes base y dependencias durante el build. |

## Requisitos de Software
- **Sistema Operativo**: Windows 10/11 (con WSL2), Ubuntu 22.04 LTS o macOS Ventura+.
- **Docker Desktop / Engine**: Versión 24.0 o superior.
- **Docker Compose**: Versión 2.20 o superior.
- **Git**: Versión 2.40 o superior para la obtención del código fuente.

# Procedimiento de Instalación

## 1. Preparación del Entorno
Antes de iniciar, el instalador debe verificar la disponibilidad de los puertos críticos (ej. 8080 para el Gateway, 5432 para PostgreSQL). En caso de conflicto, se deben detener los servicios locales o modificar el archivo de configuración.

## 2. Obtención del Proyecto
El código fuente se obtiene mediante el clonado del repositorio oficial:
```bash
git clone https://github.com/Jonas7891/ProyectoFaceAttendEDU.git
cd ProyectoFaceAttendEDU/FULL
```

## 3. Configuración del Sistema
La configuración se centraliza en un archivo `.env` en la raíz del proyecto.
1. Copiar el archivo ` .env.example` a `.env`.
2. Ajustar las variables según el entorno (especialmente `BIND_IP` y `EXPO_PUBLIC_API_URL` si se accede desde dispositivos móviles).

## 4. Despliegue y Orquestación
El despliegue se ejecuta con el siguiente comando, que construye las imágenes y levanta los contenedores en modo segundo plano:
```bash
docker compose up -d --build
```

### Orden de Arranque y Migraciones
El sistema sigue una cadena de dependencias estricta:
1. **Capa de Datos**: Se inicia PostgreSQL, MongoDB, Redis y Kafka.
2. **Migraciones**: Se ejecutan ocho tareas secuenciales de **Liquibase** que crean los esquemas y cargan los datos maestros.
3. **Microservicios**: Una vez que las migraciones finalizan, se inician los servicios de negocio.
4. **Gateway e Interfaz**: Finalmente, se levanta el Gateway (Kong) y la interfaz web (nginx).

# Verificación de la Implementación

Para declarar la instalación como exitosa, se debe ejecutar el siguiente protocolo de pruebas:

| Prueba | Método de Verificación | Resultado Esperado |
|---|---|---|
| **Estado de Contenedores** | `docker compose ps` | 18 contenedores operativos (Running/Healthy). |
| **Salud de Servicios** | `curl http://localhost:8080/health/...` | Respuesta JSON con estado "UP". |
| **Conectividad Gateway** | `curl http://localhost:8080` | Respuesta del Gateway de Kong. |
| **Autenticación** | Login con `admin.faceattend` | Retorno de un token de sesión válido. |
| **Carga de Interfaz** | Acceso a `http://localhost:8090` | Carga correcta de la pantalla de login. |
| **Aislamiento de Red** | `docker exec ... nslookup postgres` | Fallo de resolución desde el frontend. |

# Operación y Mantenimiento

## Comandos Frecuentes
- **Reiniciar sistema**: `docker compose restart`
- **Ver logs en tiempo real**: `docker compose logs -f <servicio>`
- **Actualizar versión**: `git pull` $\rightarrow$ `docker compose up -d --build`
- **Limpieza total (incluye datos)**: `docker compose down -v`

## Solución de Problemas Comunes
- **Puerto Ocupado**: Identificar el proceso con `netstat -ano` y finalizarlo o cambiar el puerto en el `.env`.
- **Error de Migración**: Revisar los logs de `identity-liquibase` para detectar fallos de sintaxis o conectividad.
- **Interfaz no conecta**: Verificar que `EXPO_PUBLIC_API_URL` coincida con la IP del host y que el frontend haya sido reconstruido (`--build`).

# Conclusiones

El procedimiento de implementación de FaceAttendEDU minimiza la intervención manual mediante la automatización con Docker. La separación de redes y la orquestación dependiente aseguran que el sistema se despliegue de forma consistente, reduciendo drásticamente el tiempo de puesta en marcha y eliminando la problemática de "funciona en mi máquina".

# Referencias

American Psychological Association. (2020). *Publication manual of the American Psychological Association* (7th ed.). https://doi.org/10.1037/0000165-000

Docker Inc. (n.d.). *Docker documentation*. https://docs.docker.com/

Kong Inc. (n.d.). *Kong Gateway documentation*. https://docs.konghq.com/gateway/

Liquibase. (n.d.). *Liquibase documentation*. https://docs.liquibase.com/
