---
title: Get Started - Basico
layout: home
parent: Git
nav_order: 1
---

# Apuntes Git

Aca dejare todos los comandos GIT para evitar buscarlos en google.


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Get Started

### Configuracion de GIT:

```terminal
git config --global user.name "cambiar"
git config --global user.email "cambiar@gmail.com"
```

### Crear Alias

```
git config --global alias.lg "log --oneline --decorate --all --graph"
```
```terminal
git config --global alias.s "status -s -b"
```

###  Ver configuracion creada:

```terminal
git config --global -e
```

### Si se cambia de pasword usar lo siguiente

```terminal
git config --global --unset user.password
```

despues pedira las credenciales

## Alias

Alias son atajos que se pueden crear en el sistema git en forma local, para eso se usa git config. Ejemplos:

### Crear alias:

```terminal
git config --global alias.lg "log --oneline --decorate --all --graph"
```

### Como se usa:

```terminal
git lg
```

### Crear alias:

git config --global alias.s "status -s -b"

### Como se usa:

```terminal
git s 
```


## Stage commands

- Quitar archivos del stage

    ```terminal
    git reset *.xml
    git reset nombrearchivo
    ```

