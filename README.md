# DOSW_ParcialT2_CristianMoreno

Prerrequisitos del parcial práctico DOSW - Segundo tercio.

## Índice

1. [Datos del estudiante](#1-datos-del-estudiante)
2. [Descripción del proyecto](#2-descripción-del-proyecto)
3. [Despliegue](#3-despliegue)
4. [Estructura del proyecto](#4-estructura-del-proyecto)
5. [Tecnologías y dependencias](#5-tecnologías-y-dependencias)
6. [Configuración y variables de entorno](#6-configuración-y-variables-de-entorno)
7. [Cómo ejecutar](#7-cómo-ejecutar)
8. [Pruebas](#8-pruebas)
9. [Docker](#9-docker)
10. [CI/CD](#10-cicd)
11. [Herramientas de modelado y diseño](#11-herramientas-de-modelado-y-diseño)

---

## 1. Datos del estudiante

- **Nombre completo:** [Escribir nombre completo]
- **Grupo:** [Escribir grupo]
- **Profesor agregado al repositorio:** Pendiente de confirmar.

## 2. Descripción del proyecto

DOSW-ParcialT2 es un servicio backend desarrollado con Java y Spring Boot para el parcial práctico de Desarrollo y Operaciones de Software.

El proyecto utiliza una arquitectura MVC organizada en capas. La base de datos es MongoDB y el servicio cuenta con un endpoint de salud y una configuración inicial de seguridad.

## 3. Despliegue

- **URL del servicio en Azure:** Pendiente.
- **URL del endpoint de salud:** Pendiente.
- **URL de Swagger:** Pendiente.

Endpoint de salud local: http://localhost:8080/api/v1/health

Swagger local: http://localhost:8080/swagger-ui/index.html

## 4. Estructura del proyecto

El proyecto sigue una arquitectura MVC organizada en capas:

- `controller/`: controladores REST, DTOs, mappers de entrada y handlers de excepciones.
- `core/`: modelos de dominio, servicios, validadores y utilidades.
- `persistence/`: entidades de MongoDB, repositorios y mappers de persistencia.
- `config/`: configuraciones transversales, incluida la seguridad.
- `main/`: clase principal de Spring Boot.
- `src/test/`: pruebas automatizadas.
- `docs/uml/`: archivos fuente de diagramas.
- `docs/images/`: capturas y evidencias.

La clase principal es `ParcialApplication`, ubicada en `edu.dosw.parcial.main`.

Las clases de persistencia de MongoDB utilizarán la anotación `@Document`.

## 5. Tecnologías y dependencias

| Tecnología | Versión | Uso |
|---|---|---|
| Java | 21 | Lenguaje de programación |
| Maven | 3.9.16 (local) | Construcción y dependencias |
| Spring Boot | 3.5.16 | Framework de la aplicación |
| Spring Web | Gestionada por Spring Boot | API REST |
| Spring Data MongoDB | Gestionada por Spring Boot | Persistencia NoSQL |
| Spring Security | Gestionada por Spring Boot | Seguridad |
| JJWT | 0.12.3 | JSON Web Tokens |
| Springdoc OpenAPI | 2.9.1 | Documentación Swagger |
| Lombok | Gestionada por Spring Boot | Reducción de código repetitivo |
| MapStruct | 1.5.5.Final | Mapeo de objetos |
| JUnit y Mockito | Spring Boot Starter Test | Pruebas automatizadas |
| JaCoCo | 0.8.12 | Cobertura de pruebas |
| Sonar Maven Plugin | 3.10.0.2594 | Análisis de calidad |
| MongoDB | 7 | Base de datos |
| Docker | 29.8.0 (local) | Contenedorización |
| Git y GitHub | Configuración local | Control de versiones |

## 6. Configuración y variables de entorno

| Variable | Descripción | Ejemplo |
|---|---|---|
| `MONGODB_URI` | Dirección de conexión a MongoDB | `mongodb://localhost:27017/dosw_parcial` |
| `JWT_SECRET` | Secreto Base64 para JWT | Generado con `openssl rand -base64 32` |
| `PORT` | Puerto de la aplicación | `8080` |
| `WEBSITES_PORT` | Puerto del servicio en Azure | `8080` |

Los secretos y las credenciales reales no deben guardarse en el repositorio. Se configurarán mediante variables de entorno y secretos de GitHub cuando corresponda.

Para ejecutar la aplicación dentro de Docker en el entorno local de Mac, la conexión a MongoDB utiliza `host.docker.internal`.

## 7. Cómo ejecutar

### Requisitos previos
- Java 21.
- Maven.
- Docker Desktop en ejecución.
- Git.

### Iniciar MongoDB
Si el contenedor no existe:

```bash
docker run -d -p 27017:27017 --name mongo-parcial mongo:7
```

Para comprobar MongoDB:

```bash
docker exec mongo-parcial mongosh --quiet --eval 'db.runCommand({ ping: 1 })'
```

### Ejecutar la aplicación con Maven

```bash
export JAVA_HOME="/opt/homebrew/opt/openjdk@21/libexec/openjdk.jdk/Contents/Home"
export PATH="$JAVA_HOME/bin:$PATH"
export JWT_SECRET="$(openssl rand -base64 32)"
export MONGODB_URI="mongodb://localhost:27017/dosw_parcial"
mvn spring-boot:run
```

Endpoint de salud: http://localhost:8080/api/v1/health

Swagger: http://localhost:8080/swagger-ui/index.html

## 8. Pruebas

Ejecutar las pruebas automatizadas:

```bash
mvn test
```

Verificar el proyecto:

```bash
mvn clean verify
```

Generar el reporte JaCoCo:

```bash
mvn jacoco:report
```

El reporte HTML se genera en `target/site/jacoco/index.html`.

Las pruebas específicas y sus evidencias están pendientes de completar.

## 9. Docker

Construir la imagen:

```bash
docker build -t dosw-parcial:local .
```

Ejecutar la aplicación en Docker:

```bash
docker run --rm --name dosw-parcial-app \
  -p 8080:8080 \
  -e JWT_SECRET="$(openssl rand -base64 32)" \
  -e MONGODB_URI="mongodb://host.docker.internal:27017/dosw_parcial" \
  dosw-parcial:local
```

Verificar el servicio desde otra terminal:

```bash
curl -i http://localhost:8080/api/v1/health
```

La imagen Docker se construyó correctamente y la aplicación arrancó conectándose a MongoDB durante las comprobaciones locales.

El `Dockerfile` utiliza una construcción multietapa. El archivo `.dockerignore` excluye archivos innecesarios del contexto de construcción.

## 10. CI/CD

El pipeline se configurará en `.github/workflows/ci-cd.yml` con GitHub Actions.

Las etapas previstas son:

1. Descargar el código fuente.
2. Configurar Java 21.
3. Compilar y ejecutar las pruebas.
4. Construir la imagen Docker.
5. Publicar la imagen en Docker Hub.
6. Desplegar la imagen en Azure App Service.

Secretos necesarios en GitHub:

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`
- `AZURE_CREDENTIALS`
- `AZURE_WEBAPP_NAME`
- `JWT_SECRET`

Los secretos deben configurarse en GitHub y no escribirse directamente en el código.

**Estado:** pendiente de configurar y verificar el pipeline y el despliegue en Azure.

## 11. Herramientas de modelado y diseño

Se seleccionará una herramienta de modelado entre Draw.io, Lucidchart o Miro, además de Figma para el diseño de interfaces.

Evidencias requeridas:

- Captura de acceso a la herramienta de modelado.
- Captura de acceso a Figma.
- Archivos fuente de diagramas en `docs/uml/`.
- Exportaciones y capturas en `docs/images/`.

**Estado:** pendiente de completar las cuentas, los diagramas y las evidencias.

---

## Estado general del proyecto

- [x] Repositorio creado y clonado.
- [x] Rama `develop` creada desde `main`.
- [x] Estructura inicial MVC preparada.
- [x] Maven configurado con Java 21.
- [x] MongoDB funcionando localmente.
- [x] Endpoint de salud verificado.
- [x] Swagger accesible localmente.
- [x] Imagen Docker construida y ejecutada.
- [ ] Pruebas específicas y evidencias de JaCoCo.
- [ ] Pipeline de GitHub Actions.
- [ ] Despliegue público en Azure.
- [ ] Profesor agregado al repositorio.
- [ ] Evidencias de modelado y Figma.
