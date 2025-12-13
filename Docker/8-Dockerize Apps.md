---
title: Dockerize Apps
layout: home
parent: Docker
nav_order: 7
---

# Dockerize Apps
{: .no_toc }


{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# .Net8

Estructura recomendada del proyecto:

```
root/
│
├── src/
│   └── MiApi/
│       ├── MiApi.csproj
│       ├── Program.cs
│       └── ...
│
├── Dockerfile
├── .dockerignore
└── README.md
```

> El Dockerfile va en la raíz, no dentro de src.

## Crear `.dockerignore`

Evita copiar archivos innecesarios al contenedor (mejora performance y seguridad).

`.dockerignore`

```
**/bin/
**/obj/
**/.vs/
**/.vscode/
.git
.gitignore
Dockerfile
README.md
```

## Dockerfile

```yml
# =========================
# Build stage
# =========================
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app

# Copiar solo el csproj y restaurar dependencias
COPY src/MiApi/*.csproj ./MiApi/
RUN dotnet restore ./MiApi/MiApi.csproj

# Copiar el resto del código
COPY src/ ./src/

# Publicar la app
RUN dotnet publish src/MiApi/MiApi.csproj \
    -c Release \
    -o /app/publish \
    /p:UseAppHost=false

# =========================
# Runtime stage
# =========================
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app

# Seguridad: crear usuario no root
RUN adduser --disabled-password --home /app appuser
USER appuser

COPY --from=build /app/publish .

ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

ENTRYPOINT ["dotnet", "MiApi.dll"]
```

## Build de la imagen Docker

```terminal
docker build -t miapi:latest .
```

verificar si se creo:

```terminal
docker images
```

## ejecutar el contenedor en docker desktop

para ambiente de desarrollo

```terminal
docker run -d `
  -p 5000:8080 `
  -e ASPNETCORE_ENVIRONMENT=Development `
  --name user-identity `
  user-identity:latest
```

verificar que este corriendo:

```terminal
docker ps
```

ver logs:

```terminal
docker logs miapi-container
```
