---
title: Troubleshooting
layout: home
parent: Git
nav_order: 3
---

# trouble shooting

Menu

- [trouble shooting](#trouble-shooting)
    - [git ignore no funciona](#git-ignore-no-funciona)
    - [Carpeta mayuscula no se guarda en repo](#carpeta-mayuscula-no-se-guarda-en-repo)
    - [Delete branch in Local/Remote](#delete-branch-in-localremote)
    - [Case - Save the current changes and load previus commit or version](#case---save-the-current-changes-and-load-previus-commit-or-version)

### git ignore no funciona

```terminal
git rm -r --cached .
git add .;
git commit -m "fix .gitignore";
```

Fuente: https://bytefreaks.net/programming-2/my-gitignore-file-is-ignored-by-git-and-it-does-not-work

### Carpeta mayuscula no se guarda en repo

```terminal
git config core.ignorecase false
```

### Delete branch in Local/Remote

Local:
```terminal
git branch -d 
```

Remote:
```terminal
git push --delete origin nombre_rama
```

### Case - Save the current changes and load previus commit or version

primero se hace
1. use 
   
```terminal
git stash
```

2. comprobar que aparece el trabajo pausado. Se puede tener varios stash

```terminal
git stash list
```
3. cargar el ultimo commit, se realizan modificaciones y luego se guardan esos cambios con un commit.
4. se vuelven a cargar los datos que se estaban modificando, el pop trae el ultimo stage guardado.

```terminal
git stash pop
```
Cuando hay conflictos en el archivo se resuelve igual que cualquier conflicto del commit. luego se tiene que borrar el stash

```terminal
git stash drop
```
### Case - Que hacer cuando se cambia el nombre del repositorio

Revisar origin con `git remote -v`. Actualizar la url del remoto(recordar que los repositorios terminan en .git):

```terminal
git remote set-url origin https://github.com/usuario/nuevo-nombre.git
```


