---
title: Get Started
layout: home
parent: Git
nav_order: 1
---

# Apuntes Git
{: .no_toc }

Aca dejare todos los comandos GIT para evitar buscarlos en google.
{: .fs-6 .fw-70 }

## Table of contents
{: .no_toc .text-delta .fs-4 }

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

# Inicializacion

* **Configuracion de GIT**

    ```
    git config --global user.name "cambiar"
    git config --global user.email "cambiar@gmail.com"
    ```

* **Ver configuracion creada**

    para salir hay que colocar `:wq`

    ```
    git config --global -e
    ```

# Alias

Alias son atajos que se pueden crear en el sistema git en forma local, para eso se usa git config.

* **Crear Alias**

    ```
    git config --global alias.lg "log --oneline --decorate --all --graph"
    ```

    ```terminal
    git config --global alias.s "status -s -b"
    ```


# Cambio de password

```
git config --global --unset user.password
```

despues pedira las credenciales


#  Comandos basicos


* **Inicializacion de repositorio**

    ```
    git init
    ```

* **Colocar archivos en stage**

    ```
    git add index.html
    ```

* **Quitar archivos del stage**

    ```
    git reset HEAD README.md
    ```

* **revertir cambios en un archivos**

    ```
    git checkout -- README.md
    ```

* **Estado del repositorio**
    muestra los archivos que han sido modificados y los que estan en stage
    
    ```
    git status
    ```
-- Ver cambios que se hicieron en el commit anterior
git diff

* **ver cambios de archivos que estan en el stage**

    ```
    git diff --stagged
    ```

# Configuracion repositorio remoto

* **para agregar repositorio remoto a repo ya existente**

    ```
    git remote add origin http://git.contaline.cl/SistemaASP/clasesasp.git
    ```

* **Clonar**

    ```
    git clone https://github.com/sergiorojas09/UdemyGitHeroes.git demo-10
    ```



head = apunta al ultimo commit a la rama de al cual estamos

# Logs

* **muestra todo el log**

    ```
    git log
    ```

* **se visualiza el hash corto**

    ```
    git log --oneline
    ```

* **Vista grafica**

```
git log -- oneline --decorate --all --graph
```
