---
title: Dockerfile
layout: home
parent: Docker
nav_order: 6
---

# Dockerfile
{: .no_toc }

Instrucciones para construir una imagen de Docker.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# Estructura

```
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

RUN echo "============== Copy Files =============="

COPY *.sln ./
COPY DemoCiCdApi/ ./DemoCiCdApi/
COPY DemoCiCdApi.Tests/ ./DemoCiCdApi.Tests/

RUN echo "============== Restore =============="

RUN dotnet restore

RUN echo "============== Publish =============="

WORKDIR /src/DemoCiCdApi
RUN dotnet publish -c Release -o /app/publish


# =============================
#   RUNTIME STAGE
# =============================

RUN echo "============== Runtime =============="

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app

COPY --from=build /app/publish .

EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080

ENTRYPOINT ["dotnet", "DemoCiCdApi.dll"]

```

# Instrucciones importantes

## From

Define la imagen base.

```
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
```

* **Qué hace (literal):** selecciona una imagen base (un sistema de archivos ya preparado) que trae el SDK de .NET 8.0 instalado.
* **Por qué está ahí:** necesitas el SDK para compilar y publicar el código (.NET SDK incluye `dotnet build`, `dotnet publish`, `dotnet restore`).
* **Nota práctica:** `AS build` le da un nombre a esta etapa (se usa luego con `--from=build`). Esto forma parte de un multi-stage build (varias etapas): se compila en una etapa y luego se copian sólo los artefactos necesarios a la etapa final.

* Tener en cuenta:
    * Usa bases mínimas cuando es posible
    * Prefiere imágenes oficiales slim/alpine
    * Evita Alpine para Python si dependes de librerías nativas (musl vs glibc)



```
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
```

Qué hace (literal): inicia una nueva etapa del build basada en la imagen aspnet:8.0. Esta imagen trae sólo el runtime de .NET (no el SDK), y es más ligera. El nombre final identifica la etapa.
Por qué: mejor separar compilación (SDK grande) y runtime (ligero) para que la imagen final sea pequeña, más segura y más rápida de transferir.

## WORKDIR

Cambia el directorio de trabajo dentro del contenedor.

```
WORKDIR /app
```

* **Qué hace:** cambia el directorio de trabajo dentro de la imagen a /src.
* **Por qué:** todas las instrucciones siguientes que usen rutas relativas se ejecutan dentro de `/src`. Evita usar rutas absolutos una y otra vez y hace el Dockerfile más legible.

## COPY

Copia archivos desde tu máquina al contenedor.

```
COPY *.sln ./
COPY DemoCiCdApi/ ./DemoCiCdApi/
COPY DemoCiCdApi.Tests/ ./DemoCiCdApi.Tests/
```

Que hace:

* copia desde el contexto de build (es decir, la carpeta en tu máquina donde ejecutas docker build) cualquier archivo que coincida con *.sln al directorio actual del contenedor (/src por el WORKDIR). .sln es el archivo de solución de .NET.
* se copian las carpetas del proyecto y de tests desde tu máquina al contenedor, preservando la estructura

***Por qué el orden importa:*** copiar primero lo que menos cambia (archivo .sln y archivos de dependencias) y luego el resto permite que Docker aproveche la cache en pasos anteriores (ver más abajo en caching). Si copiaras todo primero y después instalases dependencias, cada cambio en cualquier archivo invalidaría la cache de las instalaciones.

## RUN

Ejecuta comandos durante la construcción de la imagen.

```
RUN dotnet restore
```

* ejecuta dotnet restore dentro de /src. Esto descarga (o resuelve) todas las dependencias especificadas en tus archivos de proyecto (*.csproj) y crea una cache de paquetes NuGet.


## EXPOSE

Indica el puerto que usa la app (informativo).

```
EXPOSE 8080
```

Qué hace: documenta que la aplicación escucha en el puerto 8080.
Importante: EXPOSE no publica el puerto fuera del contenedor por sí mismo; sirve como documentación y para algunas herramientas. Para acceder desde tu máquina deberás mapear puertos en docker run con -p.

## ENV

Variables de entorno. Define una variable de entorno dentro del contenedor.

```yml
ENV ASPNETCORE_URLS=http://+:8080
```

Qué hace (literal): Aquí indica a ASP.NET Core que debe escuchar en cualquier interfaz (+) en el puerto 8080 por HTTP.

Por qué: por defecto ASP.NET Core puede escuchar en un puerto distinto o solo en localhost; con esto garantizas que dentro del contenedor la app estará accesible en http://0.0.0.0:8080.


