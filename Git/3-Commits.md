---
title: Commits
layout: home
parent: Git
nav_order: 3
---

# Commits

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# Flujo Base



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
---

# Eliminar

* **Eliminar Commits**

    Se debe indicar hasta que hash se tiene que resetar:

    ```
    git reset --hard hash
    ```
* **Eliminar Commits en Servidor**

    En el Caso que el cambio alla sido enviado al servidor remoto, se puede forzar:

    ```
    git push --force
    ```
---

# Modificar commit

## Agregar archivos a commit anterior

* **Opcion 1**

    Head apunta al ultimo commit que se hizo. `HEAD^` apunta al commit anterior.

    ```
    git reset --soft HEAD^
    ```

    despues de agregar los archivos al commit anterior se tiene que hacer un commit otra vez

    ```
    git commit -am "message"
    ```

* **Opcion 2**

    
    - Se realizan las modificaciones
    - se agregan al stage (git add .)
    - Se agrega al commit anterior, fijarse que el mensaje sea el mismo que el commit anterior:

    ```
    git commit --amend -m "mensaje commit anterior"
    ```
    > amend significa enmendar

* **Opcion 3**
    - se agrega la modificacion al stage:
  
    ```
    git add modal.js
    ```

    - se agrega al commit anterior, no edit sirve para no modificar el texto del commit anterior: 

    ```
    git commit --ammend --no-edit
    ```

    {: .important }
    > ojo, es posible que el push no funcione, se tiene que hacer un:

    ```
    git push --force origin master
    ```

    posible problema por branch protegida, para esto se tiene que ir settings-reposotory-branch protected branch


## Cambiar mensaje de commit

se selecciona el ultimo elemento

```
git rebase - HEAD~1
```
en el editor de texto cambiar el pick por reword `escape+:wq`

se abrira otra vez el editor de texto y ahi se cambia el mensaje



