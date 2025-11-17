---
title: SonarQube
layout: home
parent: CI/CD
nav_order: 3
---

# SonarQube
{: .no_toc }

Documentacion de instalacion, configuracion y uso de SonarQube
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta .fs-4 }

1. TOC
{:toc}

---

# Presentation

SonarQube es una plataforma que analiza tu código buscando:

* Bugs
* Vulnerabilidades
* Code smells
* Duplicación
* Cobertura de código
* Calidad por módulos y archivos

# Install in Docker

```
# SonarQube

version: "3.8"

services:
  # 1. Servicio de Base de Datos PostgreSQL (requerido para persistencia)
  dbQube:
    image: postgres:14
    container_name: sonarqube_db
    environment:
      # Credenciales de la BD. CÁMBIALAS a valores seguros.
      - POSTGRES_USER=sonar
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=sonar
    
    # Volumen persistente para los datos de la base de datos
    volumes:
      - ./sonarqube_data/postgres:/var/lib/postgresql/data
    
    # Reiniciar el contenedor si se detiene, a menos que se detenga manualmente
    restart: unless-stopped

  # 2. Servicio de SonarQube (conectado a la BD)
  sonarqube:
    image: sonarqube:latest # Versión específica solicitada
    container_name: sonarqube
    depends_on:
      - dbQube # Asegura que la BD se inicie antes que SonarQube
    
    ports:
      - "9000:9000" # Puerto de la interfaz web (SonarQube)
      - "9001:9001" # Puerto para el tráfico HTTP interno (opcional)

    environment:
      # Configuración de conexión a la BD (usa el nombre del servicio 'db')
      - SONARQUBE_JDBC_URL=jdbc:postgresql://db:5432/sonar
      - SONARQUBE_JDBC_USERNAME=sonar
      - SONARQUBE_JDBC_PASSWORD=sonarweb
      # Configuración de memoria (ajustar según los recursos disponibles)
      - SONAR_WEB_JAVA_OPTS=-Xmx512m -Xms512m
      - SONAR_CE_JAVA_OPTS=-Xmx512m -Xms512m
      - SONAR_SEARCH_JAVA_OPTS=-Xmx512m -Xms512m
    
    # Volúmenes persistentes para la data, extensiones y logs de SonarQube
    volumes:
      - ./sonarqube_data/data:/opt/sonarqube/data
      - ./sonarqube_data/extensions:/opt/sonarqube/extensions
      - ./sonarqube_data/logs:/opt/sonarqube/logs

    # Reiniciar el contenedor si se detiene, a menos que se detenga manualmente
    restart: unless-stopped
    
```

## Configuracion

* Entrar a localhost:9000
* login admin/admin
* cambiar contraseña

**Crear Proyecto**

***Projects → Create Project***

* Opcion mmanually
* Project key (similar al de github)
* name: igual project key (por ahora)

## Crear token

El token se usa para que Jenkins se pueda conectar con SonarQube

***Set Up → Locally***

* Nombre
* Copiar el token

## Configuracion en Jenkins

* Manage Jenkins
* Plugins
* Buscar e instalar:
    * SonarQube Scanner
* Reiniciar Jenkins.

luego

* ir a Manage Jenkins → Tools
* Buscar sección: SonarQube Scanner
    * Add:
    * Name: sonar-scanner
    * Install automatically → choose latest


# Integrar SonarQube con Jenkins


```
        stage('SonarQube - Begin') {
            steps {
                dir('app') {
                    withSonarQubeEnv('sonarqube') {
                        sh """
                            dotnet tool install --global dotnet-sonarscanner
                            export PATH="\$PATH:/root/.dotnet/tools"

                            dotnet sonarscanner begin \
                                /k:"demo-dotnet-api" \
                                /d:sonar.host.url="http://sonarqube:9000" \
                                /d:sonar.login="${SONAR_AUTH_TOKEN}" \
                                /d:sonar.cs.opencover.reportsPaths="TestResults/coverage.opencover.xml"
                        """
                    }
                }
            }
        }

        stage('SonarQube - End') {
            steps {
                dir('app') {
                    withSonarQubeEnv('sonarqube') {
                        sh """
                            export PATH="\$PATH:/root/.dotnet/tools"
                            dotnet sonarscanner end /d:sonar.login="${SONAR_AUTH_TOKEN}"
                        """
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
```