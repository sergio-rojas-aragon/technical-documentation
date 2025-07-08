---
title: Get Started - Basico
layout: home
parent: Git
nav_order: 1
---

# Apuntes Git

Aca dejare todos los comandos GIT para evitar buscarlos en google.

## Menu

1. [troubleshooting](<git troubleshooting.md>)


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Tabla de Contenidos
- [Apuntes Git](#apuntes-git)
  - [Menu](#menu)
  - [Table of contents](#table-of-contents)
  - [Tabla de Contenidos](#tabla-de-contenidos)
  - [Get Started](#get-started)
  - [Alias](#alias)
  - [Stage commands](#stage-commands)
  - [Commit commands](#commit-commands)
    - [commit simple](#commit-simple)
    - [commit en una linea](#commit-en-una-linea)
    - [Agregar cambios a commit anterior](#agregar-cambios-a-commit-anterior)
    - [Cambiar mensaje de commit](#cambiar-mensaje-de-commit)
  - [Procedimientos](#procedimientos)
    - [Cambiar Commit anterior](#cambiar-commit-anterior)
    - [Unir ramas](#unir-ramas)
    - [Unir ramas con conflicto](#unir-ramas-con-conflicto)
    - [Recorrer ramas](#recorrer-ramas)
    - [Eliminar Commits](#eliminar-commits)
    - [Descartar cambios](#descartar-cambios)

## Get Started

- Configuracion de GIT:

```terminal
git config --global user.name "cambiar"
git config --global user.email "cambiar@gmail.com"
```

- Crear Alias

```terminal
git config --global alias.lg "log --oneline --decorate --all --graph"
```
```terminal
git config --global alias.s "status -s -b"
```

- Ver configuracion creada:

```terminal
git config --global -e
```

- Si se cambia de pasword usar lo siguiente

```terminal
git config --global --unset user.password
```

despues pedira las credenciales

## Alias

Alias son atajos que se pueden crear en el sistema git en forma local, para eso se usa git config. Ejemplos:

- Crear alias:

```terminal
git config --global alias.lg "log --oneline --decorate --all --graph"
```

- Como se usa:

```terminal
git lg
```

- Crear alias:

git config --global alias.s "status -s -b"

- Como se usa:

```terminal
git s 
```


## Stage commands

- Quitar archivos del stage

    ```terminal
    git reset *.xml
    git reset nombrearchivo
    ```

## Commit commands

### commit simple

```terminal
git commit
```
Abre un archivo commit en donde se puede editar multilinea
para editar colocar la letra a
luego de terminar de escribir colocar escape :wq

### commit en una linea

```terminal
git commit -m "Template listo sin rutas"
```


Modifica el commit anterior (amend significa enmendar)

```terminal
git commit --amend -m "Actualizacion de mensaje"
```

git add .

deja los archivos en el stage o escenario

git add .*png

agrega todos los archivos que tengan la extencion png dentro del proyecto

git add css/

agrega todos los archivos que estan en esa carpeta

git add -A

Agrega todos los archivos que han sido modificados

git add ".*png"

agregara todos los archivos dentro del directorio actual

git add --all
agrega todos los archivos

git add pdfs/*.pdf
agega todos los pdfs dentro de la carpeta pdfs

### Agregar cambios a commit anterior

```terminal
git reset --soft HEAD^
```

Head apunta al ultimo commit que se hizo. HEAD^ apunta al commit anterior.

despues de agregar los cambios al commit anterior se tiene que hacer un commit otra vez

```terminal
git commit -am "message"
```

### Cambiar mensaje de commit

se selecciona el ultimo elemento

git rebase - HEAD~1

en el editor de texto cambiar el pick por reword
escape+:wq

se abrira otra vez el editor de texto y ahi se cambia el mensaje

## Procedimientos

### Cambiar Commit anterior

- Se realizan las modificaciones
- se agregan al stage (git add .)
- Se agrega al commit anterior, fijarse que el mensaje sea el mismo que el commit anterior:

```terminal
git commit --amend -m "mensaje commit anterior"
```

Otra forma (Con ejemplo):

- se agrega la modificacion al stage:
  
```terminal
git add modal.js
```

- se agrega al commit anterior, no edit sirve para no modificar el texto del commit anterior: 

```terminal
git commit --ammend --no-edit
```

ojo, es posible que el push no funcione, se tiene que hacer un:

```terminal
git push --force origin master
```

posible problema por branch protegida, para esto se tiene que ir settings-reposotory-branch protected branch

### Unir ramas

para unir ramas se tiene que posicionarse e la rama en donde se quiere fucionar algo:

```terminal
git checkout master
```

head = apunta al ultimo commit a la rama de al cual estamos

```terminal
git merge rama-villanos
```

merge ramas con commit de mensaje

```terminal
git merge --no-f rama -m "mensaje"
```

### Unir ramas con conflicto

Cuando se van a unir ramas es probable que salga un mensaje:

```terminal
CONFLICT CONTENT: Merge conflict in archivo.txt
automatic merge failed.
```

Dentro del head y el == es el cambio de la rama actual. Dentro del == y el nombre de la rama es el cambio que esta en la rama.

```terminal
<<<<<< HEAD
hola
======
chao
>>>>>> rama
```

Simplemente se deja el texto que se quiere dejar y el resto se borra

```terminal
chao
```

---

### Recorrer ramas

- volver a un punto especifico

```terminal
git reset --soft d70db2a
```

- volver al punto anterior

```terminal
git checkout -- .
```

---

### Eliminar Commits

Se debe indicar hasta que hash se tiene que resetar:

```terminal
git reset --hard hash
```

En el Caso que el cambio alla sido enviado al servidor remoto, se puede forzar:

```terminal
git push --force
```

### Descartar cambios

git checkout .

<https://stackoverflow.com/questions/1146973/how-do-i-revert-all-local-changes-in-git-managed-project-to-previous-state>