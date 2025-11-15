---
title: Stage
layout: home
parent: Git
nav_order: 5
---

# Stage

* **deja los archivos en el stage**

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

* **saca del stage**
    todos los archivos xml

    ```
    git reset *.xml
    ```
 

