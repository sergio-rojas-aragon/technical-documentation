---
title: Pruebas Unitarias
layout: home
parent: DotNet
nav_order: 6
---

# Pruebas Unitarias
{: .no_toc }

{: .fs-6 .fw-70 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# Asserts

* **Validar Lista Vacia**

    ```csharp
    // lista vacía
    Assert.NotNull(result);
    Assert.Empty(result);
    ```

* **Validar lista no vacia**

    ```csharp
    // lista NO vacía
    Assert.NotNull(result);
    Assert.NotEmpty(result);
    ```