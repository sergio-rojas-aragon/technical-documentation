---
title: Tags
layout: home
parent: Git
nav_order: 5
---

# Tags
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

--- 

* **Crear tag**

    ```terminal
    git tag nombre
    ```

* **ver tags**

    ```terminal
    git tag
    ```

* **borrar tags**

    ```terminal
    git tag -d nombreTag
    ```

* **agregar tag con anotaciones**

    ```terminal
    git tag -a v1.0.0 -m "Version 1.0.0"
    ```

* **agregar tag con anotaciones a otro punto de la rama**

    ```
    git tag -a v0.1.0 345d7de -m "Version alfa"
    ```

* **para ver lo que contiene cierto tag**

    ```terminal
    git show v0.1.0
    ```

* **subir tags**

    ```terminal
    git push --tags
    ```