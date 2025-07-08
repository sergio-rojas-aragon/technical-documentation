---
title: Tags
layout: home
parent: Git
nav_order: 5
---

# Tags

### Crear tag

```terminal
git tag nombre
```

### ver tags

```powershell
git tag
```

### borrar tags

```powershell
git tag -d nombreTag
```

###agregar tag con anotaciones

```powershell
git tag -a v1.0.0 -m "Version 1.0.0"
```

### agregar tag con anotaciones a otro punto de la rama

```powershell
git tag -a v0.1.0 345d7de -m "Version alfa"
```

### para ver lo que contiene cierto tag

```powershell
git show v0.1.0
```
### subir tags

```powershell
git push --tags
```