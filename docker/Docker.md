# Docker commands



Ejemplo:

```yml


```
## Volumes

Hay 3 tipos de volúmenes, son usados para hacer persistente la data entre reinicios y levantamientos de imágenes.

* **Named Volumes**
* **Anonymous Volumes**. Volúmenes donde sólo se especiﬁca el path del contenedor y Docker lo asigna automáticamente en el host
* **Bind Volumes**. pwd es el path en donde se encuentra la terminal, y en lazara en la app.
  
    ```powershell
    -v "$(pwd):/app"
    ```

* List volumes
  
    ```powershell
    docker volume ls
    ```

* Create volume
  
    ```powershell
    docker volume create todo-db
    ```

* Delete volumes if its are not used
  
    ```powershell
    docker volume prune
    ```
* Delete specific by volumes name
  
    ```powershell
    docker volume prune
    ```

## Network

Si dos o más contenedores están en la misma red, podrán hablar entre sí. Si no lo están, no podrán.


* Listar redes: 
    ```powershell
    docker network ls
    ```
* Crear red:
    ```powershell
    docker network create nombre-red
    ```

* Eliminar redes:
    ```powershell
    docker network prune
    ```


* Agregar un contenedor a una red:
    ```powershell
    docker network connect nombre-red nombre-contenedor
    ```

* inspect. Show list of containers in the selected network.
    ```powershell
    docker network inspect <NAME o ID>
    ```

