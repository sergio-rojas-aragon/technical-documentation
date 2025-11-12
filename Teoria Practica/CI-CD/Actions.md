---
title: Github Actions
layout: home
parent: CI/CD
nav_order: 2
---

# Github Actions
{: .no_toc }

GitHub Actions usa YAML para definir pipelines o workflows.
Cada workflow describe cuándo y cómo ejecutar automatizaciones.
El archivo se guarda en la ruta:

```
.github/workflows/ci.yml
```

1. TOC
{:toc}


## Estructura


```
name: CI - Build and Test        # 👈 Nombre del workflow (visible en GitHub)

on:                              # 👈 Cuándo se ejecuta
  push:
    branches: [ "main" ]         #    - Cuando hay un push en main
  pull_request:
    branches: [ "main" ]         #    - O un PR contra main

jobs:                            # 👈 Conjunto de trabajos (pipelines paralelos o secuenciales)
  build:                         #    - Nombre del job
    runs-on: ubuntu-latest       # 👈 En qué sistema operativo se ejecuta (máquina virtual)

    steps:                       # 👈 Pasos que ejecutará el job
      - name: Checkout código
        uses: actions/checkout@v4   # 👈 Acción predefinida que clona el repo

      - name: Configurar .NET
        uses: actions/setup-dotnet@v4
        with:
          dotnet-version: '8.0.x'   # 👈 Indica qué versión de .NET instalar

      - name: Restaurar dependencias
        run: dotnet restore         # 👈 Ejecuta un comando en la terminal de esa VM

      - name: Compilar
        run: dotnet build --no-restore --configuration Release

      - name: Ejecutar pruebas unitarias
        run: dotnet test --no-build --verbosity normal

```

| Sección       | Explicación                                                                                                                   | Ejemplo                                           |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| **`name`**    | Nombre descriptivo del workflow. Aparece en la pestaña **Actions**.                                                           | `"CI - Build and Test"`                           |
| **`on`**      | Define los **eventos que disparan el workflow** (pueden ser `push`, `pull_request`, `schedule`, etc.).                        | `on: push` ejecuta al subir código.               |
| **`jobs`**    | Un *job* es un conjunto de pasos ejecutados en un mismo entorno. Puedes tener varios jobs (por ejemplo: build, test, deploy). | `jobs: build:`                                    |
| **`runs-on`** | Indica el **sistema operativo** del runner (máquina virtual).                                                                 | `ubuntu-latest`, `windows-latest`, `macos-latest` |
| **`steps`**   | Cada *step* es una acción o comando ejecutado dentro del job.                                                                 | `run: dotnet test`                                |
| **`uses`**    | Indica que el step utiliza una **acción predefinida** de GitHub o de la comunidad.                                            | `uses: actions/checkout@v4`                       |
| **`run`**     | Ejecuta comandos directamente en la consola del runner.                                                                       | `run: dotnet build`                               |
| **`with`**    | Pasa parámetros a la acción que estás usando.                                                                                 | `with: dotnet-version: '8.0.x'`                   |