## ENTRYPOINT

Establece el comando que se ejecutará cuando alguien arranque el contenedor. 

```yml
ENTRYPOINT ["dotnet", "DemoCiCdApi.dll"]
```
Qué hace (literal): En este caso ejecuta dotnet DemoCiCdApi.dll, que inicia la aplicación .NET.
Diferencia clave: ENTRYPOINT fija el principal proceso del contenedor (PID 1). Si usas docker run <imagen>, esto se ejecuta automáticamente. Puedes pasar argumentos adicionales al final de docker run que se unirán como parámetros si el ENTRYPOINT está en forma exec (array), como aquí.


## CMD y ARG


* **CMD**
    Define el comando que se ejecuta al arrancar el contenedor.
    ```yml
    CMD ["python", "app.py"]
    ```


* **ARG**  – Variables solo disponibles en build-time.

## Diferencias

**RUN vs ENTRYPOINT vs CMD**

* RUN → se ejecuta durante el build (por ejemplo dotnet restore, dotnet publish), modifica la imagen.
* ENTRYPOINT → comando fijo que se ejecuta cuando se arranca el contenedor.
* CMD → argumentos por defecto que pueden ser sobreescritos cuando inicias el contenedor. (En este Dockerfile solo se usa ENTRYPOINT.)

# Comandos relacionados

## Pilares para entender

1. **Construir imagen** → docker build
1. **Autenticarse en un registry** → docker login
1. **Subir la imagen** → docker push
1. **Ejecutar o actualizar contenedor** → docker run o docker-compose up

## Explicacion de Comandos

* **Construir una imagen:**
    Crea la imagen con el nombre democicdapi con la version latest.
    el punto significa que tiene que construir utilizando el dockerfile que se encuentra en este directorio.

    ```
    docker build -t democicdapi:latest .
    ```

    es importante colocarle un tag para saber que version estas mandando.


* **Ejecutar un contenedor:**
    ```
    docker run -p 3000:3000 miapp
    ```

* **Listar imágenes:**
    ```
    docker images
    ```

## Registry

Es un servidor donde se guardan imágenes Docker.

Tipos:

* Docker Hub (el más conocido)
* Azure Container Registry
* AWS ECR
* GitHub Container Registry
* Harbor
* GitLab Registry
* etc.

¿Por qué se necesita?

Porque en un pipeline de deploy:

✔ Jenkins construye la imagen
✔ La etiqueta
✔ La sube al registry
✔ Y el servidor donde se deploya la descarga desde ahí

El registry es para Docker lo que un repositorio Git es para código.


### Autenticarse en el registry (login)

Antes de subir una imagen necesitas loguearte:

```
docker login -u usuario -p contraseña registry-url
```

Ejemplos:

```
docker login -u user -p mypass docker.io
docker login -u $CI_USER -p $CI_TOKEN ghcr.io
docker login -u AWS -p $(aws ecr get-login-password) xxxxx.dkr.ecr.us-east-1.amazonaws.com
```

En Jenkins los credenciales se guardan siempre en Credentials y se leen desde environment o con `withCredentials`.

### Registry en servidor local

El registry local funciona como un servidor HTTP que guarda imágenes en formato Docker.
Docker ya trae una imagen oficial llamada registry.

```terminal
docker run -d -p 5000:5000 --name registry registry:2
```

Esto crea:

* un contenedor llamado registry
* escuchando en http://localhost:5000
* que funciona como servidor de imágenes

## Subir imagen al registry

```terminal
docker push nombre_imagen:tag
```

***Para poder subirla, el nombre debe incluir el nombre del registry.***

**Ejemplo para dockerhub:**

```terminal
docker build -t usuario/miapp:1.0 .
docker push usuario/miapp:1.0
```

**ejemplo para registry privado:**

```terminal
docker build -t my-registry.com/miempresa/miapp:1.0 .
docker push my-registry.com/miempresa/miapp:1.0
```

**Ejemplo para un registry local:**

primero se etiqueta la imagen:

```terminal
docker tag miapp:1.0 localhost:5000/miapp:1.0
```

Luego push:

```terminal
docker push localhost:5000/miapp:1.0
```

### Consumir imagen desde registry local

Desde un local:

```terminal
docker pull localhost:5000/miapp:1.0
```

y luego:

```terminal
docker run -d --name api -p 8080:8080 localhost:5000/miapp:1.0
```

### Ejecutar la imagen en un servidor

```terminal
docker run -d --name api -p 8080:8080 miapp:1.0
```
