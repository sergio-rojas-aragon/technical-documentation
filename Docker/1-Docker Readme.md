---
title: Basic
layout: home
parent: Docker
nav_order: 1
---

# Get Started Docker
{: .no_toc }

{: .fs-6 .fw-70 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Container

* **mostrar todos los contenedores**

    ```yml
    docker container ls -a
    ```
* detener y eliminar los contenedores de forma forzada

    ```yml
    docker container rm -f <container-id> o <ID1 ID2...> 
    ```

## Volumes

Hay 3 tipos de volúmenes, son usados para hacer persistente la data entre reinicios y levantamientos de imágenes.

* **Named Volumes**
* **Anonymous Volumes**. Volúmenes donde sólo se especiﬁca el path del contenedor y Docker lo asigna automáticamente en el host
* **Bind Volumes**. pwd es el path en donde se encuentra la terminal, y en lazara en la app.
  
    ```powershell
    -v "$(pwd):/app"
    ```

* List volumes
  
    ```powershell
    docker volume ls
    ```

* Create volume
  
    ```powershell
    docker volume create todo-db
    ```

* Delete volumes if its are not used
  
    ```powershell
    docker volume prune
    ```
* Delete specific by volumes name
  
    ```powershell
    docker volume prune
    ```

