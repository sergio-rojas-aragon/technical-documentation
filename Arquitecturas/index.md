---
title: Arquitecturas
layout: home
nav_order: 23
---

# Clean Architecture
{: .no_toc }

Clean Architecture desde Cero, una explicacion mas sencilla a como se enseña tecnicamente para entender mejor los conceptos.
{: .fs-6 .fw-100 }

Finalmente esto se aplicara en un desarrollo en .net8

{: .no_toc .text-delta }

1. TOC
{:toc}


## Introduccion

PARTE 1: ¿Qué problema queremos resolver?

Antes de hablar de arquitectura, pensemos en el problema que resuelven estas ideas.

Imagina que estás construyendo una aplicación (por ejemplo, una app para gestionar pedidos en un restaurante 🍔).
Con el tiempo, el código se vuelve difícil de entender:

* Hay lógica de negocio mezclada con código de base de datos.
* Los cambios en una parte rompen otras.
* No sabes dónde poner cada cosa nueva.

👉 DDD y Clean Architecture buscan organizar el código para:

* Mantener la lógica de negocio limpia y aislada.
* Permitir cambiar la base de datos o frameworks sin reescribir todo.
* Tener un código modular, fácil de probar y mantener.

## Qué es DDD (Domain-Driven Design)

DDD = Diseño guiado por el dominio.

“Dominio” = el problema principal que tu software resuelve.

Ejemplo:

En una app de pedidos, el dominio es cómo se crean, pagan, y entregan los pedidos.
No es la base de datos, ni la API, ni la interfaz.
Es el corazón del negocio.

📚 Objetivo de DDD:

Centrar el diseño del software en el lenguaje y reglas del negocio, no en los detalles técnicos.

| Concepto                              | Qué es                                                      | Ejemplo                                  |
| ------------------------------------- | ----------------------------------------------------------- | ---------------------------------------- |
| **Entidad (Entity)**                  | Un objeto con identidad propia                              | Un `Pedido` o un `Cliente`               |
| **Valor (Value Object)**              | Un dato sin identidad, definido por su valor                | Un `Email`, una `Dirección`, un `Precio` |
| **Agregado (Aggregate)**              | Conjunto de entidades que se gestionan como una sola unidad | Un `Pedido` con sus `Items`              |
| **Repositorio (Repository)**          | Abstracción para guardar/leer objetos del dominio           | `PedidoRepository` guarda pedidos en BD  |
| **Servicio de dominio**               | Lógica de negocio que no encaja en una sola entidad         | “Calcular total del pedido”              |
| **Caso de uso / Application Service** | Ejecuta una acción del sistema                              | “Crear pedido”, “Pagar pedido”           |



## Dominio 


Es donde vive la lógica de negocio pura, libre de frameworks, bases de datos o UI.

```
/Domain
│
├── Entities/
│   ├── CuentaBancaria.cs
│   ├── Cliente.cs
│   └── Transaccion.cs
│
├── ValueObjects/
│   ├── Dinero.cs
│   └── Email.cs
│
├── Events/
│   ├── TransaccionRealizadaEvent.cs
│
├── Services/
│   ├── ServicioTransferencia.cs
│
├── Interfaces/
│   ├── ICuentaBancariaRepository.cs
│
└── Exceptions/
    ├── SaldoInsuficienteException.cs
```

