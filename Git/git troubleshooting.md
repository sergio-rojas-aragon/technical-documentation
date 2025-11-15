---
title: Troubleshooting
layout: home
parent: Git
nav_order: 10
---

# trouble shooting

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


### git ignore no funciona

```
git rm -r --cached .
git add .;
git commit -m "fix .gitignore";
```

Fuente: https://bytefreaks.net/programming-2/my-gitignore-file-is-ignored-by-git-and-it-does-not-work

### Carpeta mayuscula no se guarda en repo

```
git config core.ignorecase false
```

### Delete branch in Local/Remote

Local:
```
git branch -d 
```

Remote:
```
git push --delete origin nombre_rama
```

### Case - Save the current changes and load previus commit or version

primero se hace
1. use 
   
```
git stash
```

2. comprobar que aparece el trabajo pausado. Se puede tener varios stash

```
git stash list
```
3. cargar el ultimo commit, se realizan modificaciones y luego se guardan esos cambios con un commit.
4. se vuelven a cargar los datos que se estaban modificando, el pop trae el ultimo stage guardado.

```
git stash pop
```
Cuando hay conflictos en el archivo se resuelve igual que cualquier conflicto del commit. luego se tiene que borrar el stash

```
git stash drop
```

### Case - Que hacer cuando se cambia el nombre del repositorio

Revisar origin con `git remote -v`. Actualizar la url del remoto(recordar que los repositorios terminan en .git):

```
git remote set-url origin https://github.com/usuario/nuevo-nombre.git
```


