---
title: Get Started
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

# Configs

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

```terminal
git config --global --unset user.password
```

despues pedira las credenciales


