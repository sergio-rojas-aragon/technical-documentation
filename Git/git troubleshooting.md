# troubleshooting

Menu

- [troubleshooting](#troubleshooting)
    - [git ignore no funciona](#git-ignore-no-funciona)
    - [Carpeta mayuscula no se guarda en repo](#carpeta-mayuscula-no-se-guarda-en-repo)

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