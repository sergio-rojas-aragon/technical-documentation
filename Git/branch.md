---
title: Branch
layout: home
parent: Git
nav_order: 2
---

# Branchs commands

### Ver branchs

```terminal
git branch
```

### Se cambia de rama a la que se esta mencionando

```terminal
git checkout rama-villanos
```

### Crear rama en repositorio remoto

```terminal
git push origin rama-capitan-loco
```

### Crear una rama nueva:
  
```terminal
git branch rama-villanos
```

### Crea rama y se posiciona en ella
  
```terminal
git checkout -b <branch_name>
```

### se cambia de rama a la que se esta mencionando

```terminal
git checkout rama-villanos
```
## Delete branchs

### Eliminar una rama. es una buena practica eliminar la rama una vez que se termina de usar.

```terminal
git branch -d rama-villanos
```

### Eliminar una rama del remoto

```terminal
git push <remote_name> --delete <branch_name>
```

### Saber diferencia entre dos ramas:

```terminal
git diff rama-villanos master
```

### Merge con mensaje

```terminal
git merge --no-f rama -m "mensaje"
```

### Volver al punto anterior

```terminal
git checkout -- .
```