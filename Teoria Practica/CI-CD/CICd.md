---
title: Teoria
layout: home
parent: CI/CD
nav_order: 1
---

# CI/CD
{: .no_toc }

Informacion General

1. TOC
{:toc}

# Que es CI/CD

CI/CD significa Integración Continua (Continuous Integration) y Entrega/Despliegue Continuo (Continuous Delivery/Deployment).

Son prácticas que automatizan el ciclo de desarrollo de software para entregar código más rápido, estable y con menos errores.

## Integración Continua (CI)

{: .important }
> 👉 Objetivo: detectar errores rápido y mantener el código siempre “listo para entregar”.

Qué implica:

* Cada vez que un desarrollador sube código (push o pull request), se dispara un pipeline automático.
* Ese pipeline compila, ejecuta pruebas unitarias, y valida calidad de código.
* Si algo falla, el equipo lo sabe al instante.

> **Beneficio:** evitas el clásico “funciona en mi máquina”.

{: .highlight }
> Ejemplo: Cada commit en tu repo ejecuta pruebas automáticas en GitHub Actions o Jenkins.

## Entrega Continua (CD – Continuous Delivery)

{: .important }
> 👉 Objetivo: tener siempre una versión lista para desplegar.

Qué hace:

* Después de pasar CI, el sistema genera artefactos (por ejemplo, imágenes Docker).
*Los guarda en un repositorio (Docker Hub, GitHub Packages, etc.).
* Permite desplegar a ambientes (staging, QA, producción) con un solo clic o comando.

{: .highlight }
> Ejemplo:
> El pipeline crea la imagen Docker de tu API y la deja lista para desplegar en Kubernetes.

## Despliegue Continuo (Continuous Deployment)

> {: .important }
👉 Objetivo: que el sistema se despliegue automáticamente tras superar todos los tests.

Diferencia:

* ***Delivery*** → requiere aprobación humana.

* ***Deployment*** → 100% automático.

{: .highlight }
> Ejemplo:
> Cada vez que pasas las pruebas en main, Jenkins o GitHub Actions despliega directamente en Kubernetes.

## Heramientas clave (gratuitas)

| Etapa                    | Herramientas gratuitas comunes |
| ------------------------ | ------------------------------ |
| **Código & Repositorio** | GitHub, GitLab                 |
| **Integración continua** | GitHub Actions, Jenkins        |
| **Calidad del código**   | SonarQube                      |
| **Contenedores**         | Docker                         |
| **Despliegue**           | Kubernetes (Minikube, k3s)     |
| **Monitoreo**            | Prometheus, Grafana            |

## Comparativas

| Característica              | **Integración Continua (CI)**                                | **Entrega Continua (Continuous Delivery)**                                 | **Despliegue Continuo (Continuous Deployment)**             |
| --------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------- | ----------------------------------------------------------- |
| **Objetivo principal**      | Detectar errores rápido integrando código frecuentemente.    | Tener el software siempre listo para desplegar.                            | Automatizar completamente el despliegue a producción.       |
| **Automatiza**              | Compilación, pruebas unitarias y validaciones de código.     | Construcción de artefactos y despliegue a ambientes intermedios (staging). | Todo el flujo, desde el commit hasta producción.            |
| **Desencadenante**          | Cada *commit* o *pull request*.                              | Paso exitoso del pipeline de CI.                                           | Paso exitoso del pipeline de CI y CD.                       |
| **Validaciones comunes**    | Compilación, tests unitarios, análisis estático (SonarQube). | Tests funcionales, integración, QA.                                        | Monitoreo post-despliegue, rollback automático.             |
| **Resultado esperado**      | Código validado y estable en el repositorio.                 | Versión lista para desplegar (ej. imagen Docker).                          | Aplicación desplegada automáticamente en producción.        |
| **Nivel de automatización** | Alto, pero limitado a build y test.                          | Parcial (requiere aprobación manual).                                      | Total (sin intervención humana).                            |
| **Beneficio principal**     | Detección temprana de fallos.                                | Entregas frecuentes y predecibles.                                         | Entrega continua sin fricción ni tiempos muertos.           |
| **Ejemplo práctico**        | Ejecutar tests en GitHub Actions o Jenkins al hacer commit.  | Jenkins genera una imagen Docker lista para staging.                       | Jenkins despliega automáticamente esa imagen en Kubernetes. |
