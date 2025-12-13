---
title: Terminal
layout: home
parent: Docker
nav_order: 4
---

# Terminal
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

--- 

# Use Terminal in docker container

* Entrar en una terminal de un contenedor.

    ```powershell
    docker exec -it conteinerId folder
    ```

-it: interactive terminal

limpiar consola: ctrl + L

modificar archivo:

commando `cat archivo` sirve para ver el archivo.

comando `vi nombreArchivo` para ver archivo y ademas sirve para editar:

* para editar presionar  `i`
* para salir colocar `ESC` y agregar `:wq!` y enter.

para salir escribir `exit`


docker exec -it PostgresContenedor psql -U admin -d next