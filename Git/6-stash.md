---
title: Stash
layout: home
parent: Git
nav_order: 6
---
# Stash

es un contenedor en donde se deja el codigo que se esta trabajando actualmente, y despues se pueda recuperar.

entocnes por ejemplo se esta trabajando en la master y se estan realizando algunas modificaciones
pero se necesita urgente cargar el ultimo commit

se utiliza entonces:

```
git stach
```

lo que hace es guardar el trabajo que se estaba realizando y lo deja en un punto con hash.

para ver los stash guardados:

```
git stach list
```

se hacen modificaciones en la master y se pueden hacer algunos commits de la emergencia y se quiere recuperar los cambios del stash.

```
git stash pop 
```





