---
title: Branch
layout: home
parent: Git
nav_order: 2
---

# Branchs commands

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