---
title: Git
layout: home
nav_order: 2
---

# Apuntes GIT

Aca dejare todos los comandos GIT para evitar buscarlos en google.

## Menu

1. [troubleshooting](<git troubleshooting.md>)

## Tabla de Contenidos

- [Apuntes GIT](#apuntes-git)
  - [Menu](#menu)
  - [Tabla de Contenidos](#tabla-de-contenidos)
  - [Get Started](#get-started)
  - [Alias](#alias)
  - [Branch commands](#branch-commands)
  - [Stage commands](#stage-commands)
  - [Commit commands](#commit-commands)
  - [Tags commands](#tags-commands)
  - [Procedimientos](#procedimientos)
    - [Cambiar Commit anterior](#cambiar-commit-anterior)
    - [Unir ramas](#unir-ramas)
    - [Unir ramas con conflicto](#unir-ramas-con-conflicto)
    - [Recorrer ramas](#recorrer-ramas)
    - [Eliminar Commits](#eliminar-commits)
    - [Descartar cambios](#descartar-cambios)

## Get Started

- Configuracion de GIT:

```powershell
git config --global user.name "cambiar"
git config --global user.email "cambiar@gmail.com"
```

- Crear Alias

```powershell
git config --global alias.lg "log --oneline --decorate --all --graph"
git config --global alias.s "status -s -b"
```

- Ver configuracion creada:

```powershell
git config --global -e
```

- Si se cambia de pasword usar lo siguiente

```powershell
git config --global --unset user.password
```

despues pedira las credenciales

## Alias

Alias son atajos que se pueden crear en el sistema git en forma local, para eso se usa git config. Ejemplos:

- Crear alias:

```powershell
git config --global alias.lg "log --oneline --decorate --all --graph"
```

- Como se usa:

```powershell
git lg
```

- Crear alias:

git config --global alias.s "status -s -b"

- Como se usa:

```powershell
git s 
```

## Branch commands

- Ver branches

    ```powershell
    git branch
    ```

- Se cambia de rama a la que se esta mencionando

    ```powershell
    git checkout rama-villanos
    ```

- Crear rama en repositorio remoto.

```powershell
git push origin rama-capitan-loco
```

- Crear una rama nueva:
  
    ```powershell
    git branch rama-villanos
    ```

- Crea rama y se posiciona en ella
  
    ```powershell
    git checkout -b <branch_name>
    ```

- se cambia de rama a la que se esta mencionando

    ```powershell
    git checkout rama-villanos
    ```

- Eliminar una rama. es una buena practica eliminar la rama una vez que se termina de usar.

    ```powershell
    git branch -d rama-villanos
    ```

- Eliminar una rama del remoto

    ```powershell
    git push <remote_name> --delete <branch_name>
    ```

- Saber diferencia entre dos ramas:

git diff rama-villanos master

- Merge con mensaje

    ```powershell
    git merge --no-f rama -m "mensaje"
    ```

## Stage commands

- Quitar archivos del stage

    ```powershell
    git reset *.xml
    git reset nombrearchivo
    ```

## Commit commands

- Modifica el commit anterior (amend significa enmendar)

```powershell
git commit --amend -m "Actualizacion de mensaje"
```

## Tags commands

- ver tags

    ```powershell
    git tag
    ```

- borrar tags

    ```powershell
    git tag -d superRelease
    ```

- agregar tag con anotaciones

    ```powershell
    git tag -a v1.0.0 -m "Version 1.0.0"
    ```

- agregar tag con anotaciones a otro punto de la rama

    ```powershell
    git tag -a v0.1.0 345d7de -m "Version alfa"
    ```

- para ver lo que contiene cierto tag

    ```powershell
    git show v0.1.0
    ```

- Enviar Tags al servidor

    ```powershell
    git push origin --tags
    ```

## Procedimientos

### Cambiar Commit anterior

- Se realizan las modificaciones
- se agregan al stage (git add .)
- Se agrega al commit anterior, fijarse que el mensaje sea el mismo que el commit anterior:

```powershell
git commit --amend -m "mensaje commit anterior"
```

Otra forma (Con ejemplo):

- se agrega la modificacion al stage:
  
```powershell
git add modal.js
```

- se agrega al commit anterior, no edit sirve para no modificar el texto del commit anterior: 

```powershell
git commit --ammend --no-edit
```

ojo, es posible que el push no funcione, se tiene que hacer un:

```powershell
git push --force origin master
```

posible problema por branch protegida, para esto se tiene que ir settings-reposotory-branch protected branch

### Unir ramas

para unir ramas se tiene que posicionarse e la rama en donde se quiere fucionar algo:

```powershell
git checkout master
```

head = apunta al ultimo commit a la rama de al cual estamos

```powershell
git merge rama-villanos
```

merge ramas con commit de mensaje

```powershell
git merge --no-f rama -m "mensaje"
```

### Unir ramas con conflicto

Cuando se van a unir ramas es probable que salga un mensaje:

```powershell
CONFLICT CONTENT: Merge conflict in archivo.txt
automatic merge failed.
```

Dentro del head y el == es el cambio de la rama actual. Dentro del == y el nombre de la rama es el cambio que esta en la rama.

```powershell
<<<<<< HEAD
hola
======
chao
>>>>>> rama
```

Simplemente se deja el texto que se quiere dejar y el resto se borra

```powershell
chao
```

---

### Recorrer ramas

- volver a un punto especifico

```powershell
git reset --soft d70db2a
```

- volver al punto anterior

```powershell
git checkout -- .
```

---

### Eliminar Commits

Se debe indicar hasta que hash se tiene que resetar:

```powershell
git reset --hard hash
```

En el Caso que el cambio alla sido enviado al servidor remoto, se puede forzar:

```powershell
git push --force
```

### Descartar cambios

git checkout .

<https://stackoverflow.com/questions/1146973/how-do-i-revert-all-local-changes-in-git-managed-project-to-previous-state>