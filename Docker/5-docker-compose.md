---
title: Docker Compose
layout: home
parent: Docker
nav_order: 5
---

# Docker Compose
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

--- 


### para ejecutar 

```terminal
docker compose up
```

### elimina volumes y todo lo relacionado

```terminal
docker compose down
```

### indicar donde guardar los datos en el sistema del PC

se tiene que comentar el volume external true. Se puede hacer para base de datos y aplicaciones.

```
volumes:
	- ./posrgres: #ruta de archivos del contenedor
```

### para ejecutar algun comando desde el docker compose. dentro del servicio.

```
command: ['--auth']
```

### usar variables de entorno en docker compose. crear el archivo env.

```
container_name: ${ NOMBRE_VARIABLE }
```

y en el archivo env se coloca:

```terminal
NOMBRE_VARIABLE=asdasdasd
```
