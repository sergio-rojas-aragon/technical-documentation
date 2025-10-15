---
title: Angular Basic
layout: home
parent: Angular
nav_order: 1
---


### instalar angular

con permisos de administrador y global

```terminal
npm install -g @angular/cli@18.2.21
```

### instalar angular sin instalarlo globalmente

```terminal
npx -p @angular/cli@18.2.21 ng new nombre-del-proyecto
```

### ver version instalada

```terminal
ng version
```

### crear proyecto nuevo

```terminal
ng new nombre-del-proyecto
```

### crear proyecto dentro de la carpeta

Asegurarse que el nombre de la carpeta no tenga punto.

```terminal
ng new . --directory . --routing --style=scss
```

### iniciar el servidor

```terminal
ng serve --open
```


ng generate service services/tareas