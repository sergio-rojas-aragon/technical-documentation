---
title: Branch and Merges
layout: home
parent: Git
nav_order: 2
---

# Branchs And Merges

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# Basicos

* **Ver branchs**

    ```
    git branch
    ```

* **Se cambia de rama a la que se esta mencionando**

    ```
    git checkout rama-villanos
    ```

## Crear Ramas

* **Crear una rama nueva:**
    
    ```
    git branch rama-villanos
    ```

* **Crear rama en repositorio remoto**

    ```
    git push origin rama-capitan-loco
    ```

* **Crea rama y se posiciona en ella**
  
    ```
    git checkout -b <branch_name>
    ```

## Delete branchs

* **Eliminar una rama** *es una buena practica eliminar la rama una vez que se termina de usar*

    ```
    git branch -d rama-villanos
    ```

* **Eliminar una rama del remoto**

    ```
    git push <remote_name> --delete <branch_name>
    ```


* **Cambiarse de branch**

    ```
    git checkout rama-villanos
    ```

## Merges

Its important to position on the branch taht will recieve the changes.
If the changes is in BranchB, i position on branch A

* **Merge fast forward**

    ```
    git merge RamaB
    ```

* **Merge con commit y mensaje**

    ```
    git merge --no-ff RamaB -m "mensaje"
    ```

## Others


* **Saber diferencia entre dos ramas**

    ```
    git diff RamaA RamaB
    ```
    
--- 

### Volver al punto anterior

```
git checkout -- .
```

### volver a un punto especifico

```
git reset --soft d70db2a
```

