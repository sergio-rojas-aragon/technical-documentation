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

```
ENTRYPOINT ["dotnet", "DemoCiCdApi.dll"]
```
Qué hace (literal): En este caso ejecuta dotnet DemoCiCdApi.dll, que inicia la aplicación .NET.
Diferencia clave: ENTRYPOINT fija el principal proceso del contenedor (PID 1). Si usas docker run <imagen>, esto se ejecuta automáticamente. Puedes pasar argumentos adicionales al final de docker run que se unirán como parámetros si el ENTRYPOINT está en forma exec (array), como aquí.


## CMD y ARG


* **CMD**
    Define el comando que se ejecuta al arrancar el contenedor.
    ```
    CMD ["python", "app.py"]
    ```


* **ARG**  – Variables solo disponibles en build-time.

## Diferencias

**RUN vs ENTRYPOINT vs CMD**

* RUN → se ejecuta durante el build (por ejemplo dotnet restore, dotnet publish), modifica la imagen.
* ENTRYPOINT → comando fijo que se ejecuta cuando se arranca el contenedor.
* CMD → argumentos por defecto que pueden ser sobreescritos cuando inicias el contenedor. (En este Dockerfile solo se usa ENTRYPOINT.)

# Comandos

* **Construir una imagen:**
    ```
    docker build -t miapp .
    ```
* **Ejecutar un contenedor:**
    ```
    docker run -p 3000:3000 miapp
    ```

* **Listar imágenes:**
    ```
    docker images
    ```