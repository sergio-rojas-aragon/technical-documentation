---
title: Npm
layout: home
nav_order: 15
---

# NPM

## Rearmar node modules

    ```terminal
    npm i
    ```


## actualizacion de dependencias

* ver librerias del proyecto

    ```terminal
    npm list --depth=0
    ```

* forzar instalar dependencias que no estan actualizadas

    ```terminal
    npm i --legacy-peer-deps
    ```

* ver dependencias de una libreria

    ```terminal
    npm ls <libreria>
    ```
* ver paquetes desactualizados

    ```terminal
    npm outdated
    ```

* desinstalar paquete

    ```terminal
    npm uninstall <nombre-del-paquete>
    ```

