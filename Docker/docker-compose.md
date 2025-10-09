---
title: Docker Compose
layout: home
parent: Docker
nav_order: 4
---

# Docker Compose


### para ejecutar yml

```terminal
docker compose up
```

### elimina volumes y todo lo relacionado

```terminal
docker compose down
```

### indicar donde guardar los datos en el sistema del PC

se tiene que comentar el volume external true. Se puede hacer para base de datos y aplicaciones.

```yml
volumes:
	- ./posrgres: #ruta de archivos del contenedor
```

### para ejecutar algun comando desde el docker compose. dentro del servicio.

```yml
command: ['--auth']
```

### usar variables de entorno en docker compose. crear el archivo env.

```yml
container_name: ${ NOMBRE_VARIABLE }
```

y en el archivo env se coloca:

```terminal
NOMBRE_VARIABLE=asdasdasd
```
