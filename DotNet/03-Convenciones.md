---
title: Convenciones
layout: home
parent: DotNet
nav_order: 3
---

# Convenciones de Nomenclatura Clave en .NET (C#)


| Identificador | Convención | Uso Típico | Ejemplo |
| :--- | :--- | :--- | :--- |
| **Clases, Structs, Enums** | `PascalCase` | Estructuras de datos y tipos de valor. | `Producto`, `DetalleFactura`, `TipoUsuario` |
| **Interfaces** | `PascalCase` con prefijo `I` | Define contratos para clases (servicios, repositorios). | `IUserService`, `IRepository`, `IDisposable` |
| **Métodos** | `PascalCase` | Acciones o funcionalidades de una clase. | `CalcularTotal()`, `ObtenerPorId()`, `GuardarCambios()` |
| **Propiedades Públicas** | `PascalCase` | Miembros públicos que exponen datos de la clase. | `NombreCompleto`, `StockDisponible`, `FechaCreacion` |
| **Controladores (ASP.NET)** | `PascalCase` con sufijo `Controller` | Clases que manejan solicitudes HTTP. | `UsersController`, `ProductsController` |
| **Servicios / Lógica de Negocio** | `PascalCase` con sufijo `Service` (o sin sufijo) | Clases que contienen la lógica de negocio. | `UserService`, `ProductManagementService` |
| **Variables Locales** | `camelCase` | Variables declaradas dentro de un método. | `totalCuenta`, `cantidadItems` |
| **Parámetros de Métodos** | `camelCase` | Argumentos pasados a un método. | `usuarioId`, `nuevoNombre` |
| **Campos Privados** | `_camelCase` | Variables internas privadas de una clase (campos de apoyo). | `_dbContext`, `_logManager`, `_nombreUsuario` |
| **Constantes (`const`)** | `PascalCase` | Valores inmutables. | `MaximoIntentos`, `MensajeErrorGenerico` |
| **Nombres de Archivos / Carpetas** | `PascalCase` | Organización de la estructura del proyecto. | `Controllers/`, `Services/`, `User.cs` |


Puntos Clave Adicionales:

* **Interfaces (I prefijo):** La convención de usar I como prefijo para las interfaces es una regla estricta en .NET, lo que ayuda a diferenciarlas inmediatamente de las clases (ej. IProductService vs. ProductService).
* **Sufijos:** Utilizar sufijos como Controller, Service, Repository o Manager es una práctica estándar que comunica el rol del componente en la arquitectura de la aplicación.
* **Abreviaturas:** Se recomienda evitar las abreviaturas a menos que sean muy conocidas y estándar en la industria (ej. HTML, XML, UI). Si se usan, deben seguir PascalCase o camelCase según la convención (ej. GetHtml() no GetHTML()).

