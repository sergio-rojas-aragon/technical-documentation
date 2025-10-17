---
title: Angular Basic
layout: home
parent: Angular
nav_order: 1
---

## Instalacion

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

o tambien:

```terminal
ng v
```

## Inicio

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

## Desarrollo

### Crear Servicio

```terminal
ng generate service services/tareas
```

### crear componente standalone (angular 18)

```terminal
ng generate component componentes/lista-productos --standalone
```

### crear componente dashboard prefabricado

```terminal
ng generate @angular/material:dashboard <component-name>
```

## Sesiones

### Crear guard

```terminal
ng generate guard guards/auth.guard
```

preguntara que tipo de guard crear.

