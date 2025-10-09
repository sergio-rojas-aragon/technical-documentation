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

---
## Paquetes NuGet

### Agregar referencia a paquete de NuGet

en la carpeta del proyecto (no de la solucion), colocar:

```terminal
dotnet add package Microsoft.EntityFrameworkCore
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