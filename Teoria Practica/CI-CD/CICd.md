---
title: Teoria
layout: home
parent: CiCd
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
👉 Objetivo: detectar errores rápido y mantener el código siempre “listo para entregar”.

Qué implica:

* Cada vez que un desarrollador sube código (push o pull request), se dispara un pipeline automático.
* Ese pipeline compila, ejecuta pruebas unitarias, y valida calidad de código.
* Si algo falla, el equipo lo sabe al instante.

> **Beneficio:** evitas el clásico “funciona en mi máquina”.

{: .highlight }
Ejemplo: Cada commit en tu repo ejecuta pruebas automáticas en GitHub Actions o Jenkins.

## Entrega Continua (CD – Continuous Delivery)

{: .important }
👉 Objetivo: tener siempre una versión lista para desplegar.

Qué hace:

* Después de pasar CI, el sistema genera artefactos (por ejemplo, imágenes Docker).
*Los guarda en un repositorio (Docker Hub, GitHub Packages, etc.).
* Permite desplegar a ambientes (staging, QA, producción) con un solo clic o comando.

{: .highlight }
> Ejemplo:
> El pipeline crea la imagen Docker de tu API y la deja lista para desplegar en Kubernetes.

## Despliegue Continuo (Continuous Deployment)

{: .important }
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

