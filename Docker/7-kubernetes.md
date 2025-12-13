---
title: Kubernetes
layout: home
parent: Docker
nav_order: 7
---

# Kubernetes
{: .no_toc }


Kubernetes es un **orquestador** que mantiene tus aplicaciones corriendo, reparándolas y escalándolas automáticamente.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# MiniKube

## Instalacion

Se tiene que hacer con permisos de administrador:

```terminal
choco install minikube -y
```

**Iniciar el cluster:**

si se tiene docker desktop:

```terminal
minikube start --driver=docker
```

puedes usar Hyper-V o VirtualBox:

```terminal
minikube start --driver=hyperv
```

```terminal
minikube start --driver=virtualbox
```


**Confirmar que funciona:**

```terminal
minikube status
```


```terminal
kubectl get nodes
```

Debe mostrar 1 nodo Ready.

## Configuracion

```terminal
kubectl create namespace demoapi
```

# Uso

## deployment.yml

Un Deployment define cómo se despliega y administra tu aplicación dentro del clúster.
Describe la aplicación en sí y cómo Kubernetes debe ejecutarla y mantenerla funcionando.

Sirve para describir:

* Qué **imagen** de contenedor usar
* Cuántas **réplicas** de tu aplicación correrán
* Cómo deben actualizarse (rolling updates)
* Qué **puerto** expone el contenedor
* Qué variables de entorno, recursos, probes, etc.

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mi-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: mi-app
  template:
    metadata:
      labels:
        app: mi-app
    spec:
      containers:
        - name: mi-app-container
          image: miimagen:1.0.0
          ports:
            - containerPort: 3000
          env:
            - name: NODE_ENV
              value: "production"
          resources:
            limits:
              cpu: "500m"
              memory: "512Mi"
            requests:
              cpu: "200m"
              memory: "256Mi"
          readinessProbe:
            httpGet:
              path: /
              port: 3000
            initialDelaySeconds: 5
            periodSeconds: 10
          livenessProbe:
            httpGet:
              path: /
              port: 3000
            initialDelaySeconds: 15
            periodSeconds: 20

```

### Encabezado

```yml
apiVersion: apps/v1
kind: Deployment
```

* apiVersion: versión del API que define este tipo de recurso.
* kind: el tipo de objeto => Deployment.

### metadata

```yml
metadata:
  name: mi-app
```

* Le da un nombre al Deployment.
* Se usa para identificarlo después con kubectl.

El nombre del Deployment **no** es el nombre del Pod; Kubernetes generará nombres de pods dinámicos como `mi-app-5f8d9c8f8f-mp2dk`

### spec: la parte principal

Aquí declaras lo que quieres que exista.

### replicas

```yml
replicas: 3
```

Esto le dice a Kubernetes:
👉 "Quiero que siempre existan 3 pods iguales de esta aplicación."

Si uno cae → Kubernetes lo recrea.

### selector

```yml
selector:
  matchLabels:
    app: mi-app
```

Esto indica qué pods pertenecen a este Deployment. Kubernetes usará estas labels para saber qué Pods debe controlar.

### template: plantilla para crear pods

```yml
template:
  metadata:
    labels:
      app: mi-app
```

El Deployment usa esta plantilla para crear nuevos Pods. Las labels aquí deben coincidir con el selector.

👉 Cada Pod creado tendrá estas labels.

### spec (de los pods)

```yml
spec:
  containers:
    - name: mi-app-container
```

Aquí defines cómo debe ser cada contenedor dentro del pod.

### image

```yml
image: miimagen:1.0.0
```

* Define qué imagen usar.Es la parte más importante.
* Cambiar el tag permite hacer deploy continuo (rolling updates).

### ports

```yml
ports:
  - containerPort: 3000
```

Indica qué puerto abre la app dentro del contenedor.

OJO: esto no expone el servicio fuera del clúster, solo sirve para declarar el puerto interno del contenedor.

### Variables de entorno env

```yml
env:
  - name: NODE_ENV
    value: "production"
```

Define variables como:
* claves
* configuración
* URLs
* etc.

### resources

```yml
resources:
  limits:
    cpu: "500m"
    memory: "512Mi"
  requests:
    cpu: "200m"
    memory: "256Mi"
```

Define cuánto consume la aplicación:
* requests = lo mínimo necesario para arrancar
* limits = lo máximo permitido
* Evita saturar el nodo.

### Liveness y Readiness Probes

Servidores web y APIs deben tener probes.

readinessProbe

```yml
readinessProbe:
  httpGet:
    path: /
    port: 3000
```

Indica cuándo el pod está listo para recibir tráfico.
Antes de eso, el Service no lo incluye en el load balancing.
Indica si la app está viva.
Si falla → Kubernetes reinicia el pod.


## service.yml

Un Service define cómo se accede a la aplicación dentro del clúster (y a veces desde afuera).
Permite que otros pods o usuarios puedan acceder a tu aplicación.

Sirve para:

* Dar una IP estable a un conjunto de pods (los pods cambian, el Service no).
* Hacer balanceo de carga entre réplicas.
* Exponer la aplicación internamente (ClusterIP) o externamente (NodePort / LoadBalancer).

```yml
apiVersion: v1
kind: Service
metadata:
  name: mi-app-service
spec:
  selector:
    app: mi-app
  ports:
    - port: 80
      targetPort: 3000
  type: ClusterIP
```

### Encabezado

```yml
apiVersion: v1
kind: Service
```

* apiVersion: todos los Services usan v1.
* kind: indica que es un Service.

### metadata

```yml
metadata:
  name: mi-app-service
```

Esto:
* Le da un nombre único al Service.
* Se usa para referenciarlo con kubectl.
* No tiene relación directa con el nombre del Deployment o los pods.

### spec

### selector

```yml
selector:
  app: mi-app
```

* Esto le dice al Service "Enruta tráfico hacia cualquier Pod que tenga esta label."
* Es lo que conecta Service → Pods.

Los Pods normalmente tienen esa label porque el Deployment se la pone en el template.

### ports

```yml
ports:
  - protocol: TCP
    port: 80
    targetPort: 3000
```

Aquí defines cómo el Service expone tu aplicación.
* protocol: Normalmente TCP, puede ser UDP o SCTP.
* port: Es el puerto del Service: "Alguien dentro del clúster me llama en el puerto 80".
* targetPort: Es el puerto del contenedor. Es decir, el Service redirige tráfico hacia *Service:80 → Pod:3000*

### type

```yml
type: ClusterIP
```

Define cómo se expone el Service. Las opciones principales:

* ClusterIP (por defecto) Solo accesible dentro del clúster. Ideal para comunicación interna entre servicios.
* NodePort: Expone el Service en cada nodo del clúster en un puerto entre 30000–32767.
* LoadBalancer_ Pide un load balancer externo al proveedor cloud (AWS, GCP, Azure, DigitalOcean, etc).
* ExternalName: Crea un alias DNS hacia otro dominio.


## Flujo

1. Despliegas la app:
    ```terminal
    kubectl apply -f deployment.yml
    ```
1. Se crean los pods.
1. Creas el service:
    ```terminal
    kubectl apply -f service.yml
    ```
1. El Service detecta los pods (por las labels) y empieza a dirigir tráfico a ellos.