---
title: Stage
layout: home
parent: Git
nav_order: 2
---

# Stage Commands

# Agregar

* **deja los archivos en el stage**

    ```
    git add .
    ```

* **agrega archivo especifico**

    ```
    git add README.md
    ```

* **agregar archivos por extension**

    ```
    git add .*png
    ```

* **agrega todos los archivos que estan en esa carpeta**

    ```
    git add css/
    ```

* **agrega el archgivo txt de todo el proyecto**

    ```
    git add "*.txt" 
    ```

* **agrega todos los archivos del directorio actual**

    ```
    git add *.txt 
    ```

* **Agrega todos los archivos que han sido modificados**

    ```
    git add -A
    ```

* **agrega todos los pdfs dentro de la carpeta pdfs**

    ```
    git add pdfs/*.pdf
    ```

* **saca del stage**
    todos los archivos xml

    ```
    git reset *.xml
    ```
 
 # Quitar

* **quitar archivos de stage**

    ```
    git reset
    ```

* **restarurar archivos de una carpeta**

    ```
    git restore carpeta
    ```

