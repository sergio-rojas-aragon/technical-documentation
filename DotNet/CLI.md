---
title: CLI
layout: home
parent: DotNet
nav_order: 1
---

Estos son comandos de CLI para .Net8.

No olvidar que para ver los archivos creados se puede utilizar cat nombre.extension

# CLI commands

### Create Solution

La nueva solucion obtiene el nombre de la carpeta:

```terminal
dotnet new sln
```

La nueva solucion obtiene el nombre entregado por el comando:

```terminal
dotnet new sln -n nombre
```

### agregar proyectos a la solucion

```terminal
dotnet sln add .\Proyecto\
```

--- 

## Proyectos

### Crear proyecto de libreria

```terminal
dotnet new classlib -n nombre
```

### Crear proyecto especificando que framework utilizar

```terminal
dotnet new webapi -n nombre -f net8.0
```

### Crear proyecto de consola

```terminal
dotnet new console -n nombre
```

### Crear proyecto de pruebas unitarias

```terminal
dotnet new xunit -n nombre
```

### referenciar proyectos

Dentro de la carpeta donde esta el proyecto:

```terminal
dotnet add reference ..\Proyecto2\
```

### ver proyectos referenciados

```terminal
dotnet list reference
```

### Listado de proyectos que se pueden crear


| Comando `dotnet new`                     | Qué crea / descripción                                                      |
| ---------------------------------------- | --------------------------------------------------------------------------- |
| `dotnet new console`                     | Crea una aplicación de consola                                              |
| `dotnet new classlib`                    | Crea una biblioteca de clases                                               |
| `dotnet new web`                         | Crea un proyecto ASP.NET Core vacío                                         |
| `dotnet new webapi`                      | Crea un proyecto API web (controladores)                                    |
| `dotnet new mvc`                         | Crea un proyecto Web con patrón MVC                                         |
| `dotnet new webapp` / `dotnet new razor` | Crea un proyecto Web con Razor Pages / Web App                              |
| `dotnet new grpc`                        | Crea un servicio gRPC                                                       |
| `dotnet new blazor`                      | Crea una aplicación Blazor (servidor / híbrido) ([Microsoft Learn][1])      |
| `dotnet new blazorwasm`                  | Crea una aplicación Blazor WebAssembly independiente ([Microsoft Learn][1]) |
| `dotnet new mstest`                      | Crea un proyecto de pruebas con MSTest                                      |
| `dotnet new xunit`                       | Crea un proyecto de pruebas con xUnit                                       |
| `dotnet new nunit`                       | Crea un proyecto de pruebas con NUnit                                       |
| `dotnet new sln`                         | Crea un archivo de solución vacío (.sln)                                    |
| `dotnet new globaljson`                  | Crea un archivo `global.json` para fijar la versión del SDK                 |
| `dotnet new editorconfig`                | Crea un archivo `.editorconfig` para estilos de código                      |
| `dotnet new buildprops`                  | Crea un archivo `Directory.Build.props`                                     |
| `dotnet new buildtargets`                | Crea un archivo `Directory.Build.targets`                                   |
| `dotnet new tool-manifest`               | Crea un manifiesto para herramientas locales (dotnet local tool manifest)   |

[1]: https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new-sdk-templates?utm_source=chatgpt.com ".NET default templates for dotnet new - .NET CLI | Microsoft Learn"


## Paquetes NuGet

### Agregar referencia a paquete de NuGet

en la carpeta del proyecto (no de la solucion), colocar:

```terminal
dotnet add package Microsoft.EntityFrameworkCore
```

### Agregar referencia a paquete de NuGet fuera de la carpeta del proyecto

```terminal
dotnet add FileReader.Tests package Moq
```

### mostrar el listado de package

```terminal
dotnet list package
```

---

## Ejecutar y Construir

### construyendo

```terminal
dotnet build
```
### Publicar (simple)

```terminal
dotnet publish -o C:\proyecto
```

### Ejecutar pruebas unitarias

dentro de la carpeta de las pruebas unitarias:

```terminal
dotnet test
```

## Entity FrameWorkCore

### Instalar

```terminal
dotnet add package Microsoft.EntityFrameworkCore
```

### activar migraciones

dotnet ef migrations add InitialCreate