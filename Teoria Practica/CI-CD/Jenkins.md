---
title: Jenkins
layout: home
parent: CI/CD
nav_order: 3
---

# Jenkins
{: .no_toc }

Documentacion de instalacion, configuracion y uso de Jenkins
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta .fs-4 }

1. TOC
{:toc}

---

# Jenkins en Docker

Para mantenerlo en disco

crear carpeta con `mkdir -p ~/jenkins_data`

crear archivo docker-compose.yml

```
version: '3.8'

services:
  jenkins:
    build: .
    image: jenkins/jenkins:jdk21
    container_name: jenkins
    environment:
      - DOCKER_HOST=tcp://host.docker.internal:2375    
    ports:
      - "8080:8080"
      - "50000:50000"

    volumes:
      - ./jenkins_data:/var/jenkins_home
    restart: unless-stopped

```

buscar la password:

```
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

### Instalar pluggins

En Jenkins → Manage Jenkins → Plugins → Available Plugins, instala:

* Pipeline
* Git
* GitHub Integration Plugin
* Blue Ocean (interfaz moderna para pipelines)
* .NET SDK Installer (opcional, si deseas instalar .NET en el contenedor)

(en el módulo siguiente instalaremos SonarQube)

Reinicia Jenkins después de instalarlos.

# Paso a paso — Jenkins ← GitHub (Pipeline from SCM)

Precondiciones

* Jenkins corriendo (ej. http://localhost:8080) y plugins: Git, Pipeline, GitHub instalados.
* repo en GitHub ya existe y contiene Jenkinsfile en la raíz de la rama main o master

## Conexion con Jenkins local con Github

***Si el repo es público*** → se puede saltar las credenciales y usar la URL HTTPS directa.
***Si el repo es privado*** → crea un Personal Access Token (PAT) en GitHub

### Generar access token

Ir a *GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token*

**Scopes mínimos recomendados**

 * repo (completo)
 * admin:repo_hook si quieres que Jenkins cree webhooks
 
 Copia el token.

### Agregar credenciales en Jenkins 

(solo para repos privados o para mejores integraciones)

Ir a *Jenkins → Manage Jenkins → Credentials*

* Seleccionar global
* Add Credentials

Llenar:

* **Tipo**: Seleccionar "Username with password"
* **Username:** tu usuario GitHub
* **Password:** pegar el token generado
* **ID / Description:** colocar por ejemplo github-pat-demo


> (Alternativa moderna: usa la integración GitHub App; más segura pero requiere pasos adicionales en GitHub).

## Crear nuevo pipeline en Jenkins (desde SCM)

* *Jenkins → New Item → escribe nombre del job → **Pipeline** → OK.*

En la configuración del job → sección **Pipeline**:

* Definition: Pipeline from SCM
* SCM: Git
* Repository URL: `https://github.com/<tu_usuario>/<tu_repo>.git`
* Credentials: selecciona la credencial que agregaste (o None para repos públicos).
* Branches to build: */main (o la rama que uses)
* Script Path: Jenkinsfile (ruta dentro del repo)

Guardar.

## Probar manualmente

* En el job creado → **Build Now.**
* Revisa la consola de la ejecución (Console Output) para confirmar que Jenkins hizo git clone, ejecutó etapas del Jenkinsfile y completó build/test.

## (Opcional) Activar webhooks automáticos

Si se usa PAT con admin:repo_hook, Jenkins puede crear webhooks automáticamente. Si no:

* Ve a GitHub → Repo → Settings → Webhooks → Add webhook.
* Payload URL: http://<tu-jenkins-host>/github-webhook/ (ej.: http://localhost:8080/github-webhook/)
* Content type: application/json
* Events: elegir Push y Pull request.

Ahora cada push/PR desencadenará el job automáticamente.

Verificar disparo automático

Haz un git push al repo (o abre un PR).

En Jenkins → Activity/Builds del job deberías ver un nuevo build iniciado automáticamente.



# Jenkins File

```
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<tu_usuario>/DemoApi-CICD.git'
            }
        }

        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --no-restore --configuration Release'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test --no-build --verbosity normal'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline ejecutado correctamente.'
        }
        failure {
            echo '❌ Algo falló en el pipeline.'
        }
    }
}

```

| Bloque        | Función                                             | Ejemplo                           |
| ------------- | --------------------------------------------------- | --------------------------------- |
| `pipeline {}` | Define el pipeline declarativo.                     | `pipeline { agent any ... }`      |
| `agent any`   | Jenkins puede usar cualquier nodo/VM para ejecutar. | —                                 |
| `stages {}`   | Contiene las etapas del pipeline.                   | `stage('Build')`, `stage('Test')` |
| `steps {}`    | Comandos ejecutados dentro de cada etapa.           | `sh 'dotnet test'`                |
| `post {}`     | Acciones al finalizar (éxito o fallo).              | Mensajes, notificaciones, etc.    |
