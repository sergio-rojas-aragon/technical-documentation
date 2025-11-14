---
title: Commits
layout: home
parent: Git
nav_order: 6
---

# Commits

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# Flujo Base

```
        ╔══════════════════════════════╗
        ║      Working Directory       ║
        ║   (editas archivos aquí)     ║
        ╚───────────────╦──────────────╝
                        │
                        │ git add
                        ▼
            ┌────────────────────────┐
            │      Staging Area      │
            │  (índice: cambios OK)  │
            └─────────────┬──────────┘
                        │
                        │ git commit
                        ▼
            ┌────────────────────────┐
            │       Local Repo       │
            │ (tus commits locales)  │
            └───────────┬────────────┘
                        │
                        │ git push
                        ▼
            ┌────────────────────────┐
            │      Remote Repo       │
            │ (GitHub / GitLab etc.) │
            └───────────┬────────────┘
                        │
                        │ git pull (fetch + merge)
                        ▼
            (Local Repo actualizado)
```

# Basic

* **commit simple**

    Abre un archivo commit en donde se puede editar multilinea. Para editar colocar la letra A
    luego de terminar de escribir colocar escape :wq

    ```
    git commit
    ```

* **commit en una linea**

    ```
    git commit -m "Template listo sin rutas"
    ```



### Modifica el commit anterior (amend significa enmendar)

```
git commit --amend -m "Actualizacion de mensaje"
```

# Stage


### deja los archivos en el stage o escenario

```
git add .
```

### agrega todos los archivos que tengan la extencion png dentro del proyecto

```
git add .*png
```

### agrega todos los archivos que estan en esa carpeta

```
git add css/
```
### Agrega todos los archivos que han sido modificados

```
git add -A
```

### agrega todos los pdfs dentro de la carpeta pdfs

```
git add pdfs/*.pdf
```



### Agregar cambios a commit anterior

```
git reset --soft HEAD^
```

Head apunta al ultimo commit que se hizo. HEAD^ apunta al commit anterior.

despues de agregar los cambios al commit anterior se tiene que hacer un commit otra vez

```
git commit -am "message"
```

### Cambiar mensaje de commit

se selecciona el ultimo elemento

git rebase - HEAD~1

en el editor de texto cambiar el pick por reword
escape+:wq

se abrira otra vez el editor de texto y ahi se cambia el mensaje

# Procedimientos

## Cambiar Commit anterior

- Se realizan las modificaciones
- se agregan al stage (git add .)
- Se agrega al commit anterior, fijarse que el mensaje sea el mismo que el commit anterior:

```
git commit --amend -m "mensaje commit anterior"
```

Otra forma (Con ejemplo):

- se agrega la modificacion al stage:
  
```
git add modal.js
```

- se agrega al commit anterior, no edit sirve para no modificar el texto del commit anterior: 

```
git commit --ammend --no-edit
```

ojo, es posible que el push no funcione, se tiene que hacer un:

```
git push --force origin master
```

posible problema por branch protegida, para esto se tiene que ir settings-reposotory-branch protected branch

## Unir ramas

para unir ramas se tiene que posicionarse e la rama en donde se quiere fucionar algo:

```
git checkout master
```

head = apunta al ultimo commit a la rama de al cual estamos

```
git merge rama-villanos
```

merge ramas con commit de mensaje

```
git merge --no-f rama -m "mensaje"
```

## Unir ramas con conflicto

Cuando se van a unir ramas es probable que salga un mensaje:

```
CONFLICT CONTENT: Merge conflict in archivo.txt
automatic merge failed.
```

Dentro del head y el == es el cambio de la rama actual. Dentro del == y el nombre de la rama es el cambio que esta en la rama.

```
<<<<<< HEAD
hola
======
chao
>>>>>> rama
```

Simplemente se deja el texto que se quiere dejar y el resto se borra

```
chao
```


### Eliminar Commits

Se debe indicar hasta que hash se tiene que resetar:

```
git reset --hard hash
```

En el Caso que el cambio alla sido enviado al servidor remoto, se puede forzar:

```
git push --force
```

### Descartar cambios

git checkout .

<https://stackoverflow.com/questions/1146973/how-do-i-revert-all-local-changes-in-git-managed-project-to-previous-state>