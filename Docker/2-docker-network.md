---
title: Networks
layout: home
parent: Docker
nav_order: 2
---

# Networks
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

--- 

Si dos o más contenedores están en la misma red, podrán hablar entre sí. Si no lo están, no podrán.

https://docs.docker.com/engine/network/tutorials/standalone/

* Listar redes: 
    ```powershell
    docker network ls
    ```
* Crear red:
    ```powershell
    docker network create nombre-red
    ```

* Eliminar redes:
    ```powershell
    docker network prune
    ```
* Agregar un contenedor a una red:
    ```powershell
    docker network connect nombre-red nombre-contenedor
    ```

* inspect. Show list of containers in the selected network. In Json name `Containers`
    ```bash
    docker network inspect <NAME o ID>
    ```

## Conectar dos contenedores

first, create a new network and put the name.

In te yml file write this:

    networks:
      - net-postgres

networks:
    net-postgres:
        external: true

